# 1. Singleton Pattern

`Singleton Pattern`은 인스턴스가 1개만 생성되는 것을 보장하는 자바의 디자인 패턴 중 하나다. 싱글톤 패턴을 유지하려면 인스턴스를 1개 이상 생성하지 못하도록 방지해야하기 때문에 `private`생성자를 사용해서 new 키워드의 사용을 막아야한다.

**| SingletonExam**

```java
private static final StingletonExam instance = new SingletonService();
// public으로 지정해서 인스턴스가 필요하면 이 메서드를 통해서만 조회가능하도록 허용
public static SingletonService getInstance() {
	return instance;
}
// 생성자 private를 사용하여 외부에서 new 키워드를 사용해서 객체 생성 불가
private SingletonService(){}
```

위의 코드를 보면 private생성자로 SingletonService의 인스턴스를 생성하고 생성자 또한 private생성자를 사용해서 구현함으로써 외부에서 SingletonExam을 생성해서 참조하지 못하도록 설계되어 있다.

SingletonService를 사용하기 위해서는 이미 SIngletonExam클래스에서 생성된 생성자를 `getInstance()`로 참조해서 사용을 해야한다.

위와 같이 싱글톤 패턴을 사용하면 여러 사용자가 요청을 하면 요청받은 수 많큼 인스턴스화 하는 것이 아닌 이미 생성된 인스턴스를 참조하게 만든다.

그러나 위의 싱글톤 패턴에도 여러가지 문제점이 존재하는데 다음과 같은 문제점이 존재한다.

## 1. 문제점

- 싱글톤 패턴 구현을 위한 코드를 따로 작성해야함.(구현 코드 자체가 길다)
- 의존관계 상 클라이언트가 구현체 클래스에 의존(class.getInstance()) → DIP위반
- 클라이언트가 구현체 클래스에 의존해서 OCP원칙 위반 가능성 ⭕️
- 테스트의 어려움(인스턴스가 이미 생성되어 설정이 되어 있음)
- 내부 속성을 변경하거나 초기화 어려움
- private 생성자로 자식 클래스 생성 어려움
- 로직의 유연성 ↓

자바에서 싱글톤 패턴은 위와 같은 문제점들을 지니고 있다. 하지만 스프링에서는 이런 싱글톤 패턴의 문제점들을 해결할 수 있다.

# 2. 싱글톤 컨테이너(Singleton Container)

스프링에서 싱글톤 컨테이너를 사용하면 싱글톤 패턴의 문제점을 해결하고, 싱글톤 패턴의 장점인 인스턴스를 오직 1개로 유지시켜 줄 수 있다.

스프링 컨테이너에서는 애너테이션하나로 `자동으로 싱글톤으로 인스턴스를 관리`해준다. 또한 스프링 컨테이너가 싱글톤 컨테이너역할을 수행하며 이런 기능(싱글톤 인스턴스 생성 및 관리)을 `싱글톤 레지스트리`라고 한다. 이렇게 스프링에서 `자동으로 싱글톤 패턴을 유지`시켜주기 때문에 싱글톤 패턴의 단점들(원칙 위반 가능성, 낮은 유연성 등)을 지우고 장점만 사용할 수 있게 됐다.

싱글톤 방식은 하나의 인스턴스를 공유하기 때문에 무상태(stateless)로 설계해야한다. 특정 클라이언트에 의존적인 필드가 있으면 안되며, 값을 변경할 수 있는 필드도 마찬가지로 존재하면 안된다. 가급적 `read-only`방식으로만 지원되어야 하며 필드가 아닌 공유되지 않는 지역변수, 파라미터, ThreadLocal 등을 사용해야한다.

> 💡**ThreadLocal**
> 찾아보고 적어보자

만약 스프링 빈의 필드에 공유 값을 설정하게 되면 동시 참조가 되고 있을 때 값을 수정하고 저장했을 때 늦게 저장된 값으로 바로 덮어져 큰 장애가 발생할 수 있다.

```java
public static class TestConfig {
	@Bean
	public ... Service() {
		return new Service();
	}
}
```

만약 위와 같이 bean이 등록되어 있을 때 싱글톤 패턴을 사용해서 값을 수정하는 Test코드를 작성해보았다.

```java
@Test
void test() {
	ApplicationContext ac = new AnnotationConfigApplicationContext(TestConfig.class);
    Service service1 = ac.getBean(Service.class);
    Service service2 = ac.getBean(Service.class);

	int userKim = service1.order("Kim", 1000);
    int userLee = service2.order("Lee", 2000);
    assertEquals(userKim, 2000);
}
```

