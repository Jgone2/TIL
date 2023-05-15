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

**| PrototypeBean**
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

**| PrototypeBeanInSingleton**
```java
@Scope	// singleton타입이 default이기 때문에 명시해주지 않아도 된다.
static class SingletonBean {
	private final PrototypeBean prototypeBean;
    
    public SingletonBean(PrototypeBean prototypeBean) {
    	this.prototypeBean = prototypeBean;
    }
    
    public int logic() {
    	prototypeBean.addCount();
        return prototypeBean.getCount();
    }
}
```

**| PrototypeBeanInSingletonTest**
```java
@Test
void onlyPrototype() {
	ApplicationContext ac = new AnnotationConfigApplicationContext(PrototypeBean.class);
	PrototypeBean protoBean1 = ac.getBean(PrototypeBean.class);
	PrototypeBean protoBean2 = ac.getBean(PrototypeBean.class);

	protoBean1.addCount();
	protoBean2.addCount();
	assertThat(protoBean1.getCount()).isEqualTo(1);
	assertThat(protoBean2.getCount()).isEqualTo(1);
}

// ====== result ======
Test Success
// ====================

@Test
void protoWithSingleton() {
	ApplicationContext ac = new ANnotationConfigApplicationContext(SingletonBean.class, PrototypeBean.class);
    SingletonBean sBean1 = ac.getBean(SingletonBean.class);
	int count1 = sBean1.logic();
    assertThat(count1).isEqualTo(1);
    
    SingletonBean sBean2 = ac.getBean(SingletonBean.class);
    int count2 = sBean2.logic();
    assertThat(count2).isNotEqualTo(1);
    assertEquals(2, count2);
}
// ====== result ======
Test Success
// ====================
```
위의 테스트에서 첫번째 테스트인 `onlyPrototype()`테스트에서는 프로토타입 스코프만 사용해서 테스트를 수행했다. 프로토 타입만 사용했을 때는 생성과 의존관계 주입까지만 수행한 후 반환될 때 `protoBean2`를 수행할 때 `getCount()`값이 1인 것을 알 수 있다.

하지만 두번째 테스트인 `protoWithSingleton()`테스트에서 싱글톤 빈을 생성한 후 싱글톤 빈에서 프로토타입빈을 주입해서 사용했을 때는 `count`값이 싱글톤타입에서 관리되기 때문에 값이 유지된다. **싱글톤에서 관리되는 프로토타입 빈은 싱글톤 빈 생성시점에 주입되어서 새로 생성되었고, 아래의 사진결과와 같이 사용할 때마다 새로 생성되지 않기 때문이다.**

