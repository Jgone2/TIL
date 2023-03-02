# 1. 상속

상속은 **기존의 클래스를 재활용하여 새로운 클래스를 작성**하는 것을 의미합니다. 두 개의 클래스로 상위 클래스와 하위 클래스로 나누어 상위 클래스의 필드, 메서드, 이너 클래스를 하위 클래스와 공유하는 형태입니다.

상속은 상속을 받을 클래스명 뒤에 상속받고자 하는 클래스명을 `extends`키워드를 사용해서 작성하면 상속이 구현됩니다.

```java
class Parent{}
class Child extends Parent{}
```

위의 형태로 `extends`를 사용해서 상속을 구현하며 예시를 보겠습니다.

```java
class Person {
	String name;
    int age;
    String local;

	void wakeUp() {
		System.out.println("일어나기");
    }
	void walk() {
		System.out.println("걷기");
   }
   void eat() {
   		System.out.println("먹기");
   }
}

class Student extends Person {
	String schoolName;

    void goSchool() {
    	System.out.println("등교하기");
    }
}

class Programmer extends Person{
	String companyName;

    void coding() {
    	System.out.println("코딩하기");
    }
}


public class Sanggsok {
	public static void main(String[] args) {
        Student kim = new Student();
        kim.name = "김코딩";
        kim.age = 18;
        kim.local = "서울";
        kim.schoolName = "한국고등학교";
        System.out.println("=== " + kim.name + " ===");
        kim.wakeUp();
        kim.eat();
        kim.walk();
        kim.goSchool();

        Programmer park = new Programmer();
        park.name = "박자바";
        park.age = 27;
        park.local = "부산";
        park.companyName = "(주)코딩";
        System.out.println("=== " + park.name + " ===");
        park.wakeUp();
        park.eat();
        park.coding();
    }
}
```

예제에서 볼 수 있듯이 클래스 `Person`에서 인스턴스 변수 'name', 'age', 'local'를 선언하고 인스턴스 메서드 'wakeUp()', 'walk()', 'eat()'이 선언되어 있습니다.

그리고 클래스 `Student`와 `Programmer`에서는 `extend`키워드를 사용해서 `Person`을 상속받고 상위 클래스에서 선언된 것을 제외하고 각자 필요한 것만 따로 선언한 것을 볼 수 있습니다.

그리고 main에서 각자의 클래스로 선언한 후에 보시면 `Person`에서 선언한 멤버변수들과 메서드를 사용할 수 있는 것을 보실 수 있습니다.

> 💡 자바에서는 클래스를 이용한 다중 상속은 불가능합니다. 그러나 추후 알아볼 **인터페이스**를 사용하면 다중 확장이 가능합니다.

# 2. 포함 관계(composite)

포함관계는 상속과는 다르지만 상속처럼 클래스를 재사용할 수 있는 방법 중 하나로, 다른 클래스 타입의 참조변수를 선언해서 사용하는 방법 입니다.

```java
public class Card {
	int id;
    String cardNum;
    String exPeriod;
    CustomerInfo cusInfo;

    public static void main(String[] args) {
    	CustomerInfo cus1 = new CustomerInfo("김코딩", 18, "01012345678");
        CustomerInfo cus2 = new CustomerInfo("박자바", 27, "01087654321");

        Card CdCard = new Card(1, "1234", "24/10", cus1);
        Card SSCard = new Card(2, "5678", "27/02", cus2);

        CdCard.showInfo();
        SSCard.showInfo();
    }

    public Card(int id, String cardNum, String exPeriod, CustomerInfo cusInfo) {
    	this.id = id;
        this.cardNum = cardNum;
        this.exPeriod = exPeriod;
        this.cusInfo = cusInfo;
    }

    void showInfo() {
    	System.out.printf("%d세 %s님의 %s번 카드 유효기간은 %s까지 입니다.", cusInfo.age, cusInfo.customerName, cardNum, exPeriod);
    }
}

public CustomerInfo {
	String customerName;
    int age;
    String phoneNum;

    public CustomerInfo(String customerName, int age, String phoneNum) {
	    this.customerName = customerName;
        this.age = age;
        this.phoneNum = phoneNum;
    }
}
```

