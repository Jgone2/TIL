# 제네릭(Generic)

`제네릭(generic)`은 다양한 타입의 객체들을 다루는 메서드나 클래스에서 컴파일 시에 타입체크를 해주는 기능입니다. 컴파일 시에 객체의 타입을 체크하면 다음과 같은 장점을 얻을 수 있습니다.

- 클래스나 메소드 내부에서 사용되는 객체의 타입 안정성↑
- 반환값에 대한 타입 변환 및 타입 검사에 들어가는 노력↓ → 코드의 간결화

여기서 타입 안정성을 높인다는 것은 의도하지 않은 타입의 객체가 저장되거나 저장된 객체를 꺼내올 때 잘못 형변환되는 오류를 줄여주는 것을 말합니다.

특히 ArrayList와 같은 `컬렉션(Collection)` 클래스에서는 다양한 객체를 저장할 수 있는데 원하지 않는 객체가 포함되는 것을 막아주는 역할을 하기도 합니다.

# 1. 제네릭의 선언 및 생성

## 1. 제네릭의 선언

제네릭 타입은 클래스와 메서드에 선언할 수 있습니다. 제네릭은 일반 클래스를 생성하듯이 하고 클래스명 옆에 `<T>`를 붙이면 됩니다.

```java
class RandomTypeBox<T> {
	T item;

    RandomTypeBox(T item) {
    	this.item = item;
    }

    public T getItem() {
    	return item;
    }

    public void setItem(T item) {
    	this.item = item;
    }
}
```

여기서 `T` 타입(Type)의 첫 글자를 따온 것입니다. `T`이외에도 요소(Element)의 `E`, 키(Key)와 값(Value)에서 따온 `K` 그리고 `V`가 있습니다. K와 V는 보통 key와 value로 이루어진 `Map`에서 사용됩니다.

그리고 제네릭 클래스는 타입 매개변수를 임의의 타입으로 설정하는 만큼 클래스 변수를 사용할 수 없습니다. 그 이유는 클래스 변수(static)은 모든 인스턴스가 공유하는 변수인데 제네릭을 사용하면 각 인스턴스가 지정된 타입으로 설정됩니다.  
그래서 제네럴 지정 시 변수의 타입이 인스턴스 별로 달라질 수 있기 때문에 클래스 변수를 사용할 수 없습니다.

## 2. 제네릭의 생성

제네릭의 타입부분에 `String`을 지정하면 `String`형으로, `Integer`으로 지정하면 `Integer`형으로 지정이 됩니다.

```java
RandomTypeBox<String> strBox = new RandomTypeBox<String>("Computer");
String computer = strBox.getItem();
System.out.println(computer);

RandomTypeBox<Integer> intBox = new RandomTypeBox<>(1234);
int result = intBox.getItem();
System.out.println(result);

RandomTypeBox<Boolean> booleanBox = new RandomTypeBox<>(true);
boolean booleanResult = booleanBox.getItem();
System.out.println(booleanResult);

/* ====== result ======
Computer
1234
true
======================= */
```

### 1. 다형성의 적용

위의 예제처럼 다양한 형을 지정하는 방법으로도 사용할 수 있지만 `다형성`도 제네릭 클래스에 적용이 가능합니다.

```java
class Framework{}
class SpringFramework extends Framework {}
class SpringSeason{}

class Test {
	public static void main(String[] args) {
    	Box<Framework> spring = new Box<>();
        spring.setItem(new SpringFramework());	// 다형성 적용
        spring.setItem(new SpringSeason());		// error
    }
}

class Box<T> {
	private T item;

    public T getItem() {
		return item;
    }

    public void setItem(T item) {
    	this.item = item;
    }
}
```

`new SpringFramework()`로 생성된 인스턴스는 `SpringFramework`타입으로 `Framework`클래스를 상속받고 있기 때문에 다형성이 적용이 되기 때문에 `new SpringSeason()`으로 생성된 인스턴스는 `Framework`클래스와 관련없는 클래스로 할당되지 못하고 에러가 발생하게 됩니다.

### 2. 제한된 제네릭 클래스

위에서 살펴본 제네릭 클래스에서는 인스턴스화를 수행할 때 한 종류의 타입을 지정할 수 있지만 또 다른 인스턴스를 생성하면 모든 종류의 타입을 지정할 수 있었습니다.  
하지만 이 방법으로는 원하는 타입만 입력할 수 있도록 제한할 수 없는데, 이를 해결할 수 있는 타입의 종류를 제한할 수 있는 방법이 있습니다.

다음과 같이 제네릭 타입에 클래스 상속에서 사용했던 `extends`키워드를 사용해서 특정 타입의 자손들만 지정할 수 있게 제한 할 수 있습니다.

```java
class Apple {}
class IPhone extends Apple{}
class Galaxy {}
class AppleBox<T extneds Apple> {
	// 수행문
}
class Main {
	public static void main(String[] args) {
    	AppleBox<IPhone> iPhone = new AppleBox<>();
        AppleBox<Galaxy> galaxy = new AppleBox<>();	// error
    }
}
```

만약 상속을 클래스가 아니라 인터페이스로 구현을 했어도 위의 예제와 똑같이 `extends`키워드를 사용해서 구현이 가능합니다.

```java
class Apple {}
class IPhone extends Apple{}
class Galaxy {}
class AppleBox<T extneds Apple> {
	// 수행문
}
```

클래스를 상속받고 동시에 특정 인터페이스를 통해 구현한 클래스 타입으로만 지정할 수 있게하려면 `&`을 사용해서 작성하면 됩니다. 여기서 주의해야할 점은 클래스가 인터페이스보다 앞에 위치해야 한다는 것입니다.

