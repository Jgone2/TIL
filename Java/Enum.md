# 열거형(enum)

열거형은 **서로 관련된 상수들을 편리하게 선언/관리**할 수 있게 해주는 문법요소입니다. 상수라고 하면 변하지 않는 값으로 `final`키워드를 사용하여 선언할 수 있습니다. 열거형은 많은 상수를 사용할 때 관련된 그룹으로 묶어 편리하게 관리 할 수 있습니다.

```java
public static final int SPRING = 1;
public static final int SUMMER = 2;
public static final int FALL = 3;
public static final int WINTER = 4;

public static final int SPRING = 1;	// 계절의 SPRING과 중복 발생
public static final int NEST = 2;
public static final int DENO = 3;
public static final int DJANGO = 4;
```

위의 예제처럼 상수를 나열했을 때 같은 상수명을 가졌기 때문에 컴파일 에러가 발생하게 됩니다. 이와 같은 문제를 보완한 것이 바로 `열거형(enum)`입니다.

# 1. 열거형의 정의

열거형을 정의하는 방법은 매우 간단합니다.

```java
enum 열거형이름 { 상수명1, 상수명2, ... }

enum Seasons { SPRING, SUMMER, FALL, WINTER }
enum School { ELEMENTARY, MIDDLE, HIGH, UNIVERSITY }
```

위의 열거형 Seasons을 일반 상수로 나타내면 다음과 같습니다.

```java
class Seasons {
	static final Seasons SPRING = new Seasons("SPRING");
    static final Seasons SUMMER = new Seasons("SUMMER");
    static final Seasons FALL = new Seasons("FALL");
    static final Seasons WINTER = new Seasons("WINTER");
}
```

또한, 열거형을 정의하면 각 상수들에는 자동적으로 0부터 시작하는 정수값이 할당됩니다. 이는 배열의 index와 비슷하게 생각해주시면 되는데, 열거형의 정수값은 상수명(정수값)으로 직접 설정할 수 있습니다.

# 2. 열거형의 사용

열거형을 사용하는 방법은 `열거형이름.상수명`으로 사용할 수 있습니다.

```java
enum Menu { RAMEN, KIMBAB, PIZZA, HAMBURGER, CHICKEN }

public class MenuTest {
	public static void main(String[] args) {
    	Menu myMenu = Menu.PIZZA;
        System.out.println(myMenu);
    }
}
```

또한 열거형은 switch문의 case로 사용할 수 있습니다.

```java
enum Seasons { SPRING, SUMMER, FALL, WINTER }

public class EnumSwitch {
	public static void main(String[] args) {
    	Seasons nowSeason = Seasons.SPRING;
        System.out.println(nowSeason);
        switch(nowSeason) {
        	case SPRING:
            	System.out.println("현재는 봄입니다.");
                break;
            case SUMMER:
            	System.out.println("현재는 여름입니다.");
            	break;
            case FALL:
            	System.out.println("현재는 가을입니다.");
            	break;
            case WINTER:
            	System.out.println("현재는 겨울입니다.");
            	break;
        }
    }
}

/* ====== result ======
SPRING
현재는 봄입니다.
======================= */
```

변수에 값을 대입할 때는 열거형이름.상수명으로 사용했지만 switch문의 case에서는 상수명만 입력받는 데 주의해서 사용해주시면 됩니다. 열거형이름.상수명을 사용했을 때는 컴파일 에러가 발생했습니다.

## 1. 열거형에서 사용할 수 있는 메서드

모든 열거형은 `java.lang.Enum`의 하위 클래스 입니다. 클래스에서 최상위 클래스인 `Object`클래스에 정의된 메서드를 사용할 수 있었던 것 처럼 열거형에서도 `java.lang.Enum`에 선언되어 있는 메서드를 사용할 수 있습니다.

| method            | 설명                                       |
| ----------------- | ------------------------------------------ |
| name()            | 열거형 상수명을 문자열로 반환              |
| ordinal()         | 열거형 상수의 정의 순서를 반환(0부터 시작) |
| compareTo(비교값) | 매개값과 비교해서 순번 차이를 리턴         |
| valueOf(name)     | 문자열의 열거형 객체를 리턴                |
| values()          | 모든 열거 객체들을 배열로 리턴             |

위의 메서드들을 사용해서 제가 뭘 먹고싶은지 맞춰보는 예제를 만들어 보았습니다.

```java
enum Menu {
    RAMEN,
    KIMBAB,
    PIZZA,
    HAMBURGER,
    CHICKEN
}

public class DoYouKnowWhatIWant {
    public static void main(String[] args) {
        Menu iWannaEat = Menu.PIZZA;
        int count = 3;
        boolean isValid = true;

        Scanner sc = new Scanner(System.in);
        System.out.println("기회는 3번입니다.");
        while(isValid) {
            printMenu();
            System.out.printf("남은 기회: %d\n", count);
            System.out.println("제가 먹고 싶어하는 메뉴를 맞춰보세요(정확한 메뉴명을 입력해야 합니다.): ");
            Menu menu = Menu.valueOf(sc.next());

            if (iWannaEat.equals(menu)) {
                System.out.println("정답!");
                System.out.println("프로그램을 종료합니다 :)");
                return;
            } else {
                count--;
                if (ifCountZero(count)) break;
                System.out.println("오답!");
                System.out.println("다시 도전하시겠습니까?? (1)_재도전 (2)_나가기");
                String input = sc.next();
                if(input.equals("1")) continue;
                break;
            }
        }

    }

    private static boolean ifCountZero(int count) {
        if(count == 0) {
            System.out.println("실패!");
            System.out.println("다음에 도전해 주세요!");
            return true;
        }
        return false;
    }

    private static void printMenu() {
        Menu[] allMenu = Menu.values();
        System.out.println("🧾 MENU");
        System.out.println("=".repeat(10));
        for (Menu menu : allMenu) {
            System.out.printf("%-2d번   %s\n", menu.ordinal()+1, menu.name());
        }
        System.out.println("=".repeat(10));
    }
}
```

위의 예제를 보시면 제가 먹고싶은 메뉴를 열거형 객체 중에 `PIZZA`를 선택해서 참조변수 `iWannaEat`에 대입한 후 메서드 `printMenu()`에서 `values()`를 사용해서 `Menu`에 정의된 모든 상수를 배열로 반환한 것을 볼 수 있습니다.

반환한 배열을 for문을 통해서 메서드 `ordinal()`, `name()`을 통해서 각 상수의 정수값과 상수명을 출력하고 Scanner로 문자열을 입력받은 다음 `valueOf()`를 사용하여 문자열과 동일한 객체를 menu에 대입했습니다.

그 후 참조변수 iWannaEat과 입력받은 참조변수 menu를 비교하여 결과를 출력하도록 설계했습니다.

# 📚 Reference

- [Java의 정석](https://product.kyobobook.co.kr/detail/S000001550352)
- [CodeStates](https://www.codestates.com/course/backend-engineering?gclid=Cj0KCQiAo-yfBhD_ARIsANr56g7yG-RoEgb1FYjnCetXZpz1jYm6-RtQCTqC_qgipon5_Aw8OyY4lj8aAizpEALw_wcB)