~~(쓰고나서 생각해보니 반대가 되었어야했는데,,)~~어쨋든,, 카드정보에 고객의 정보를 포함하는 예제를 작성해 봤습니다. `customerName`, `age`, `phoneNum`이 항상 카드 정보에 포함되어야 하지만 고객정보와 카드정보를 분리함으로써 코드의 중복을 없앤 포함관계로 재사용하고 있습니다.

# 3. 메서드 오버라이딩(Method Overriding)

메서드 오버라이딩은 **상위 클래스로부터 상속받은 메서드를 재정의하는 것**을 의미합니다. `Override`의 사전적 의미가 `덮어쓰다`를 의미한다는 것을 생각해보면 이해하기 쉽습니다.

```java
public class VehicleTest {
	public static void main(String[] args) {
    	Bike bike = new Bike();
        Car car = new Car();
        MotorBike motorBike = new MotorBike();

        bike.run();
        car.run();
        motorBike();
    }
}

class Work {
	void working() {
    	System.out.println("I'm working!");
    }
}

class Coding extends Work {
	void working() {
    	System.out.println("I'm coding!");
    }
}

class Baking extends Work {
	void working() {
    	System.out.println("I'm baking!");
    }
}

class Singing extends Work {
	void working() {
    	System.out.println("I'm Singing");
    }
}
```

위의 예시에서 클래스 `Work`에 `working()` 메서드가 정의되어 있습니다. 그리고 `Coding`, `Baking`, `Singing` 각 클래스를 통해 `working()` 메소드를 호출하면 `Work`클래스의 메소드가 아닌 각 클래스의 메소드가 호출되어 수행됩니다.

오버라이딩을 성립시키기 위해서는 3가지 조건이 필요합니다. 3가지 조건은 다음과 같습니다.

> - 메서드의 선언부(메서드 이름, 매개변수, 반환타입)이 같아야 함

- 접근 제어자의 범위가 상위 클래스의 메서드보다 같거나 넓어야 함
- 예외는 상위 클래스의 메서드보다 많이 선언할 수 없음

# 4. super와 super()

## 1. super

`super`는 상위 클래스의 객체를 참조하는데 사용되는 참조변수 입니다. 그러나 상속을 받은 멤버는 `this`를 사용할 수 있기 때문에 상위 클래스와 하위 클래스의 멤버가 중복 정의되어 있는 상황인 경우에 구별의 용도로 `super`을 사용할 수 있습니다.

```java
class SuperTest {
	public static void main(String[] args) {
    	LowerClass x = new LowerClass();
        x.printNum();
    }
}
class UpperClass {
	int myNum = 1;
}

class LowerClass extends UpperClass {
	void printNum() {
    	System.out.println("myNum = " + myNum);
        System.out.println("this.myNum = " + this.myNum);
        System.out.println("super.myNum = " + super.myNum);
    }
}

/* ====== result ======
myNum = 1
this.myNum = 1
super.myNum = 1
======================= */
```

## 2. super()

`super()`은 `this()`처럼 생성자를 호출할 때 사용합니다. 'this'와 'super'의 차이처럼 this()가 같은 클래스 내의 다른 생성자를 호출하고 super()은 상위 클래스의 생성자를 호출한다는 차이점이 있습니다.

```java
class SuperTest {
	public static void main(String[] args) {
    	SecondClass s = new SecondClass();
    }
}
class FirstClass {
	FirstClass() {
    	System.out.println("첫번째 클래스 생성자");
    }
}
class SecondClass extends FirstClass {
	SecondClass() {
    	super();
        System.out.println("두번째 클래스 생성자");
    }
}

/* ====== result ======
첫번째 클래스 생성자
두번째 클래스 생성자
======================= */
```

# 5. 캡슐화(Encapsulation)

캡슐화는 **특정 객체 안에 관련된 속성과 기능을 하나의 캡슈롤 만들어 데이터를 외부로 부터 보호하는 것**을 말합니다. 캡슐화를 하는 이유는 데이터를 보호하고 불필요한 데이터 외부 노출을 방지하여 데이터의 `정보 은닉`을 실현하기 위함입니다.

캡슐화를 실현하면 외부로붵 객체의 속성과 기능을 변경하지 못하게 할 수 있고 코드 수정 시 다른 객체에 영향을 주지 않아 독립성을 확보하고 관계성을 낮출 수 있습니다. 또한 독립성이 확보되면 유지보수시 캡슐화한 부분만 수정하면 되므로 유지보수성이 향상됩니다.

