# 1. @ComponentScan과 의존관계 자동 주입

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

```java
@Configuration
@ComponentScan
public class AppConfig {}
```

정말 간단하게도 `@Bean`태그도 의존관계를 주입하는 코드도 입력하지않고
