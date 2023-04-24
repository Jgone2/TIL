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

여기서 `@Service`는 특별한 처리를 하지는 않고, 핵심 비즈니스 로직의 위치를 알려주는 용도 정도로만 사용된다.

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

> 💡 **@Controller, @Service, @Repository를 @Component대신 사용가능한 이유**
> ![](https://velog.velcdn.com/images/jgone2/post/bd08ec98-3db7-44a9-a3d8-6547321b4338/image.png)
> 인터페이스 `Service`를 대표로 보면 `@Service`인터페이스에 @Component기능이 포함되어 있는 것을 볼 수 있다. `@Repository`와 `@Controller`에도 @Component기능이 포함되어 있어 각 계층별로 구분할 수 있는 애너테이션을 사용하여 더 쉽게 관리할 수 있다.

## 3. 탐색 위치와 스캔 대상

컴포넌트 스캔을 사용할 때 모든 자바 클래스를 스캔하게된다. 하지만 특정 위치만 스캔해도 작동이 가능하다면 모든 클래스를 스캔하면 비효율적이다.

```java
@ComponentScan( basePackages = "hello.core" )
```

위의 코드처럼 `basePackages`로 탐색할 패키지의 시작 위치를 지정하고 그 패키지를 포함하고 하위 패키지를 모두 탐색 한다. `basePackages`외에도 `BasePackageClasses`도 존재한다. `BasePackageClasses`는 지정한 클래스의 패키지의 위치로 설정한다.

속성명을 지정하지 않으면 `@ComponentScan`이 붙은 설정 정보가 위치한 패키지(최상단)가 시작위치로 지정된다.

## 4. ComponentScan Filter

컴포넌트 스캔은 스캔 필터를 제공한다. 필터에는 다음 두 가지 종류가 존재한다.

- `includeFilters`: 컴포넌트 스캔 대상을 추가로 지정
- `excludeFilters`: 컴포넌트 스캔에서 제외할 대상을 지정

```java
@ComponentScan(
	includeFilters = @Filter(type = FilterType.ANNOTATION, classes = Member.class),
	excludeFilters = @Filter(type = FilterType.ANNOTATION, classes = Order.class)
)
```

### FilterType 옵션

- `ANNOTATION`: Default, 애노테이션을 인식해서 동작.
- `ASSIGNABLE_TYPE`: 지정한 타입과 자식 타입을 인식해서 동작.
- `ASPECTJ`: 패턴 사용
- `REGEX`: 정규 표현식
- `CUSTOM`: `TypeFilter`이라는 인터페이스를 구현해서 처리

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

위의 예제처럼 `@Autowired`를 지정하면, 생성자에 파라미터가 여러개여도 자동으로 스프링이 빈 객체를 찾아서 자동으로 주입해준다. `Config`파일에서의 `스프링컨테이너명.getBean()`와 동일한 역할을 수행한다.

> 💡 **생성자가 딱 1개만 있으면 @Autowired를 생략해도 자동 주입된다.**

## 1. 조회 빈이 2개 이상일 때 - Error

의존관계 주입을 설정하고 해당 주입에 할당할 수 있는 빈이 2개 이상일 때 에러가 난다. 다음과 같은 상황에서 에러가 발생한다.

```java
@Component
public class FixDiscountPolicy implements DiscountPolicy {}

@Component
public class RateDiscountPolicy implements DiscountPolicy {}
```

**| 의존관계 주입**

```java
@Autowired
private DiscountPolicy discountPolicy
```

위의 코드를 실행하면 `NoUniqueBeanDefinitionException`오류가 발생한다.
다시 하위 타입으로 지정하면 DIP를 위반하게 된다.

### 1. 필드 명 매칭

`@Autowired`는 타입 매칭을 시도하고 여러 빈이 있으면 필드명, 파라미터명으로 빈 이름을 추가적으로 매핑을 시도한다.

```java
@Autowired
private DiscountPolicy discountPolicy

                ↓
@Autowired
private DiscountPolicy rateDiscountPolicy
```

### 2. @Qualifier 사용

`@Qualifier`는 추가 구분자를 붙여주는 방식으로 `@Qualifier(구분자)`로 구분자끼리 매칭해서 의존성주입을 진행할 수 있다. 만약 구분자에 해당하는 `@Qualifier`가 없을 때는 `Spring Bean`에서 구분자에 해당하는 이름을 찾는다.

만약 구분자에도, 스프링 빈에도 없다면 마찬가지로 `NoUniqueBeanDefinitionException`오류가 발생한다. 사용법은 다음과 같다.

```java
@Component
@Qualifier("mainDiscountPolicy")
public class RateDiscountPolicy implements DiscountPolicy {}

@Component
@Qualifier("fixDiscountPolicy")
public class FixDiscountPolicy implements DiscountPolicy {}
```

**| 수정자 주입**

```java
@Autowired
public void DiscountPolicy(@Qualifier("mainDiscountPolicy") DiscountPolicy discountPolicy) {
	...
}
```

**| 필드 주입(비 권장)**

```java
@Qualifier("mainDiscountPolicy") private DiscountPolicy discountPolicy;

public 구현체 명(DiscountPolicy discountPolicy) {
	this.discountPolicy = discountPolicy;
}
```

**| 생성자 파라미터 주입**

```java
@Autowired
public 구현체 명(@Qualifier("mainDiscountPolicy") DiscountPolicy discountPolicy) {
	this.discountPolicy = discountPolicy;
}
```

> 💡 **@Qualifier와 \*ArgsConstructor의 사용 불가?**
> 평소 생성자 코드를 직접 생성하는 것보다. `@RequiredArgsConstructor`나 `@AllArgsConstructor`등 `Lombok`을 사용해서 생성자의 기능을 수행하는 것을 자주 사용했다. 그러나 롬복과 필드주입형식을 융합하여 동시사용했을 때, @Qualifier는 이런 상황에서는 적용되지 않았다.  
> 아마 잘못 사용했을 수도 있고, 지원하지 않는 기능일 수도 있다. 더 알아볼 필요가 있을 것 같다.

### 3. @Primary 사용

`@Primary`는 우선순위를 정하는 방법이다. `@Qualifier`를 사용하지 않고 여러 빈이 매칭이 되면 `@Primary`가 등록된 빈이 우선권을 가지고 매칭되는 방식이다.

메인으로 사용되는 빈에 `@Primary`를, 보조로 사용되는 나머지 빈들에 `@Qualifier`를 사용해서 함께 사용할 수 있다.

```java
@Component
@Primary
public class RateDiscountPolicy implements DiscountPolicy {}
```

우선권을 위해서 `@Primary`를 사용하기 때문에 `@Qualifier`보다 당연히 우선권이 높다.

# 3. 애너테이션 직접 생성

애너테이션 직접 생성은 `@Qualifier("구분자")`를 사용하면 컴파일시 타입 체크가 안된다. 이 문제를 해결하기위해 애너테이션 직접 생성을 사용할 수 있다. 문제를 해결하면서 `@Qualifier`기능을 사용하기 위해 `@Qualifier`를 포함한 애너테이션을 직접 생성해서 사용할 수 있다.

**| 애너테이션 정의**

```java
@Target({ElementType.FIELD, ElementType.METHOD, ElementType.PARAMETER, ElementType.TYPE, ElementType.ANNOTATION_TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Inherited
@Documented
@Qualifier("mainDiscountPolicy")
public @interface MainDiscountPolicy {
}
```

**| 애너테이션 사용**

```java
@Component
@MainDiscountPolicy
public class RateDiscountPolicy implements DiscountPolicy{}
```

```java
@Autowired
public class 구현체명(@MainDiscountPolicy DiscountPolicy discountPolicy) {
	...
}
```

# 📚 Reference

- [Infrean - 김영한님 스프링 로드맵](https://www.inflearn.com/roadmaps/373)
- [코드스테이츠 백엔드 부트캠프](https://www.codestates.com/course/backend-engineering)