# 6. package와 import

## 1. package

패키지는 특정한 목적을 가지고 있는 클래스와 인터페이스의 묶음을 말합니다. 서로 관련된 클래스와 인터페이스들을 그룹으로 묶어 놓음으로써 효율적으로 관리할 수 있게 도와줍니다.  
패키지를 사용해서 같은 클래스명이 존재할지라도 다른 패키지에 존재하는 것이 가능하여 다른 개발자와 클래스명이 충돌하는 것을 피할 수 있습니다.

패키지를 우리가 사용하는 컴퓨터 바탕화면에 있는 폴더라고 생각해주시면 쉽게 이해하실 수 있습니다. 예를들어 `download`폴더에는 다운 받은 파일들, `school`은 학교관련 파일들끼리 구분해서 폴더를 만들어 놓은 것 처럼 생각해주시면 됩니다.

```java
package 패키지명;
```

## 2. import

import문은 다른 패키지의 클래스를 사용하기 위해 사용하는 명령어입니다. 패키지를 사용하기위해서는 매번 사용할 패키지명을 다 작성해줘야하는데 import문은 사전에 컴파일러에게 소스파일에 사용된 클래스에 대한 정보를 제공해서 자동으로 추가해주는 역할을 수행합니다.

```java
import 패키지명.클래스명;
import 패키지명.*;
import 패키지명;
```

# 7. 제어자(Modifier)

## 1. 제어자

제어자는 **클래스, 변수, 메서드의 선언부에 함께 사용되어 부가적인 의미를 부여하는 키워드**입니다. 제어자의 종류는 크게 `접근 제어자`와 `기타 제어자`로 구분할 수 있습니다.

|    구분     | 종류                                                        |
| :---------: | ----------------------------------------------------------- |
| 접근 제어자 | public, protected, (default), private                       |
| 기타 제어자 | static, final, abstract, native, transient, synchronized 등 |

static은 이미 OOP1에서 언급한바 있고 abstract는 추상화를 다룰 때 함께 다룰 것 이므로 대표적으로 final을 설명드리고 지나가겠습니다.

### 1. final

`final`은 '마지막의', '최종의'라는 뜻을 가지고 있습니다. `final`키워드는 클래스, 메서드, 변수 모두 사용가능합니다.

| 위치   | 의미                                                                              |
| ------ | --------------------------------------------------------------------------------- |
| 클래스 | 변경이나 확장이 불가능한 클래스. 다른클래스의 상위클래스가 될 수 없음(상속 불가). |
| 메서드 | 오버라이딩 불가                                                                   |
| 변수   | 값을 변경할 수 없는 상수                                                          |

> 💡 상수로 선언한 변수명은 Uppercas로 작성하는 것을 권장합니다.

```java
final double PI = 3.14;
```

## 2. 접근 제어자

접근 제어자는 앞서 설명드린 캡슐화를 구현하기 위한 방법 중 하나입니다. 접근 제어자를 사용해서 클래스 외부로의 데이터 노출 방지를 할 수 있고, 외부에서의 데이터 변경을 방지할 수 있습니다. 자바 접근 제어자는 4가지가 있는데 접근 제어 범위와 함께 알아보겠습니다.

| 접근 제어자 | 접근 제한 범위                                     |
| ----------- | -------------------------------------------------- |
| public      | 접근 제한 없음                                     |
| protected   | 동일 패키지 + 동일 레벨의다른 패키지의 하위 클래스 |
| default     | 동일 패키지 내에서만 접근 가능                     |
| private     | 동일 클래스에서만 접근 가능                        |

위의 4가지 접근 제어자를 접근 제한 범위에 따라 정렬하면 public -> protected -> default -> private 순으로 더 높은 제한이 이루어 집니다. 이 중에서 `default`는 접근 제어자를 붙이지 않았을 경우 default로 설정이 됩니다.

# 8. getter & setter

getter와 setter는 앞서 알아본 접근 제어자 중 `private`에서 변수에 접근이 불가능 할 때 변수의 데이터 값을 얻고싶거나 수정하고 싶을 때 사용하는 메서드 입니다. 먼저 예시로 알아보겠습니다.

