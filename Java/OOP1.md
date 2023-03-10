# OOP(Object-Oriented Programming)

객체지향 프로그래밍은 우리가 살고있는 실제 세계처럼 모든 것은 사물(객체)로 이루어져 있고 모든일은 각 객체의 상호작용에 의해 발생한다.를 기본개념으로 발전해왔습니다.  
여기서 객체는 책상, 의자, 컴퓨터 등 우리 눈에 보이는 것 뿐만 아니라 논리, 사상, 개념 등 무형의 대상들도 객체에 해당됩니다.

- OOP는 Java와 C++, Python 등을 포함한 많은 프로그래밍 언어에 기초하는 프로그래밍 패러다임 중 하나
- 프로그래밍에서 필요한 데이터를 추상화시켜 상태와 행위를 가진 객체를 만들고 그 객체들 간의 유기적인 상호작용을 통해 로직을 구성하는 프로그래밍 방법(기존의 프로그래밍 언어와 다른 새로운 것이 아님)
- 객체의 관점에서 프로그래밍을 함
- 절차지향과 비교해서 사람의 사고방식과 가까움

# 1. OOP의 특징 및 장점

OOP의 특징이자 장점은 대표적으로 다음 3가지가 있습니다.

- 1. 코드의 재사용성↑
  - 이미 생성해놓은 객체를 가져와 사용가능
  - 상속을 통해 필요한 요소를 추가해 확장 사용 가능
- 2. 유지보수 용이
  - 코드간의 관계성을 이용하여 코드 관리 가능
  - 수정이 필요한 코드는 보통 클래스 내부의 멤버 변수 혹은 메서스(method)에 존재
- 3. 신뢰성↑
  - 코드 중복문제❌
  - 제어자와 메서드를 이용해서 데이터 보호 및 올바른 값 유지

위의 장점들을 볼 때 가장 큰 장점은 `코드의 재사용성이 높고 유지보수가 용이`하다는 것입니다. 이러한 장점들은 프로그램의 개발과 유지보수에 소모되는 시간을 획기적으로 개선해 주었습니다.

# 2. 클래스와 객체

## 1.클래스와 객체의 정의와 용도

|     구분      | 정의                                | 용도                              |
| :-----------: | ----------------------------------- | --------------------------------- |
| 클래스(Class) | 객체를 정의                         | 객체 생성                         |
| 객체(Object)  | 현실 세계에 존재하는 사물 또는 개념 | 각 객체가 가지고 있는 기능과 속성 |

`객체(Object)`는 앞서 언급했듯이 우리가 '인지할 수 있고 실재하는 모든 것'이자 '사용할 수 있는 실체'를 의미합니다.
그리고 `클래스(Class)`란 이런 **객체를 정의해 놓은 설계도 또는 틀**이라고 할 수 있습니다.  
이해하기 쉽게 실생활에서 클래스와 객체의 관계를 나타내면

> - 자동차 설계도와 완성된 자동차

- 레시피와 만들어진 음식
- 휴대폰 설계도와 우리가 사용하고 있는 휴대폰
- 붕어빵 기계와 붕어빵

으로 예시를 들 수 있겠습니다.  
위의 예시에서 '설계도'에 해당하는 '자동차 설계도', '레시피', '휴대폰 설계도', '붕어빵 기계'가 `클래스`에 해당됩니다.  
그리고 클래스를 통해 생성된 '자동차', '음식', '휴대폰', '붕어빵'을 객체라고 하는데 클래스에 의해 생성된 객체를 해당 클래스의 `인스턴스(instance)`라고 합니다. 그리고 클래스로부터 객체를 만드는 과정을 `인스턴스화(instantiate)`라고 합니다.

객체와 인스턴스는 같은 의미이므로 사용에 차이를 둘 필요는 없습니다. 다만 `객체`는 모든 인스턴스를 의미한다면, `인스턴스`는 해당 인스턴스가 어떤 클래스로부터 생성되었는지를 강조한다는 데 의미의 차이가 있습니다.

> 💡 인스턴스(Instance)

