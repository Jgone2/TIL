# 1. Java → Spring으로의 전환

앞서 IoC와 DI에서 자바코드를 이용해서 의존성 주입을 진행했다. 여기서 스프링을 사용하는 코드 변경과정을 작성해 보겠다.

AppConfig는 비즈니스로직을 작성하지 않는 코드 전반의 환경구성을 지정하는 파일이었다. 우리는 이런 사용분야를 Spring에 알려줘야하는데 `애너테이션(Annotation)`을 이용해서 알려줄 수 있다. 먼저 AppConfig를 다시 사용해서 코드 변경을 진행해보겠다.
**| AppConfig**

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

- `@configuration`: 설정을 구성한다는 의미를 가진 Annotation
- `@Bean`: @Bean으로 등록된 설정정보를 스프링 컨테이너에 등록

위와 같이 `애너테이션(Annotation)`을 추가해주면 Spring에서 'AppConfig는 설정구성 파일이구나'를 인지할 수 있게 된다. 그리고 @Bean을 사용해서 해당 애너테이션이 적인 메서드를 모두 호출해서 반환된 객체를 스프링 컨테이너에 등록하게 된다.

# 2. 스프링 컨테이너(Spring Container)

스프링 컨테이너는 `@Configuration`애너테이션을 참조하여 설정 클래스를 인지하고 설정 클래스에서 정보를 참고해서 `빈의 생성, 관계 설정 등의 제어작업을 총괄하는 컨테이너`이며, `IoC컨테이너`라고도 말한다.

`ApplicationContext`가 바로 스프링 컨테이너이다. 그렇기 때문에 bean으로 등록된 설정정보는 ApplicationContext를 통해 설정정보를 가져올 수 있다.

`ApplicationContext`는 `BeanFactory` 인터페이스에서 기능을 상속받아 사용하는데, `BeanFactory`에서 상속받은 기능을 포함하여 그 외에 부가기능까지 제공한다. 제공하는 부가기능은 다음과 같은 종류가 있다.

> 💡 **ApplicationContext에서 제공하는 부가기능**

- **MessageSource**: 메세지 다국화를 위한 인터페이스
- **EnvironmentCapable**: 개발, 운영 등 환경변수 등으로 나눠 처리하고, 애플리케이션 구동 시 필요한 정보들을 관리하기 위한 인터페이스
- **ApplicationEventPublisher**: 이벤트 관련 기능을 제공하는 인터페이스
- **ResourceLoader**: 파일, 클래스 패스, 외부 등 리소스를 편리하게 조회

또한, BeanFactory에 대해서 간단하게 정리하면 다음과 같다.

> 💡 **BeanFacory**

- 스프링 컨테이너의 최상위 인터페이스
- 빈을 등록, 생성, 조회, 반환 등 빈을 관리하는 역할
- `getBean()` 메소드를 통해 빈의 인스턴스화 가능
- `@Bean`애너테이션이 붙은 메서드의 명을 스프링 빈의 이름으로 사용해 스프링 컨테이너에 등록
- 스프링 컨테이너의 종류를 이야기하면 `BeanFactory`, `ApplicationContext`로 구분해서 이야기 하지만 `BeanFacotry`를 직접적으로 사용하는 경우는 거의 없다고 한다.

정리해서 스프링 컨테이너는 인터페이스인 `ApplicationContext`로 Bean의 생성, 관리, 등록 등의 역할을 수행한다. 이때 Bean과 관련된 기능들은 스프링 컨테이너의 최상위 인터페이스인 `BeanFacotry`의 기능을 상속받아 사용하고 그 외에도 여러 인터페이스의 기능을 상속받아 제공하는 부가기능도 존재한다. 라고 할 수 있겠다.

## 1. 스프링 컨테이너 생성

스프링 컨테이너는 `애너테이션` 방식과 `XML`방식으로 생성할 수 있다. 생성 방법은 다음과 같다.

```java
// Annotation
ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);

// XML
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");
```

### 1. 스프링 컨테이너의 생성

