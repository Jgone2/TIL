# 1. IoC(Inversion of Control)

`IoC`는 프로그램에서 제어 흐름을 반전시키는 것을 중심으로하는 소프트웨어 엔지니어링의 설계 원칙이라고 한다. 제어 흐름의 반전을 `의존성 주입(DI, Dependency Injection)`라고도 하는데, 클래스 간의 종속성에 의한 제어흐름을 개발자가 결정(흔히, 하드코딩 방식)하는 것이아니라 외부에 의해 주입받아 결정되는 것을 말한다.

이것을 짧게 말해서 `제어의 역전`이라고 말할 수 있다. IoC의 방식으로 제어흐름을 역전시키면 역할과 구현을 분리시켜 **객체 간의 결합도를 줄이고 유연한 코드 작성이 가능**하다. 또한 결합도가 줄어들게 되면 **코드중복 해소와 유지보수를 용이**하게 할 수 있다는 장점이 있다.

`IoC`의 구성요소에는 다음과 같은 4가지가 있다.

- 1. **Bean**
  - Spring IoC 컨테이너에서 관리되는 객체
  - Java 클래스의 인스턴스
  - 인스턴스화하여 스프링 컨테이너에 등록된 객체를 `스프링 빈`이라고 함.
  - Spring IoC 컨테이너는 Bean의 수명 주기를 생성, 구성 및 관리
- 2. **ApplicationContext**
  - Spring IoC 컨테이너의 중앙 인터페이스
  - bean의 구성, 생성 및 검색을 제공
- 3. **Configuration**
  - Spring 설정 클래스로 지정
  - 컨테이너에 빈을 생성하고 연결할 수 있도록 함.
  - `@Configuration` 애너테이션을 사용
- 4. **의존성 주입(DI)**
  - 스프링 컨테이너가 bean끼리의 의존성을 관리하기 위해 사용하는 기술
  - 생성자 주입, 세터 주입, 필드 주입 등 다양한 종류 존재

# 2. 의존성 주입 - DI(Dependency Injection)

의존성 주입은`IoC`의 내용을 구체화 시킨 것으로 객체 간의 결속력을 낮추는 역할을 수행한다. IoC와 DI의 차이점은 IoC는 위에서 언급했듯이 제어의 역전을 위한 원칙이며, DI는 IoC의 목적을 달성하기위한 디자인 패턴 중 하나다. 여기서 의존성이란 특정 클래스의 구성이 변경될 때 연관있는 다른 클래스에서도 영향을 받는 것을 의미한다.

의존성 주입의 방법에는 생성자 주입, Setter주입 등이 있다. 스프링에서는 생성자 주입을 권장하고 있으며, 생성자 주입을 할 때 인터페이스를 상속받아 구현하면 의존성 주입을 할 객체들, 즉 생성자 파라미터를 강제화 할 수 있다. 때문에 DI를 구현할 때 인터페이스를 활용한 생성자 주입을 사용하는 것이 좋다고 생각된다.

먼저 클래스 간 결합도가 높은 예제에서부터 의존성 주입을 사용하여 결합도를 점점 낮춰보는 식으로 진행하려한다.

**| OrderServiceImpl**

```java
public class OrderServiceImpl implements OrderService {
	private final MemberRepository memberRepository = new MemoryMemberRepository();
    private final FixDiscountPolicy discountPolicy = new FixDiscountPolicy();

	@Override
    public Order createOrder(Long memberId, String itemName, int itemPrice) {
        Member member = memberRepository.findById(memberId);
        int discountPrice = discountPolicy.discount(member, itemPrice);
        return new Order(memberId, itemName, itemPrice, discountPrice);
    }
}
```

**| FixDiscountPolicy**

```java
public class FixDiscountPolicy {

    private int discountFixAmount = 1000; // 1000원 할인

    public int discount(Member member, int price) {
    	return discountFixAmount;
	}
}
```

> 💡 **What is 'Impl'?**
> 인터페이스를 구현한 클래스가 1개라면 인터페이스명+Impl로 클래스명을 구성하는 것이 관례라고 한다.

위의 예제에서는 우리가 어떤 종류의 클래스를 상속하는지 비즈니스로직이 있는 `OrderServiceImpl`에서 고정값으로 할인정책을 한다는 정보를 알고 있다. 하지만 고정가격할인이 아닌 비율할인으로 변환을 하려면 FixDiscountPolicy로 적혀 있는 모든 곳을 수정해야 한다.