```java
interface Apple {}
class Phone implements Apple{}
class IPhone14 extends Phone implements Apple{}

class Box<T extends Phone & Apple> {
}
```

# 2. 와일드 카드

와일드 카드는 어떠한 타입으로든 대체될 수 있는 타입 파라미터를 의미하며, `?`키워드를 사용해서 와일드카드를 구현할 수 있습니다. 그러나 `?`키워드만 사용하면 모든 타입의 파라미터로 대체할 수 있으므로 `Object`타입과 같게 됩니다.

모든 타입으로의 대체를 방지하고 원하는 타입만 대입할 수 있도록 지정하기 위해서 `extends`와 `super`을 사용하여 상한과 하한타입을 지정할 수 있습니다.

> - < ? extends T > : 와일드 카드의 상한 제한. T와 그 자손들만 대입 가능

- < ? super T > : 와일드 카드의 하한 제한. T와 그 조상들만 대입 가능

와일드 카드의 예제로 다음과 같은 클래스를 정의 했습니다. 휴대폰 클래스가 있고 휴대폰 클래스를 상속받는 아이폰과 갤럭시 그리고 아이폰과 갤럭시를 상속받는 각 세부 모델 클래스를 정의했습니다.

```java
class Phone{}

class IPhone extends Phone{}
class Galaxy extends Phone{}

class IPhone14 extends IPhone{}
class S23 extends Galaxy{}

class User<T> {
	public T phone;

    public User(T phone) {
    	this.phone = phone;
    }
```

여기서 각 아이폰과 갤럭시의 각 기능을 분류해보겠습니다.

- 공통
  - `call`: 아이폰과 갤럭시 두 기종 모두 통화가 가능
- 아이폰
  - `applePay`: 아이폰에서만 사용 가능
  - `faceId`: 아이폰에서만 사용 가능
- 갤럭시
  - `samsungPay`: 갤럭시에서만 사용 가능

휴대폰 - 갤럭시/아이폰의 기능을 최소한으로 나눠보았습니다.

```java
class PhoneFunc {
	public static void call(User<? extends Phone> user) {
    	System.out.println("모든 Phone은 통화를 할 수 있습니다.");
    	System.out.println("user.phone = " + user.phone.getClass().getSimpleName());
    }


	public static void faceId(User<? super IPhone> user) {
    	System.out.println("faceId는 IPhone에서만 사용 가능 합니다");
        System.out.println("user.phone = " + user.phone.getClass().getSimpleName());
    }

    public static void applePay(User<? extends IPhone> user) {
    	System.out.println("ApplePay는 IPhone에서만 사용 가능 합니다");
        System.out.println("user.phone = " + user.phone.getClass().getSimpleName());
    }

	public static void samsungPay(User<? extends Galaxy> user) {
    	System.out.println("SamsungPay는 Galaxy에서만 사용 가능 합니다");
        System.out.println("user.phone = " + user.phone.getClass().getSimpleName());
    }
}
```

위의 예제에서 `call`, `applePay`, `samsungPay`는 `extends`키워드를 사용했고 `faceId`는 `super`키워드를 사용 했습니다.

먼저 `extends`키워드를 사용해서 구현한 메서드 먼저 살펴보겠습니다.

위의 예제를 호출하면 `call`은 `Phone`이거나 상속받고 있는 `IPhone`과 `Galaxy`. 그리고 아이폰14와 S23기종까지 모두 호출할 수 있습니다.

그리고 `applePay`는 `IPhone`타입이거나 상속받고 있는 아이폰14에서 호출이 가능합니다. 이와 마찬가지로 `samsungPay`는 `Galaxy`타입이거나 상속받고 있는 S23에서 호출이 가능합니다.

그리고 `super`키워드를 사용한 `faceId`를 각 객체에서 호출하면 타입이 `Galaxy`이거나 상속받고 있는 `S23`의 경우 당연히 호출할 수 없고 `super`키워드가 하한 제한이기 때문에 `IPhone`의 하위 클래스인 아이폰14에서도 `faceId`는 호출될 수 없습니다. 오직 `IPhone`클래스과 상위 클래스인 `Phone`클래스에서만 호출할 수 있습니다.

# 3. 제네릭 메서드

제네릭 메서드는 메서드의 선언부에 제네릭 타입이 선언된 메서드를 말합니다. 제네릭 메서드의 타입 선언은 반환타입 앞에서 선언됩니다. 또한 제네릭 클래스의 타입 매개변수와 제네릭 메서드의 타입 매개변수는 별개의 것으로 적용 됩니다.

```java
class Test<T> {
	public <T> void add(T element) {
		// 구현부
	}
}

class GenericTest {
	public static void main(String[] args) {
    	Test<String> test = new Test<>();	// Test<T>가 <String>으로 지정
        test.<Integer>add(10);				// <T> void add가 <Integer>로 지정 -> <Integer> 생략가능
    }
}
```

또한 제네릭 메서드를 사용하면 메서드가 호출되는 시점에 제네릭 타입이 결정되므로 정의하는 시점에서 `charAt()`, `length()`와 같은 `String`클래스의 메서드를 사용할 수 없습니다.

그러나 최상위 클래스인 `Object`클래스의 메서드(`equals()`, `toString()` ...)는 사용가능 합니다.

# 📚 Reference

- [Java의 정석](https://product.kyobobook.co.kr/detail/S000001550352)
- [TCP School](http://tcpschool.com/java/intro)
- [CodeStates](https://www.codestates.com/course/backend-engineering)