스프링 컨테이너를 애너테이션 방식으로 생성을 하면 `new AnnotationConfigApplicationContext(AppConfig.class)`로 생성하면 `AppConfig.class`를 구성 정보로 지정해서 `Collection Framework`처럼 `Key(Bean Name)`와 `Value(Bean 객체)`형식으로 스프링 컨테이너 내부에 스프링 빈 저장소가 생성된다.

![](https://velog.velcdn.com/images/jgone2/post/e23e8982-092f-4c76-a2ae-7cf27aac4588/image.png)

여기에 @Bean으로 등록된 `메서드명이 빈 이름`으로 `리턴되는 객체가 빈 객체`로 저장된다.

## 2. 스프링 컨테이너 사용과 미사용의 비교

먼저 예제는 앞서 계속 사용하던 OrderService를 사용해서 Test를 사용하는데 있어서 스프링 컨테이너를 사용했을 때와 사용하지 않았을 때의 코드비교를 해보겠다.
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

**| 스프링 컨테이너 사용 전**

```java
class OrderServiceTest {

    MemberService memberService;
    OrderService orderService;

    @BeforeEach
    public void beforeEach() {
    	// 구성정보 생성 및 할당
        AppConfig appconfig = new AppConfig();
        // 구성정보 등록된 구성정보명(?)을 가져옴.
        memberService = appconfig.memberService();
        orderService = appconfig.orderService();
    }

    @Test
    void createOrder() {
        ...
    }
```

**| 스프링 컨테이너 사용 후**

```java
class OrderServiceTest {

    MemberService memberService;
    OrderService orderService;

    @BeforeEach
    public void beforeEach() {
    	// 스프링 커넽이너 생성
        ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);
        // 컨테이너에 등록된 정보 가져옴.
        memberService = ac.getBean("memberService", MemberService.class);
        orderService = ac.getBean("orderService", OrderService.class);
    }

    @Test
    void createOrder() {
        ...
    }
}
```

# 3. 스프링 빈(Spring Bean)

`스프링 빈(Spring Bean)`은 `스프링 컨테이너에서 인스턴스화된 객체`를 의미한다. 빈은 클래스의 등록정보, 및 포함된 메서드의 정보를 가지고 있고 컨테이너에서 사용되는 `설정메타 데이터`로 생성된다.

## 1. 빈의 접근 방식

빈의 접근 방식은 스프링 컨테이너의 사용/미사용의 비교에서 이미 접근 방식을 사용해봤다.

```java
// 스프링 컨테이너 생성
ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);
// getBean()으로 bean에 접근
memberService = ac.getBean("memberService", MemberService.class);
orderService = ac.getBean("orderService", OrderService.class);
```

위의 예제에서 스프링 컨테이너를 생성하고 스프링 컨테이너 ac에서 getBean()을 사용해서 스프링 빈 저장소에있는 빈에 접근할 수 있다.

## 2. 스프링 컨테이너에서의 빈 저장

앞서 언급했듯이 스프링 컨테이너에서 스프링 빈 저장소에 빈의 정보를 저장할때 `Key(Bean Name)`와 `Value(Bean 객체)`형식으로 빈 저장소에 구성정보를 저장하며, @Bean으로 등록된 `메서드명이 빈 이름`으로 `리턴되는 객체가 빈 객체`로 저장된다.

여기에 추가로 빈 이름은 @Bean(name="")을 사용하여 빈 이름을 변경할 수 있으면 스프링 빈 저장소에도 변경한 빈 이름으로 저장된다.

하지만 빈 이름은 `Collection Framework`에서 `key`처럼 중복이 되면 안된다. 같은 이름을 부여하면 중복된 빈 중에 하나만 인식이 되거나 내용을 덮어버리는 등의 오류가 발생할 수 있다.

```java
@Bean(name="ThisIsExample")
public Member memberService() {
	return MemberServiceImpl(memoryRepository());
}
```

위와 같은 예제로 스프링 컨테이너를 생성하면 스프링 빈 저장소에는 다음과 같이 저장된다.
![](https://velog.velcdn.com/images/jgone2/post/ad0a9d53-8283-4411-a007-b2e69129db12/image.png)

## 3. Bean생성 시점

스프링 빈의 생성시점은 크게 두가지로 나눌 수 있다.

### 1. Lazy Loading - BeanFactory

- On-Demand 방식으로 빈을 사용할 때 로딩
- 실제로 빈을 사용할 때 로딩하기 때문에 가벼운 컨테이너라고 할 수 있다.
- 빈을 사용할 때 로딩하기 때문에 해당 Bean을 사용하기 전까지는 오류의 유무를 확인할 수 ❌

### 2. Eager Loading - ApplicationContext

- 런타임 실행 시 모든 빈을 로딩
- 문제가 있는 Bean 객체가 있을 때 오류의 유무 미리 확인 ⭕️
- 컨테이너는 런타임 실행 시점에 등록된 빈을 모두 로딩해야하기 때문에 부담↑
- BeanFactory의 Lazy Loading을 사용할 수 있다.

#### ApplicationContext에서 Lazy Loading 사용법

**| 방법1**

```java
@Configuration
@Lazy
public class AppConfig {
	...
}
```

**| 방법2**

```java
@Configuration
public class AppConfig {
	@Bean
    @Lazy
    public ...() {}
}
```

**| 방법3**

```java
<beans default-lazy-init="true">
```

## 4. BeanDefinition - 스프링 빈 설정메타데이터

스프링에서 다양한 설정 형식을 지원할 수 있는 이유는 인터페이스 `BeanDefinition`이라는 추상화가 존재하기 때문이다.

스프링 컨테이너는 이 `BeanDefinition`이라는 추상화를 통해 메타정보를 얻고 이 정보를 기반으로 스프링 빈을 생성한다.

추상화를 이용하기 때문에 DI를 활용할때처럼 구현체인 스플이 컨테이너는 bean이 `XML`로 구성되어 있는지 `Annotation`으로 구성되어 있는지 몰라도 된다. `BeanDefinition`에서 `@Bean`, `<bean>`상관없이 각각 하나의 메타 정보를 생성해주기 때문이다.

BeanDefinition에서는 `BeanDefinitionReader`를 사용해서 각 구성정보를 읽어올 수 있다.

## 1. BeanDefinition 정보

- `BeanClassName`: 생성할 빈의 클래스명(자바 설정처럼 팩토리 역할의 빈을 사용하면 없음)
- `facoryBeanName`: 팩토리 역할의 빈을 사용할 경우 이름, 예)appConfig
- `factoryMethodName`: 빈을 생성할 팩토리 메서드 지정, 예)memberService
- `Scope`: 싱글톤(default)
- `lazyInit`: 스프링 컨테이너를 생ㅇ성할 때 빈을 생성하는 것이 아니라, 실제 빈을 사용할 때까지 최대한 생성을 지연처리 하는지 여부
- `InitMethodName`: 빈을 생성하고, 의존관계를 적용한 뒤에 호출되는 초기화 메서드 명
- `DestroyMethodName`: 빈의 생명주기가 끝나서 제거하기 직전에 호출되는 메서드 명
- `Constructor arguments, Properties`: 의존관계 주입에서 사용(자바 설정처럼 팩토리 역할의 빈을 사용하면 없음)

## 2. BeanDefinition 역할

- `getRole()` : 빈의 역할을 반환
- `ROLE_APPLICATION` : 직접 등록한 애플리케이션 빈
- `ROLE_INFRASTRUCTURE` : 스프링이 내부에서 사용하는 빈
- `ROLE_SUPPORT` : 지원하는 빈
- `ROLE_ANNOTATION` : 애노테이션을 통해 등록한 빈
- `ROLE_INDIRECT` : 직접 등록하지 않은 빈

# 📚 Reference

- [Infrean - 김영한님 스프링 로드맵](https://www.inflearn.com/roadmaps/373)
- [코드스테이츠 백엔드 부트캠프](https://www.codestates.com/course/backend-engineering)