```java
public class GetterSetterEx {

    public static void main(String[] args) {
        Student kim = new Student();
        kim.setSchoolName("자바고");
        kim.setName("김코딩");
        kim.setAge(18);

        System.out.printf("%s 학생은 %d세로 %s에 다니고 있습니다.", kim.getName(), kim.getAge(), kim.getSchoolName());
    }

}

public class Student {
    private String schoolName;
    private String name;
    private int age;

    public String getSchoolName() {
        return schoolName;
    }

    public void setSchoolName(String schoolName) {
        this.schoolName = schoolName;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

class `Student`에서 모든 멤버변수들이 접근 제어자 `private`로 선언되어 외부 클래스에서 사용할 수 없게 됐습니다. 위의 예시로 보았을 때 `setter`메서드는 데이터 값을 수정할 수 있게 해주고 `getter`메서드는 변수에 할당된 값을 읽어오는 것을 알 수 있습니다.

필요에 따라 `setter`메서드만 부여할 수도 `getter`메서드만 부여할 수도 있으므로 외부 클래스에서 값이 수정간으하게 혹은 읽어올 수만 있도록 의도하여 개발할 수 있습니다.

# 9. 다형성

다형성은 **하나의 객체가 여러가지 형태를 가질 수 있는 성질**을 의미합니다. 자바에서는 **상위클래스 타입의 참조변수로 하위클래스의 인스턴스를 참조할 수 있도록**한다는 것을 의미합니다.

```java
public class ClassTest {
	public static void main(String[] args) {
    	UltraClass u = new UltraClass();
        LowerClass1 x1 = new LowerClass1();
        UltraClass x2 = new LowerClass2();

        u.classInfo();
        x1.classInfo();
        x2.classInfo();
    }
}

class UltraClass {
	public void classInfo() {
    	System.out.println("This is UltraClass");
    }
}

class LowerClass1 extends UltraClass {
	public void classInfo() {
    	System.out.println("This is LowerClass1");
    }
}

class LowerClass2 extends UltraClass {
	public void classInfo() {
    	System.out.println("This is LowerClass2");
    }
}

/* ====== result ======
This is UltraClass
This is LowerClass1
This is LowerClass2
======================= */
```

위의 에시에서 처럼 LowerClass2인스턴스를 참조하는 변수 x2는 `UltraClass`로 생성되어 있습니다. 하지만 그래도 result에서는 LowerClass2의 classInfo()를 잘 참조해서 결과값이 출력된 것을 알 수 있습니다.

**상위 클래스 타입의 참조변수로 하위 클래스의 객체를 참조할 수 있는** 다형성의 핵심적인 특성을 이용했기 때문입니다. 그러나 참조변수가 **상위 클래스 타입이기 때문에 상위 클래스의 멤버의 수만큼만 사용가능**합니다.

## 1. 참조변수의 형변환

참조변수도 기본형 변수처럼 형변환이 가능합니다. 다만 참조변수의 형변환을 수행하기 위해서는 다음과 같은 조건을 충족해야 합니다.

> - 서로 상속관계에 있을 것

- 하위 클래스 -> 상위클래스(업캐스팅): 형변환 연산자 생략 가능
- 상위 클래스 -> 하위 클래스(다운캐스팅): 형변환 연산자 필수 명시

위의 예제에서 x2의 경우가 '하위클래스 -> 상위클래스'의 형변환이라고 볼 수 있고 반대의 경우는

```java
UltraClass u = new UltraClass();
LowerClass2 x2 = (LowerClass2) u;
```

로 형변환을 수행할 수 있겠습니다.

## 2. instanceof 연산자

`instanceof`연산자는 참조변수가 참조하고 있는 타입을 알아보기 위해 사용합니다. 연산의 결과를 `boolean`형으로 반환하며 `true`를 반환값으로 얻으면 형변환이 가능한 타입 입니다.

```java
참조변수 instanceof 타입

public class InstanceOfEx {
	public static void main(String[] args) {
    	Animal animal = new Animal();
        System.out.println(animal instanceof Object);	// true
        System.out.println(animal instanceof Animal);	// true
        System.out.println(animal instanceof Dog);		// false
        System.out.println(animal instanceof Cat);		// false

        Aniaml dog = new Dog();
        System.out.println(dog instanceof Object);	// true
        System.out.println(dog instanceof Animal);	// true
        System.out.println(dog instanceof Dog);		// true
        System.out.println(dog instanceof Cat);		// false
    }
}

