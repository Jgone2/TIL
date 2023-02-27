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
        this.cusinfo = cusinfo;
    }
}
```