![](https://velog.velcdn.com/images/jgone2/post/623161af-d854-4aa7-a154-9c6866cca16b/image.png)

위의 결과를 확인하면 PrototypeBean.init이 한번만 수행된 것을 볼 수 있다. 그렇기 때문에 `sBean2`에서 테스트를 수행할 때 `isNotEqualTo(1)`과 `assertEquals(2, count2);`를 수행했을 때 테스트가 성공한 것을 볼 수 있다.

스프링이 일반적으로 싱글톤 빈 환경에서 사용되므로 싱글톤 빈 환경에서 프로토타입 빈이 사용하게 될텐데 위와 같은 문제점이 발생한다.

이 문제점을 어떻게 해결할 수 있을까?

이 문제점은 `Provider`이라는 것을 사용해서 해결할 수 있었다.

## 4. Provider - PrototypeWithSingleton 해결

위의 문제를 해결하기 위해 Provider를 사용할 수 있는데 싱글톤 빈에서 프로토타입 빈을 주입하는 것을 프로토타입을 사용할 때마다 새로 스프링컨테이너에 요청하도록 변경하면 된다. 먼저 `Provider`를 사용하지 않고 변경해보겠다.

**| PrototypeBeanInSingleton**
```java
@Scope
static class SingletonBean {
	private APllicationContext ac;
    
    public int logic() {
    PrototypeBean prototypeBean = ac.getBean(PrototypeBean.class);
    	prototypeBean.addCount();
        return prototypeBean.getCount();
    }
}
```
위의 방식대로 logic()을 수행할 때마다 항상 새로운 프로토타입을 생성할 수 있다. 외부에서 주입받는 것이 아니라 직접 의존관계를 찾는 것을 `Dependency Lookup(DL)`이라고 한다.

하지만 이 방식을 사용하는 것은 스프링의 `Application Context`를 주입받게 되므로 스프링컨테이너에 종속적이게 되고, 단위 테스트도 어려워 진다는 단점이 존재한다. 이를 해결하기 위해 `Provider`를 사용한다.

### 1. ObjectFactory와 ObjectProvider

위의 방식처럼 개발자가 직접 빈을 주입하지 않고 지정한 빈을 스프링 컨테이너에서 대신 찾아주는 DL 서비스를 제공하는 것이 바로 `ObjectProvider`와 `ObjectFactory`가 있다. 

`ObjectFactory`는 비교적 과거의 기술로 별도의 라이브러리가 필요없는 대신 기능이 단순한 특징이 있다. ObjectFactory에 편의 기능이 추가되어 만들어진 것이 바로 `ObjectProvider`다.
ObjectFactory를 상속하고 옵션, 스트림 처리 등의 편의 기능이 많으며 별도의 라이브러리가 필요하다는 특징이 있다. 마찬가지로 스프링에 의존하는 기술이다.

**| PrototypeBeanInSingleton**
```java
@Scope
static class SingletonBean {
//	private ObjectFactory<PrototypeBean> prototypeBeanProvider; // ObjectFactory를 사용할 때
	private ObjectProvider<PrototypeBean> prototypeBeanProvider;
    
    // 생성자 주입
    ...
    
    public int logic() {
    PrototypeBean prototypeBean = prototypeBeanProvider.getObject();
    	prototypeBean.addCount();
        return prototypeBean.getCount();
    }
}
```
`prototypeBeanProvider.getObject()`를 통해서 항상 새로운 프로토타입 빈이 생성되는 것을 알 수 있다. 하지만 `Provider`역시 스프링에 의존하는 기술로 스프링 컨테이너 내부에서만 사용이 가능하다. 

스프링이 아닌 외부의 다른 컨테이너 사용하려면 자바표준 Provider를 사용하면 된다.

### 2. JSR-330 Provider

`javax.inject.Provider`라이브러리를 사용해서 JSR-330 자바 표준을 사용하는 방법을 사용하면 스프링에 의존하지 않고 자바코드 스프링이 아닌 다른 컨테이너에서도 사용가능하다.

먼저 `build.gradle`파일에 라이브러리를 추가해야한다.
- 스프링부트 3.0 미만: `implementation 'javax.inject:javax.inject:1'`
- 스프링부트 3.0 이상: `implementation 'jakarta.inject:jakarta.inject-api:x.x.x'`

**| PrototypeBeanInSingleton**
```java
@Scope
static class SingletonBean {
	private Provider<PrototypeBean> prototypeBeanProvider;
    
    //생성자 주입
    ... 
    
    public int logic() {
    PrototypeBean prototypeBean = prototypeBeanProvider.get();
    	prototypeBean.addCount();
        return prototypeBean.getCount();
    }
}
```

JSR-330 자바 표준 `Provider`를 사용하면 `.get()`을 통해서 항상 새로운 프로토타입 빈을 생성한다. `ObjectProvider`처럼 DL 서비스를 제공한다.

이런 방법들 외에도 메서드에 `@Lookup` 애노테이션을 사용하는 방법도 있다. 싱글톤 빈으로 대부분의 문제를 해결할 수 있기 때문에 프로토타입 빈을 직접적으로 사용하는 일은 드물다고 한다. 그렇기 때문에 필요시에 싱글톤 환경에서 프로토타입 빈을 사용하게 된다면 `@Lookup` 애노테이션을 사용한 방식을 사용해 봐야겠다.