class Animal{};
class Dog extends Animal{};
class Cat extends Aniaml{};
```

> 💡 **다형성**특징 다시 보기

- 하나의 코드가 여러 자료형으로 구현되어 실행되는 것
- 같은 코드에서 여러 다른 실행 결과가 나옴
- 정보은닉, 상속과 더불어 객체지향 프로그래밍의 가장 큰 특징 중 하나
- 다형성을 잘 활용하면 유연하고 확장성 있고, 유지보수가 편리한 프로그램을 만들 수 있음

# 10. 추상화

`추상`은 사전적 의미로 `여러 가지 사물이나 개념에서 공통되는 특성이나 속성 따위를 추출하여 파악하는 작용`을 말합니다. 여기서 자바에서의 추상화에 해당하는 부분이 **공통되는 특성을 모아서 추출하여 정의해놓은 것**을 의미합니다.

자바에서는 추상화를 구현하기 위해 `추상 클래스`와 `인터페이스`를 사용합니다.

## 1. abstract 제어자

앞서 제어자 파트에서 `abstract`를 언급한 적이 있습니다. `abstract`제어자는 '추상 메서드'와 '추상 클래스'를 구현하기 위해 사용하는 제어자 입니다. 클래스 내에 추상 메서드가 포함되어있으면 해당 클래스는 자동으로 추상 클래스가 됩니다.

```java
abstract class 클래스명 {			// 추상 메서드가 최소 하나 이상 포함되어 있어야함
	abstract 반환타입 메서드명();	// 메서드 바디가 없는 추상 메서드
}
```

위의 예제를 보면 알 수 있듯이 추상 메서드에는 메서드 바디`{}`가 존재하지 않는다는 것을 알 수 있습니다. 이는 `abstract`가 `추상적인`, `미완성`의 뜻을 내포하고 있는데 구체화되지 않은 `미완성 메스드`, `미완성 클래스`로 설계하기 위한 제어자이기 때문입니다.  
추상 메서드/클래스는 미완성이기 때문에 객체 생성도 불가능합니다.

## 2. 추상 클래스

추상 클래스는 앞서 이야기 했듯이 `미완성 설계도`라고 할 수 있습니다. 미완성 설계도라고 하는 이유는 완성되지 못한 메서드를 가지고 있기 때문입니다.

객체로의 생성도 불가능한 추상클래스를 사용하는 이유는 상속을 받는 하위 클래스들의 틀로써 역할을 수행할 수 있기 때문입니다.

하위 클래스들에서 무조건 선언해야하는 클래스들의 모음을 추상 클래스에 선언부만 작성해놓으면 하위 클래스에서 오버라이딩하여 각자 세부 사항을 설계할 수 있으므로 효율적으로 코드를 작성할 수 있습니다.

```java
public class PlayerExample {
	public static void main(String[] args) {
    	Player song = new Song();
        Video video = new Video();
        song.brand();
        video.brand();
    }
}

abstract class Player {
	public String kind;
    public abstract void brand();
}

class Song extends Player {
	public Song() {
    	this.kind = "MP3 Player";
    }

    public void brand() {
    	System.out.println("IRIVER");
    }
}

class Video extends Player {
	public Video() {
    	this.kind = "Pad";
    }

    public void brand() {
    	System.out.println("Apple");
    }
}

