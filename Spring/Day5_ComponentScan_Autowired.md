# 1. @ComponentScan

이 전까지 모든 의존관계 주입을 `Config.class`파일을 만들어서 하나하나 의존관계를 주입시켜 줬다. 지금처럼 예제를 위한 코드에서는 몇 줄 되지 않는 코드의 양이지만 스프링 빈이 수십, 수백개가 되면 일일이 등록하기 힘들기도하고, 관리하기도 힘들 것이다. 또한 누락하는 문제도 발생할 수 있다.

스프링에서는 이 문제를 해결하기 위해 설정 정보가 없어도 자동으로 스프링 빈을 등록하는 `@ComponentScan`기능을 제공한다.

## 1. @ComponentScan 사용 전 코드

```java
@Configuration
public class AppConfig {
		@Bean
        public MemberService memberService() {
            return new MemberServiceImpl(memberRepository());
        }
		@Bean
        public OrderService orderService() {
            return new OrderServiceImpl(memberRepository(), discountPolicy());
        }
		@Bean
        public MemberRepository memberRepository() {
            return new MemoryMemberRepository();
        }
		@Bean
        public DiscountPolicy discountPolicy() {
            return new FixDiscountPolicy();
        }
}
```

## 2. @ComponentScan 사용 후 코드

**| AppConfig**

```java
@Configuration
@ComponentScan
public class AppConfig {}
```

정말 간단하게도 `@Bean`태그를 사용하지 않고 스프링 빈이 스프링 컨테이너에 등록되게 된다.

`@ComponentScan`을 사용하면 구성 클래스에서 직접 의존 관계들을 작성하지 않고 스프링 빈으로 등록할 클래스들에 `@Component`를 붙여주면 된다. `@Component`대신 컨트롤러(API계층), 서비스(비즈니스 로직)에 따라 `@Controller`, `@Service`, `@Repository`애너테이션을 붙이기도 한다.

아래와 같이 애너테이션을 추가해주면 된다.

**| MemoryMemberRepository**

```java
@Repository
public class MemoryMemberRepository implements MemberRepository {}
```

**| RateDiscountPolicy**

```java
@Component
public class RateDiscountPolicy implements DiscountPolicy {}
```

**| MemberServiceImpl**

```java
@Service
public class MemberServiceImpl implements MemberService {}
```

> 💡 **@Controller, @Service, @Repository를 @Component대신 사용가능한 이유** > ![](https://velog.velcdn.com/images/jgone2/post/bd08ec98-3db7-44a9-a3d8-6547321b4338/image.png)
> 인터페이스 `Service`를 대표로 보면 `@Service`인터페이스에 @Component기능이 포함되어 있는 것을 볼 수 있다. `@Repository`와 `@Controller`에도 @Component기능이 포함되어 있어 각 계층별로 구분할 수 있는 애너테이션을 사용하여 더 쉽게 관리할 수 있다.

# 2. @Autowired

`@ComponentScan`을 사용해서 `@Bean`을 사용하지 않고 스프링 컨테이너에 스프링 빈을 등록했었다. 하지만 의존성 주입을 하는 코드도 함께 사라졌다. `@Autowired`를 사용하면 이 문제를 해결할 수 있다.

`@Autowired`를 붙이면 구성 클래스에서 직접적으로 의존을 주입하지 않아도 되고 스프링이 해당 타입의 빈 객체를 찾아서 필드에 할당해준다.

다음과 같이 애너테이션을 입력해주면 된다.

**| OrderServiceImpl**

```java
@Service
public class OrderServiceImpl implements OrderService {
	private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;

    @Autowired
    public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
    	this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }
}
```

위의 예제처럼 `@Autowired`를 지정하면, 생성자에 파라미터가 여러개여도 자동으로 스프링이 빈 객체를 찾아서 자동으로 주입해준다.
