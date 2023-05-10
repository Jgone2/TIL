# 1. Bean Scope
`빈 스코프(Bean Scope)`는 **`빈이 유지될 수 있는 범위`** 를 말한다. `scope`의 사전적 의미가 **범위**이니 번역 그대로 해석하면 된다. 빈 스코프는 특정 bean 객체에서 생성된 개체에 연결할 다양한 의존성 및 구성 값뿐만 아니라 특정 Bean 객체에서 생성된 개체의 범위도 제어할 수 있다.

# 2. 스프링에서 제공하는 6가지 빈 스코프
| Scope | Description |
| -- | -- |
| singleton | **<span style="color: red">(Default)</span>**, 기본 스코프로 스프링 컨테이너의 생성과 소멸까지 유지되는 가장 넓은 범위의 스코프 |
| prototype | 스프링 컨테이너는 프로토타입 빈의 생성과 의존관계 주입까지만 관여하고 더는 관리하지 않는 매우 짧은 범위의 스코프 |
| request | **<span style="color: orange">(Web)</span>**, 웹 요청이 들어오고 나갈 때까지 유지되는 스코프 |
| session | **<span style="color: orange">(Web)</span>**, 웹 세션이 생성되고 종료될때까지 유지되는 스코프 |
| application | **<span style="color: orange">(Web)</span>**, 웹의 Servlet Context와 같은 범위로 유지되는 스코프 |
| websocket | **<span style="color: orange">(Web)</span>**, 단일 빈의 범위를 WebSocket의 라이프 사이클까지 확장한 스코프.<br />→(Spring ApplicationContext의 Context에서만 유효)

스프링에서 제공되는 6가지 빈 스코프의 종류는 위의 표처럼 `singleton`, `prototype`, `request`, `session`, `application`, `websocket`이렇게 6가지가 있다. 스코프 범위 설정은 아래와 같이 할 수 있다.

**| 자동 빈 등록 사용 시**
```java
@Scope("prototype")
@Component
public class ScopeEx() {
	...
}
```

**| 수동 빈 등록 사용 시**
```java
@Scope("prototype")
@Bean
PrototypeBean protoBean() {
	...
}
```

## 1. Singleton Scope
- 스프링 컨테이너의 시작부터 종료시점까지 유지
- 하나의 공유 인스턴스만 관리
- 조회 시 항상 같은 인스턴스의 스프링 빈 반환
- 단일 인스턴스를 싱글톤 빈의 캐시에 저장
- 요청 시 캐싱된 개체를 반환

**| SingletonBean**
```java
@Scope  // default: singleton
static class SingletonBean {
	@PostConstruct
    public void init() {
		System.out.println("SingletonBean.init");
    }

    @PreDestroy
    public void destroy() {
    	System.out.println("SingletonBean.destroy");
    }
}
```
**| SingletonTest**
```java
@Test
void singletonBeanFind() {
    AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(SingletonBean.class);
    SingletonBean singletonBean1 = ac.getBean(SingletonBean.class);
    SingletonBean singletonBean2 = ac.getBean(SingletonBean.class);
        
    System.out.println("singletonBean1 = " + singletonBean1);
    System.out.println("singletonBean2 = " + singletonBean2);
        
    assertThat(singletonBean1).isSameAs(singletonBean2);
	ac.close();
}
```


**| result**
![](https://velog.velcdn.com/images/jgone2/post/ca87d4d7-c9ab-4fa9-9cb9-9c59c9d033be/image.png)

결과값을 보면 싱글톤 스코프 빈은 스프링 컨테이너의 시작부터 종료까지 유지되어 종료 직전 destroy명령어가 출력되는 것을 볼 수 있다.

## 2. Prototype Scope
프로토타입 스코프는 프로토타입 빈을 생성하고, 의존관계를 주입하고 초기화 할때까지만 유지되는 스코프다. 의존관계를 주입하고 초기화한 이후 빈을 반환하게 되면 스프링 컨테이너는 이 빈을 관리하지 않는다. 그렇기 때문에 이전의 `@PreDestroy`와 같은 종료 메서드는 호출되지 않는다.

- 프로토타입 스코프 빈을 스프링 컨테이너에 요청한 시점부터 프로토타입 빈의 생성, 의존관계 주입 시까지 유지
- 의존관계 주입 후 생성한 프로토타입 빈을 클라이언트에 반환
- 이후에 같은 요청이 와도 항상 새로운 프로토타입 빈을 생성 후 반환

여기서 중요한 것은 프로토타입 빈은 스프링 컨테이너에서 **빈 생성, 의존관계 주입, 초기화**까지의 과정에만 유지되기 때문에, 스프링 컨테이너가 종료될 때 실행되는 `@PreDestroy`와 같은 종료 메서드가 호출되지 않는다.

**| PrototypeBeanTest**
```java
@Scope("prototype")
    static class PrototypeBean {
        @PostConstruct
        public void init() {
            System.out.println("PrototypeBean.init");
        }

        @PreDestroy
        public void destroy() {
            System.out.println("PrototypeBean.destroy");
        }
    }
```

프로토타입빈 클래스를 다음과 같이 설정하고 테스트 실행을 돌렸을 때 결과는 다음과 같다.
![](https://velog.velcdn.com/images/jgone2/post/c752e45d-ee45-4453-b053-5728ec228c92/image.png)

싱글톤 빈과 다르게 `.init`이 각 프로토타입이 호출될때 마다 호출되어 두 번 호출되고 `.destroy`가 호출되지 않은 것을 볼 수 있다. `.destroy`는 스프링 컨테이너가 종료될때 `@PreDestroy`애너테이션이 위치한 메서드가 실행되는데, 프로토타입 빈은 의존관계 주입 및 초기화한 이후 스프링 컨테이너가 종료되기 전에 빈이 반환되므로 `.destroy`가 출력되지 않는다.

## 3. 싱글톤빈과 프로토타입 빈의 동시사용
스프링 컨테이너에서 프로토타입 스코프의 빈을 요청했을 때 요청마다 새로운 객체 인스턴스를 생성 및 반환한다. 그러나 싱글톤 빈과 함께 사용하면 문제점이 발생한다. 

**| PrototypeBeanTest**
```java
@Scope("prototype")
static class PrototypeBean {

	private int cnt = 0;
    public void addCount() {cnt++}
    public int getCount() {return cnt;}
    
    @PostConstruct
	...        
}
```