- 클래스는 객체의 속성을 정의하고, 기능을 구현하여 만들어 놓은 코드 상태
- 실제 클래스 기반으로 생성된 객체(인스턴스)는 각각 다른 멤버 변수 값을 가짐
- new 키워드를 사용하여 인스턴스 생성

## 2. 객체(Object)

### 1. 객체의 구성요소 - 속성과 기능

객체는 `속성`과 `기능`이라는 두 가지 구성요소로 이뤄져 있습니다. 일반적으로 객체는 여러가지 속성과 기능을 가지고 있으며 객체는 이 속성과 기능들의 집합이라고 볼 수 있습니다. 또한, 객체가 가지고 있는 속성과 기능을 그 객체의 멤버(member)라고 합니다.

`속성(property)`은 멤버변수, 필드(field), 특성, 상태라고도 하고 `기능(function)`은 메서드(method), 함수, 행위라고 지칭하기도 합니다. 보통은 속성과 필드, 그리고 기능은 보통 메서드라고 지칭합니다.

속성과 기능을 '라디오'로 예를 들어 보겠습니다.  
라디오의 속성으로는 전원상태, 너비, 높이, 색상, 볼륨, 채널과 같은 것들이 있을 수 있고, 기능으로는 전원 turnOn/Off, 볼륨 ↑/↓, 채널변경 등이 있겠습니다. 이를 클래스 객체의 코드로 작성해보면 다음과 같습니다.

```java
class Radio {
	// 라디오의 속성(attribute)
	boolean power;
    String color;
    int volume;
    double channel;


    // 라디오의 기능(method)
    void stateOfPower {
    	if(power == true) System.out.prinLn("현재 전원이 켜진 상태입니다");
        else System.out.println("현재 전원이 꺼진 상태입니다");
    }
    void turnOn() {
    	power = true;
    	System.out.println("전원을 켭니다");
    }
    void turnOff() {
    	power = false;
    	System.out.println("전원을 끕니다");
    }

    void volumeUp() {
    	volume++;
    	System.out.println("현재 볼륨은 " +volume + "입니다");
    }
    void volumeDown() {
    	volume--;
    	System.out.println("현재 볼륨은 " + volume + "입니다");
    }

    void channelUp() {
		channel++;
    	System.out.println("현재채널은 (ch)" + channel + "입니다");
    }
    void channelDown() {
    	channel--;
    	System.out.println("현재 채널은 (ch)" + channel + "입니다");
    }
}
```

### 2. 객체의 생성과 사용

#### 1. 객체의 생성과 사용

위의 예시는 단순히 라디오의 설계도에 불과하기 때문에 라디오로 사용할 수 없습니다. 설계를 끝낸 객체를 사용하려면 객체를 생성해야합니다. 즉, 인스턴스화 해야합니다.  
객체를 생성할 때는 `new`연산자를 사용하여 인스턴스화 할 수 있습니다. 그리고 생성된 객체에 포인트 연산자`.`를 사용해서 객체의 멤버에 접근할 수 있습니다.

```java
class RadioTest {
	public static void main(String args[]) {
    	Radio myRadio = new Radio();
        myRadio.power = true;
        myRadio.color = blue;
        myRadio.volume = 5;
        myRadio.channel = 7.0;

        myRadio.turnOn()
        myRadio.volumeUp();
        myRadio.channelDown();
    }
}
```

#### 2. 객체의 생성과정

다음으로 객체가 어떻게 저장되고 값이 할당되는지 알아보겠습니다.

**1. 인스턴스를 참조하기위한 참조변수 생성**

```java
Radio myRadio;
```

Radio타입의 참조변수 myRadio를 선언하고 메모리에 참조변수 myRadio를 위한 공간이 마련됩니다.

**2. 인스턴스 생성 후, 객체의 주소를 참조변수에 할당**

```java
myRadio = new Radio();
```

`new`연산자에 의해 Radio클래스의 인스턴스가 메모리의 빈 공간에 생성됩니다. 이때 `new`연산자로 생성된 객체는 `Heap Memory`에 생성되고, 참조변수 myRadio는 인스턴스가 저장되어 있는 힙 메모리의 주소값을 가리킵니다.  
여기서 객체생성 시의 메모리 구조를 자깐 살펴보겠습니다.