위의 코드에서 order()에서 price값이 return이 된다고 가정했을 때 테스트를 수행하면 정상적으로 test가 수행될 것이다. 왜냐하면 싱글톤으로 유지 되고 있기 때문에 price값이 2000으로 변경되었기 때문이다.

그러기 때문에 싱글톤 방식을 사용할 때는 주의를 기울여 사용해야한다.

> 💡 **싱글톤 방식의 주의점**

- 특정 클라이언트가 값을 변경/설정할 수 있으면 안된다.
- read-only방식이어야한다.
- 스프링 빈의 공유 값을 설정하면 장애가 발생한다.

# 3. @Configuration

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

자바코드에서 스프링 컨테이너를 사용한 코드를 변환했을 때 사용했떤 AppConfig예제다. 위의 코드를 보면 `new MemoryMemberRespository()`는 `memberService()`가 호출될 때,`orderService()`가 호출될 때 이렇게 두번 호출되어야 한다.

```java
public MemberRepository getMemberRepo() {
	return memberRepository;
}
```

그러나 `memberRespository`를 호출하는 두 메서드에 아래 코드를 삽입하고 다음과 같은 테스트 케이스를 작성하면 테스트 케이스가 정상적으로 종료된다.

```java
ApplicationContext ac = new
  AnnotationConfigApplicationContext(AppConfig.class);
          MemberServiceImpl memberService = ac.getBean("memberService",
  MemberServiceImpl.class);
          OrderServiceImpl orderService = ac.getBean("orderService",
  OrderServiceImpl.class);
assertThat(memberService.getMemberRepo()).isSameAs(orderService.getMemberRepo());
```

위와 같은 테스트 코드를 작성하고 로그를 만약에 작성해서 로그를 보면 memberRepository 인스턴스는 하나로 공유되어 사용되고 있음을 알 수 있을 것이다. 즉, `new MemoryMemberRepository`가 한번만 수행된다는 것을 알 수 있다. 어떻게 @Configuration과 @Bean만 추가했을 뿐인데 싱글톤을 유지시켜 줄 수 있을까?

## 1. 스프링 컨테이너가 싱글톤을 자동으로 유지시켜 줄 수 있는 이유 - 바이트 코드 조작(feat.@Configuration)

위의 예제에서 아래 assertThat구문을 실행사면 getMemberRepo()를 실행하면 `new MemoryMemberRepository`가 싱글톤으로 유지 되는 것을 알 수 있다.

```java
assertThat(memberService.getMemberRepository()).isSameAs(orderService.getMemberRepository());
```

이것이 가능한 이유는 스프링에서 클래스의 바이트코드를 조작하는 라이브러리를 사용하기 때문인데, 이 바이트 코드는 `@Configuration`를 적용한 구성클래스에서 사용된다.

### 1. 바이트 코드 조작

`@Configuration`를 적용한 `Config`파일에서 클래스의 정보를 출력해보면 다음과 같이 출력된다.

**| ConfigurationSingletonTest**

```java
@Test
void configurationClassTest() {
    ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);
    AppConfig bean = ac.getBean(AppConfig.class);

    System.out.println("bean = " + bean.getClass());
}
```

**| 출력 값**

```java
bean = class com.example.hellospring.AppConfig$$EnhancerBySpringCGLIB$$397d666
```

**| 순수 클래스 정보 출력**

```text/plain
bean = class com.example.hellospring.AppConfig
```

원래 순수 클래스의 정보를 가져 오면 `순수 클래스 정보 출력`과 같이 출력되겠지만 클래스명에 `CGLIB$$*`가 추가되어 출력된 것을 볼 수 있다.

이것은 스프링이 `CGLIB`라는 바이트코드 조작 라이브러리를 사용해서 `Config`클래스를 상속받은 임의의 다른 클래스를 생성하고, 이 클래스를 스프링 빈으로 등록한 것이다.

스프링 컨테이너에는 `Config@CGLIB$$*`클래스가 등록이 되어 사용되겠지만, `Config`를 상속받은 클래스이기 때문에 우리가 직접 생성한 `Config`타입으로 똑같이 조회를 할 수 있다.

이것을 사용해서 스프링에서 싱글톤의 장점만을 사용하기 위해서 구성정보에 항상 `@Configuration`애너테이션을 사용하면 된다.

# 📚 Reference

- [Infrean - 김영한님 스프링 로드맵](https://www.inflearn.com/roadmaps/373)
- [코드스테이츠 백엔드 부트캠프](https://www.codestates.com/course/backend-engineering)