/* ====== result =======
IRIVER
Apple
======================== */
```

예제를 작성하시다보면 하위 클래스가 상위 추상 클래스의 추상 메서드를 오버라이딩 하지 않았을 때 에러가 발생함을 알 수 있을 것입니다. 이렇게 추상 클래스로 하위 클래스들의 틀을 작성해 놓으면 구현헤야 할 메소드를 빠짐없이 오버라이딩하여 각각 상황에 맞는 메서드 구현을 가능하게 합니다.

추상화는 **상속계층도의 상층부에 위치할 수록 추상화의 정도가 높고 아래로 내려갈 수록 구체화**됩니다.

# 11. 인터페이스(interface)

인터페이스도 일종의 추상 클래스라고 할 수 있습니다. 하지만 앞서 배운 클래스와 추상 클래스의 상속은 다중 상속을 할 수 없습니다. 하지만 인터페이스는 다중 상속을 지원합니다. 추상 클래스와 마찬가지로 `클래스 설계도`의 역할을 수행합니다.

인터페이스는 추상 클래스보다 추상화의 정도가 더 높아서 멤버변수를 가질 수 없습니다. 인터페이스는 오직 `추상메서드`와 `상수`만 포함할 수 있습니다.

## 1. 인터페이스의 선언

인터페이스를 선언하는 방법은 클래스를 작성하는 것과 같습니다. 클래스를 작성하는 것에서 `class`를 `interface`로 작성해주시면 인터페이스로 선언됩니다.

```java
접근제어자 interface 인터페이스명 {
	public static final 타입 상수변수명 = data;
    public abstract 메서드명(매개변수 ,,,.);
}
```

## 2. 인터페이스의 구현

인터페이스의 구현은 상속을 사용할 때와 비슷합니다. 선언할 때와 마찬가지로 키워드만 변경해주면 되는데, `extends`키워드를 `implements`로 변경해주시면 됩니다.

```java
접근제어자 class 클래스명 implements 인터페이스명 {
	// 구현부
}

/* ====== 다중 구현 ======= */
접근제어자 class 클래스명 implements 인터페이스명1, 인터페이스명2, 인터페이스명3, ... {
	// 구현부
}

/* ====== 클래스상속과 인터페이스를 동시에 참조할 때 ====== */
접근제어자 class 클래스명 extends 상위클래스명 implements 인터페이스명 {
	// 구현부
}
```

위의 방식으로 필요에 따라 인터페이스와 클래스를 상속하여 사용하실 수 있습니다. (추상)클래스와 인터페이스를 동시에 상속받을 때는 클래스를 먼저 상속받고 인터페이스 상속을 후에 작성해주시면 됩니다.

인터페이스를 간단하게 구현한 예제를 살펴보겠습니다. ~~(예제소재를 새로운 것을 사용해보고 싶었는데 사용성에 있어서 적합하지 못해 보이는 점은 양해부탁드립니다,,)~~ '이런식으로 인터페이스를 상속받아 사용하는구나~' 정도로 봐주시면 감사하겠습니다 :)

```java
public class InterfaceEx {
	public static void main(String[] args) {
    	StudentKim kim = new StudentKim();
        BackSuPark park = new BackSuPark();

       kim.wakeUp();
       kim.goSchool();
       kim.hi();
       kim.study();
       System.out.println();

       park.wakeUp();
       park.hi();
    }
}

interface Person {
	public abstract void hi();
    public abstract void wakeUp();
}

interface Student {
	public abstract void goSchool();
    public abstract void study();
}

class StudentKim implements Person, Student {
	public void wakeUp() {
    	System.out.println("7시에 기상합니다.");
    }
    public void goSchool() {
    	System.out.println("등교합니다.");
    }
    public void hi() {
    	System.out.println("친구들과 인사합니다.");
    }
   public void study() {
   		System.out.println("수업시간에 공부를 합니다.");
   }
}

class BackSuPark implements Person {
	public void wakeUp() {
    	System.out.println("11시에 기상합니다.");
    }
    public void hi() {
    	System.out.println("거울보며 인사합니다.");
    }
}

/* ====== result ======
7시에 기상합니다.
등교합니다.
친구들과 인사합니다.
수업시간에 공부를 합니다.

11시에 기상합니다.
거울보며 인사합니다.
====================== */
```

## 3. 인터페이스의 장점

- 일관되고 정형화된 개발을 위한 표준화가 가능
- 개발시간 단축화(인터페이스를 작성해놓으면 메서드 호출하는 쪽에서 선언부만 알면 된다)
- 클래스의 관계성 생성가능
- 독립적인 프로그래밍 가능(구현부는 각 클래스에서 구현하기 때문에 필요에 맞게 구현부 작성가능)

# 📚 Reference

- [Java의 정석](https://product.kyobobook.co.kr/detail/S000001550352)
- [CodeStates](https://www.codestates.com/course/backend-engineering?gclid=Cj0KCQiAo-yfBhD_ARIsANr56g7yG-RoEgb1FYjnCetXZpz1jYm6-RtQCTqC_qgipon5_Aw8OyY4lj8aAizpEALw_wcB)
- [TCP School](http://tcpschool.com/java/intro)