위의 예제처럼 클래스 내부에서 다른 클래스의 객체를 직접적으로 생성하게 되면 두 클래스 간에 의존 관계가 성립하게 된다. 위와 같은 상황을 객체간 결합도가 높다.라고 말할 수 있고 다음과 같이 수정할 수 있다.

**| OrderServiceImpl**

```java
public class OrderServiceImpl implements OrderService {

    private final MemberRepository memberRepository = new MemoryMemberRepository();
    private final DiscountPolicy discountPolicy = new FixDiscountPolicy();

    @Override
    public Order createOrder(Long memberId, String itemName, int itemPrice) {
        Member member = memberRepository.findById(memberId);
        int discountPrice = discountPolicy.discount(member, itemPrice);
        return new Order(memberId, itemName, itemPrice, discountPrice);
    }
}
```

**| FixDiscountPolicy**

```java
public class FixDiscountPolicy implements DiscountPolicy {

    private int discountFixAmount = 1000; // 1000원 할인

    @Override
    public int discount(Member member, int price) {
    	return discountFixAmount;
	}
}
```

위의 예제와 달라진 점이라면 `FixDiscountPolicy`클래스에서 `DiscountPolicy` 인터페이스를 만들어서 구현해서 DiscountPolicy로 다른 구현체를 만들어서 인스턴스화하는 부분만 수정해주면 된다는 점이다. 다음과 같이 말이다.

**| OrderServiceImpl**

```java
public class OrderServiceImpl implements OrderService {

    private final MemberRepository memberRepository = new MemoryMemberRepository();
    private final DiscountPolicy discountPolicy = new RateDiscountPolicy();

    ...
}
```

그렇지만 여전히 해결하지 못한 문제가 있다. OrderService에서 주문관련 역할만 구현하는 것이 아니라 할인 정책을 인지하고 있다. 추상화에 의존하고 있지만 여전히 구체화에도 의존하고 있다.

이는 DIP위반에 해당한다.

> 💡 **DIP?**
> Dependency Inversion Principle의 약자로 인터페이스에 의존해야 유연하게 구현체 변겨잉 가능하므로 추상화에 의존해야한다는 원칙이다.

어찌됐든 어디선가는 어떤 구현체를 사용할 것인지 지정을해줘야한다. 이것을 해결하기 위해 로직부분에서의 해결이 아니라 환경설정(구성)과 관련된 클래스를 생성하여 따로 설정할 수 있다. 보통 `Config`라고 파일명을 지정하는데 `WebConfig`, `AppConfig`등을 사용한다.

**| AppConfig**

```java
public class AppConfig {

        public MemberService memberService() {
            return new MemberServiceImpl(memberRepository());
        }

        public OrderService orderService() {
            return new OrderServiceImpl(memberRepository(), discountPolicy());
        }

        public MemberRepository memberRepository() {
            return new MemoryMemberRepository();
        }

        public DiscountPolicy discountPolicy() {
            return new FixDiscountPolicy();
        }
}
```

AppConfig파일에서 의존성 주입을 해주게 되면 memberServiceImpl과 OrderServiceImpl의 코드는 다음과 같이 수정해주면 된다.

**| MemberServiceImpl**

```java
public class MemberServiceImpl implements MemberService{

    private final MemberRepository memberRepository;

    public MemberServiceImpl(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }

    ...
}
```

**| OrderServiceImpl**

```java
public class OrderServiceImpl implements OrderService {
    private final MemberRepository memberRepository;
    private DiscountPolicy discountPolicy;

    public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }

    ...
}
```

이렇게 위와 같이 클래스 내부에서 참조할 클래스를 `new`키워드를 사용하여 직접 생성하지 않고, 생성자 등을 통해 외부에서 전달 받고 있으면 `의존성 주입(DI)`이 이루어 지고 있는 것이다.

위의 코드에서 `FixDiscountPolicy`를 `RateDiscountPolicy`로 변경하고 싶다면 `AppConfig`에서 아래와 같이 Fix를 Rate로만 변경하면 된다.
**| AppConfig**

```java
public class AppConfig {

		...

        public DiscountPolicy discountPolicy() {
           // return new FixDiscountPolicy();
           return new RateDiscountPolicy();
        }
}
```

# 📚 Reference

- [Infrean - 김영한님 스프링 로드맵](https://www.inflearn.com/roadmaps/373)
- [코드스테이츠 백엔드 부트캠프](https://www.codestates.com/course/backend-engineering)
- [[10분 테코톡] 오찌, 야호의 DI와 IoC](https://www.youtube.com/watch?v=8lp_nHicYd4)