> 💡 **객체생성 시 메모리 구조** > ![](https://velog.velcdn.com/images/jgone2/post/b53e8835-9299-43bb-aa91-c9db91962c82/image.png)  
> 위의 그림을 보시면 'new'연산자로 생성된 객체가 **힙 메모리 영역**에 저장되는 것을 볼 수 있습니다. 클래스 영역, 스택 영역, 힙 메모리 영역으로 구성된 곳이 바로 JVM의 **Runtime Data Area**입니다. <br/>
> 참조변수를 생성하면 참조변수가 참조할 클래스타입은 **'클래스 영역'** 에 그리고 참조변수는 일반 변수와 같이 **'스택(stack)'** 영역에 저장됩니다.  
> 그리고 new연산자로 객체를 힙 메모리 영역에 저장하면 참조변수 myRadio는 힙메모리 영역에 적재된 주소값을 참조하게 됩니다. <br/>
> 여기서 중요한 것은 **속성**은 힙 메모리 영역에 저장되는 것에 반해 **메서드**는 **'클래스 영역'** 에 저장됩니다. 메서드가 클래스에 저장되고 객체안에서 메서드의 위치를 가리키는 이유는 같은 클래스로 만든 모든 객체는 동일한 메서드를 공유하기 때문에 한번만 저장해두고 참조하여 사용하는 것입니다.

**3. 해당 객체의 속성과 메서드에 접근하여 사용**

```java
myRadio.color = "red";
myRadio.turnOn();
```

위의 예제와 같이 `.`을 통해서 객체 안에 있는 필드값과 메서드에 접근이 가능합니다.

> 💡 힙(Heap) 메모리

- 생성된 인스턴스는 동적 메모리인 힙 메모리(heap memory)에 할당됨
- C나 C++ 사용한 동적 메모리를 직접 해제 시켜야하지만(free() or delete 사용), Java에서는 Garbage Collector가 사용하지 않는 메모리를 수거
- 하나의 클래스로부터 여러개의 인스턴스가 생성되고 각각 다른 메모리 주소를 가짐

# 3. 변수와 메서드

## 1. 선언위치에 따른 변수의 종류

자바에서 변수는 크게 `클래스 변수(cv, class variable)`, `인스턴스 변수(iv, instance variable)`, `지역 변수(lv, local variable)` 세 종류가 있습니다. 이 세가지 변수는 `변수가 선언된 위치`에 따라 종류가 결정되므로 변수가 어떤 필드에 선언되었는지가 가장 중요합니다. 이들은 선언된 위치에 따라 종류가 결정되고 각각 다른 유효 범위(scope)를 가지게 됩니다.

> 💡 **필드(Field)**

- 클래스에 포함된 변수로 클래스 내에 정의된 객체의 속성부분을 필드라고 지칭합니다.

여기서 필드에 해당하는 변수는` 클래스 변수(cv)`와 `인스턴스 변수(iv)`입니다. 두 개의 변수는 멤버변수라고도 하며 나머지는 `지역변수(lv)`에 해당합니다.  
멤버변수 중 `static`이 붙은 것이 클래스 변수이고, 그렇지 않은 것이 인스턴스 변수 입니다.

```java
class Varialbe {
	int iv;			// 인스턴스 변수
    static int cv;	// 클래스 변수(static변수, 공유 변수);

    void method() {
    	int lv;		// 지역변수, method(){}안에서만 유효
    }
}
```

### 1. 인스턴스 변수(iv)

인스턴스 변수는 멤버변수에 해당되며 `new 생성자()`를 통해서 클래스의 인스턴스가 생성될 때 만들어집니다. 그렇기 때문에 인스턴스 변수는 사용하기 위해서는 반드시 이전에 인스턴스를 생성해야 합니다.  
인스턴스는 힙 메모리의 독립적인 공간에 저장되고, 생성된 인스턴스마다 고유한 값을 가질 수 있습니다.  
사람의 이름, 전화번호, 나이, 취미 등이 인스턴스 변수로 사용될 수 있습니다.

### 2. 클래스 변수

클래스 변수는 인스턴스 변수가 독립적인 저장공간을 가지는데 반해 **공통된 저장공간을 공유**합니다. 클래스로 생성된 모든 인스턴스가 공통적인 값을 유지해야하는 경우 `static`을 사용하여 클래스 변수로 선언해야 합니다.

또한, 클래스 변수는 인스턴스를 따로 생성하지 않고도 언제라도 `클래스명.클래스변수`를 통해서 사용 가능합니다. 인스턴스를 생성하지 않고 사용가능한 이유는 클래스 변수는 **클래스 영역**에 저장되어 있기 때문입니다.

### 3. 지역변수

지역변수는 클래스 내 메서드 내에서만 사용 가능한 변수입니다. 위의 멤버변수들과 다르게 지역변수는 **스택 메모리**에 저장되어 메서드의 종료와 함께 동시에 소멸되어 더이상 사용할 수 없게 됩니다.

## 2. 인스턴스 변수와 클래스 변수의 비교

| 구분          | 인스턴스 변수           | 클래스 변수                  |
| ------------- | ----------------------- | ---------------------------- |
| 생성 시점     | 인스턴스가 생성될 때    | 프로그램 실행 시             |
| 사용가능 시점 | 인스턴스 생성 후        | 인스턴스 생성 전             |
| 생성 공간     | 힙 메모리의 인스턴스 내 | 클래스 영역의 클래스 코드 내 |
| 용도          | 각 객체의 고유한 속성   | 모든 객체의 공유 속성        |

## 3. 클래스 변수의 사용

앞서 언급한 것 처럼 클래스 변수는 모든 인스턴스가 공통된 저장공간을 공유하고 클래스 영역에 저장되어 인스턴스화를 하지 않고 클래스를 사용하여 사용할 수 있습니다. 간단한 예제를 통해 살펴보겠습니다.

```java
public static void main(String[] args) {
	StaticEx staticEx1 = new StaticEx();
 	StaticEx staticEx2 = new StaticEx();

    staticEx1.num1 = 100;
    staticEx2.num1 = 200;

    staticEx1.num2 = 300;
    staticEx2.num2 = 400;

    System.out.println("staticEx1.num1 = " + staticEx1.num1);	// 100;
	System.out.println("staticEx2.num1 = " + staticEx2.num1);	// 200;
	System.out.println("staticEx1.num2 = " + staticEx1.num2);	// 400;
	System.out.println("staticEx2.num2 = " + staticEx2.num2);	// 400;
}

class StaticEx {
	int num1;
    static int num2;
}
```

static변수를 사용할 때는 참조변수명이 아니라 클래스명.변수명으로 참조해야하지만 인스턴스 벼눗와 직접적인 차이점을 확인하기 위해 참조변수를 사용하여 사용했습니다.

각 속성의 값을 100, 200, 300, 400을 입력했습니다. 출력 시에도 100, 200, 300, 400이 출력되어야할 것 같지만 인스턴스변수는 각 고유의 값을 출력한 것에 반해, static변수로 선언된 num2는 모두 400으로 출력됨을 알 수 있습니다.

이처럼 static변수를 사용하면 모든 인스턴스에 공통적으로 공유되는 값이 적용된다는 것을 알 수 있습니다. 그리고 클래스변수는 다음과 같은 사용을 권장합니다.

```java
StaticEx.num2 = 400;
```

## 4. 메서드(Method)

메서드(method)는 **"특정 작업을 수행하는 일련의 명령문들의 집합"**을 의미합니다. 앞서 예제로 봤었던 라디오 예제에서 전원켜기, 볼륨조절, 채널조절 등이 메서드 입니다.

### 1. 메서드의 구조

메서드는 `메서드 시그니처`와 '{}'안의 `메서드 바디`로 구분할 수 있습니다.

```java
제어자 반환타입 메서드명(매개 변수) {	// 메서드 시그니처
	메서드 내용	// 메서드 바디
}

public static int add(int x, int y) {
	return x + y;
}
```

`메서드 시그니처`에는 **제어자 반환타입 메서드명 그리고 매개변수가 필요한데, 여기서 매개변수는 메서드 바디의 작업을 수행하기 위해 필요한 속성을 입력받는 변수**입니다.

### 2. 메서드를 사용해야하는 이유

메서드를 사용해서 얻을 수 있는 이점은 다음과 같은 3가지가 있습니다.

> - 높은 재사용성(reusability)

- 중복코드제거
- 프로그램의 구조화

아무래도 메서드를 사용하면 한번 만들어 놓은 메서드를 필요한 만큼 반복해서 사용할 수 있고, 다른 프로그램에서도 메서드 사용이 가능하기 때문에 재사용성을 높일 수 있고 중복코드를 제거 할 수 있습니다.

또한 main메서드에 한번에 코드를 작성하는게 아니라 작업 단위로 나눠서 프로그래밍할 수 있어서 프로그램의 구조화가 가능합니다.

### 3. 메서드의 호출

메서드를 정의한다고 해서 사용되는 것은 아닙니다. 메서드를 필요한 곳에 호출해야 메서드 바디의 수행문들이 수행됩니다. 외부 클래스의 메서드를 사용하기 위해서는 인스턴스 생성 후 사용해야 하지만 동일 클래스의 메서드끼리는 따로 인스턴스를 생성하지 않고 호출가능합니다.

메서드를 호출하는 방법은 다음과 같습니다.

```java
void add(int x, int y) {
	System.out.printf("x + y = %d", x + y);
}

void printHelloWorld() {
	System.out.println("Hello World!");
}

printHelloWorld();
add(3, 5);
```

`add()`메서드를 호출할 때 괄호()안에 값을 적어주게되는데 이것을 `인자(argument)`라고 합니다. 인자의 개수와 순서는 정의된 매서드의 매개변수와 일치해야 합니다. 혹은 자동형변환이 가능해야 합니다.

### 4. 메서드 오버로딩(Method Overloading)

메서드 오버로딩은 하나의 클래스 안에 같은 이름으로 된 메서드를 여러개 정의 한 것을 말합니다. 메서드는 메서드의 이름 또는 매개변수의 타입이 다르면 다른 매개변수라고 인식하는 자바 가상머신의 기능과 관계가 있습니다.

오버로딩(overloading)의 사전적 의미는 '과적하다'입니다. 즉 많이 싣는 것을 뜻하는데 하나의 메서드에 여러 기능을 구현하기 때문에 붙여진 이름입니다.

```java
public class Overloading {
	public static void main(String[] args) {
		Add result = new Add();

       result.addNum();
       result.addNum(2, 3);
       result.addNum(2, 3, 5);
       result.addNum(2, 3, 5, 10);
    }
}

class Add {
	public void addNum() {
    	System.out.println("계산할 숫자를 입력해 주세요");
    }

	public void addNum(int x, int y) {
    	System.out.printf("%d + %d = %d\n", x, y, x+y);
    }

    public void addNum(int x, int y, int z) {
    	System.out.printf("%d + %d + %d = %d\n", x, y, z, x+y+z);
    }

    public void addNum(int x, int y, int z, int a) {
    	System.out.printf("%d + %d + %d + %d = %d\n", x, y, z, a, x+y+z+a);
    }
}

/* ====== result ======
계산할 숫자를 입력해 주세요
2 + 3 = 5
2 + 3 + 5 = 10
2 + 3 + 5 + 10 = 20
====================== */
```

위의 예시와 같이 addNum()이라는 이름을 가진 동일한 이름의 메서드가 4개가 존재하는데 메서드마다 다른 값을 출력하는 것을 보실 수 있습니다. 무조건 매서드명만 같다고 메서드 오버로딩이 되는 것이 아닙니다.  
메서드를 오버로딩하기 위한 조건으로는 다음 두 가지가 있습니다.

> - 메서드의 이름이 일치

- 매개변수의 개수 또는 타입이 달라야함

메서드 오버로딩의 장점으로는 하나의 메서드로 여러 경우의 수를 해결할 수 있다는 점입니다. 만약 메서드 오버로딩이 없었다면 위의 예제의 경우 '2개의 수를 더하는 매서드명', '3개의 수를 더하는 매서드명', '4개의 수를 더하는 매서드명' 등 경우의 수에 따른 매서드명을 추가하고 사용할 때마다 매서드명을 검색해야 하는 번거로움이 발생할 것입니다.

# 4. 생성자

생성자는 인스턴스가 생성될 때 호출되는 인스턴스 초기화 메서드 입니다. 생성자 역시 메서드 처럼 클래스 내에 선언되고 메서드와 비슷하지만 `return`값이 없는 것이 특징입니다. 생성자의 조건은 다음 두 가지가 있습니다.

> - 생성자의 이름은 클래스의 이름과 같아야 함

- 생성자는 리턴 값이 없음

생성자는 기본 생성자와 매개변수가 있는 생성자로 나눌 수 있습니다. 기본 생성자와 매개변수가 있는 생성자를 간단하게 살펴보겠습니다.

```java
클래스 이름(타입변수명, 타입변수명, ...) {
	// 인스턴스 변수 초기화
}

Card() {	// 기본 생성자
}

Card(String customerID, String cardNum, String customerName) {	// 매개변수가 있는 생성자
	...
}
```

위의 예시에서 볼 수 있듯이 기본 생성자와 매개변수가 있는 생성자로 생성자 오버로딩이 가능합니다. 클래스에서 봤듯이 생성자는 객체를 생성하는 것이기 때문에 `new`를 사용해서 호출할 수 있습니다.

## 1. 기본 생성자(default constructor)

기본 생성자는 위의 예제에서 볼 수 있듯이 매개변수가 없는 생성자 입니다. 모든 클래스에는 반드시 하나 이상의 생성자가 정의되어 있어야 합니다.

하지만 생성자를 선언하지 않아도 클래스를 인스턴스화 할 수 있었는데 이는 자바 컴파일러가 '기본 생성자(default constructor)'를 제공해주기 때문입니다.

```java
class Card {
	int cardNum;

    Card(int cardNum) {
    this.cardNum = cardNum;
    }
}
class defaultEx {
	public static void main(String[] args) {
    	Card SHcard = new Card();	// compile error 발생
    }
}
```

위의 예제에서는 기본 생성자를 사용하여 생성자를 선언했지만 에러가 발생했습니다. 그 이유는 클래스 내부에서 생성자를 만들었기 때문입니다. 기본 생성자는 클래스 내 생성자가 단 1개라도 있으면 생성되지 않습니다.

## 2. 매개변수가 있는 생성자

매개변수가 있는 생성자는 메서드처럼 호출 시 인자 값을 넘겨받아서 인스턴스를 초기화하는 생성자 입니다. 매개변수로는 클래스의 속성들이 속합니다. 위의 예제에서 Card는 매개변수가 있는 생성자를 가지고 있습니다.

## 3. 생성자 오버로딩

생성자도 마찬가지로 오버로딩이 가능합니다. 생성자를 따로 구현하게 되면 기본 생성자가 제공되지 않으므로 필요시 직접 생성해서 사용해야합니다. 생성자도 역시 오버로딩이 성립하기 위해서 조건이 필요한데 메서드 오버로딩의 조건과 같습니다.

> - 메서드 이름이 같아야 함

- 매개변수의 개수 또는 타입이 달라야함

```java
class Card {
	int cardNum;

    Card() {}

    Card(int cardNum) {
    this.cardNum = cardNum;
    }
}
```

# 5. this(), this

## 1. this()

같은 클래스 내의 메서드들끼리 서로 호출할 수 있는 것 처럼 생성자도 `this()`를 사용해서 상호 호출이 가능합니다. `this()`를 사용하기 위해서 두 가지 조건을 만족해야 하는데 다음과 같습니다.

> - 반드시 첫 줄에 위치해야함

- 생성자의 내부에서만 사용가능

```java
public class ClassThis {
	public static void main(String[] args) {
    	ThisEx ex1 = new ThisEx();
        ThisEx ex2 = new ThisEx("Kim");
    }
}

class ThisEx {
	public ThisEx() {
    	System.out.println("ThisEx의 기본생성자 호출");
    }

    public ThisEx(String str) {
    	this();
        System.out.printf("%s님이 ThisEx의 두 번째 생성자를 호출", str);
    }
}

/* ====== Result ======
ThisEx의 기본생성자 호출
ThisEx의 기본생성자 호출
Kim님이 ThisEx의 두 번째 생성자를 호출
======================= */
```

class ThisEx는 두 개의 생성자를 가지고 있습니다. 기본생성자 1개와 매개변수가 있는 생성자 1개 입니다. 매개변수가 있는 두 번째 생성자에는 내부의 첫 번째 줄에 `this()`가 포함되어 있습니다.

'result'에서 확인할 수 있듯이 매개변수가 있는 생성자를 호출 했을 때 `this()`의 수행으로 첫 번째 생성자의 `ThisEx의 기본 생성자 호출"`이 먼저 출력되고 두 번째 생성자의 `Kim님이 ThisEx의 두 번째 생성자를 호출"`이 호출 된 것을 볼 수 있습니다.

## 2. this

`this`키워드는 먼저 예제를 보고 알아보겠습니다.

```java
class CustomerInfo {
	private static long customerId;
    private String customerName;
    private String address;
    private int age;

	CustomerInfo (String customerName, String address, int age) {
		long customerID = customerId++;
    	this.customerName = customerName;
    	this.address = address;
        this.age = age;
    }

    void printInfo() {
    	System.out.printf("%s님은 %s에 거주하고 %d세 입니다. customerId = %d", customerName, address, age, customerId);
    }
}

public class ThisEx2 {
	public static void main(String[] args) {
    	CustomerInfo kim = new CustomerInfo("kim", "서울", 10);
        kim.printinfo();
    }
}
```

예제에서 볼 수 있듯이 class CustomerInfo에 인스턴스 변수 customerId, customerName, address, age가 있고 생성자의 매개변수로 customerName, address, age가 있습니다.

이런 경우에 인스턴스 변수와 매개변수를 이름으로 구분하기 어렵습니다. 이때 이를 구분하기 위해 사용되는 것이 `this`입니다. 위의 생성자에서 `this.customerName = customerName`에서 'this'를 지우고 작성하면 두 변수 모두 지역변수로 간주됩니다.

`this`는 인스턴스 자신을 가리키며, `this`를 통해서 인스턴스 자신의 변수에 접근합니다.

# 6. 내부 클래스

내부 클래스는 클래스 내에 선언된 클래스로, 외부 클래스와 내부 클래스가 서로 연관되어 있을 때 사용합니다. 내부 클래스를 이용하면 외부 클래스의 멤버에 쉽게 접근할 수 있고 코드의 복잡성(캡슐화)을 줄일 수 있습니다.

내부 클래스의 종류는 `인스턴스 내부 클래스`, `정적 내부 클래스`, `지역 내부 클래스`, `익명 내부 클래스`가 있습니다.

```java
class a {
	class b {
    	// 인스턴스 내부 클래스
    }
    static class c {
    	// 정적 내부 클래스
    }
    void methodEx() {
    	class d {
        	//	지역 내부 클래스
        }
    }
}
```

## 1. 인스턴스, 정적, 지역 내부 클래스의 비교

| 종류                 | 선언 위치                             | 사용 가능한 변수                   |
| -------------------- | ------------------------------------- | ---------------------------------- |
| 인스턴스 내부 클래스 | 외부 클래스의 멤버변수 위치           | 외부 인스턴스 변수, 외부 전역 변수 |
| 정적 내부 클래스     | 외부 클래스의 멤버 변수               | 외부 전역 변수                     |
| 지역 내부 클래스     | 외부 클래스의 메서드 혹은 초기화 블럭 | 외부 인스턴스 변수, 외부 전역 변수 |

# 📚 Reference

- [Java의 정석](https://product.kyobobook.co.kr/detail/S000001550352)
- [CodeStates](https://www.codestates.com/course/backend-engineering?gclid=Cj0KCQiAo-yfBhD_ARIsANr56g7yG-RoEgb1FYjnCetXZpz1jYm6-RtQCTqC_qgipon5_Aw8OyY4lj8aAizpEALw_wcB)
