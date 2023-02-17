# 문자열(String)

이전에 [변수와 자료형](https://velog.io/@jgone2/Java-%EB%B3%80%EC%88%98%EC%99%80-%ED%83%80%EC%9E%85Variable-Type)에서 말씀드렸다 싶이 특이한 점이 있습니다.  
다른 기본형 변수 타입들(`int, char, long, double ... 등`)과 똑같은 모양으로 값을 대입 시킬 수 있으나 다른 변수 타입들과는 다르게 `String`은 첫 글자가 대문자로 시작됩니다.  
대문자로 되어 있는 이유는 추후 객체, 클래스 개념을 다룰 때 설명드리겠습니다. 현재는 단순히 '일반 기본형과는 다르구나'라고 생각해주시면 좋을 것 같습니다.

# 1. String 타입의 변수 선언과 할당 방법

String 타입은 '문자를 이어붙인 것'과 같은 형태를 하고 있습니다. 자바에서는 char보다 String 클래스를 더 많이 사용합니다 그 이유는 문자형에서 제공하는 기능(메서드, method)보다 훨씬 더 많은 기능이 추가됐기 때문입니다.  
String 클래스를 선언하고, 값을 할당하는 방법은 다음과 같습니다.

```java
String str = "Hello";
String str1 = new String("Hello");
```

우리는 변수 `str`의 경우처럼 String에 문자열을 할당합니다. 하지만 new연산자를 사용해서 객체형을 생성해서 변수에 할당할 수도 있습니다. 이렇게만 본다면 선언방법이 여러가지가 있구나! 라고 생각하실 수 있습니다.  
그러나 [연산자](https://velog.io/@jgone2/Java-%EC%97%B0%EC%82%B0%EC%9E%90Operator)에서 언급한 비교 연산자로 값을 비교해본다면 차이를 알 수 있습니다.

# 2. String클래스의 비교

String클래스 타입의 변수 str1, str2... str5에 각 방식으로 문자열 "abcd e"를 입력해 보겠습니다.

```java
String str1 = "abcd e";
String str2 = "abcd e";
String str3 = new String("abcd e");
String str4 = new String("abcd e");

boolean result1 = str1 == str2;
boolean result2 = str1 == str3;
boolean result3 = str3 == str4;

System.out.println(result1);	// str1 == str2: true
System.out.println(result2);	// str1 == str3: false
System.out.println(result3);	// str3 == str4: false
```

위를 보시면 모두 같은 값을 할당했는데 비교 연산자를 사용했을 때 false로 나오는 경우가 있습니다.
![String 객체생성에 따른 차이 비교](https://velog.velcdn.com/images/jgone2/post/50f91a83-7b3b-4b59-8b86-2d9e1c13125d/image.jpeg)

그 이유는 String클래스를 저장하면 메모리의 heap영역내에 `String Constant Pool`에 저장되기 때문입니다. `str1`과 같이 리터럴로 String 클래스를 생성하면 `String Constant Pool`에서 같은 값이 있다면 새로 생성하지 않고 값을 재사용합니다.  
그러나 new연산자를 사용하여 생성하면 각 객체를 생성하여 그 객체 안에 문자열을 생성하기 때문에 `str3`과 `str4`가 서로 가리키고 있는 주소값이 다르기 때문입니다.  
때문에 일반 리터럴문자열을 입력한 `str1`과 `str3`도 같은 맥락에서 결과값 result가 `false`로 출력 됩니다.

# 3. String 클래스의 주요 메서드

String클래스에서는 많은 문자열 관련 메서드를 제공해 줍니다. 그 중에서 간단하게 주요 메서드 몇개를 소개해 드리겠습니다.

| 메서드      | 설명                                                                  |
| ----------- | --------------------------------------------------------------------- |
| charAt()    | 문자열의 특정 위치(인덱스, Index)에 있는 문자를 반환                  |
| length()    | 문자열의 길이를 반환                                                  |
| substring() | 문자열의 해당 범위에 있는 문자열을 반환                               |
| equals()    | 문자열의 내용이 다른 객체와 같은지 확인 후 결과를 반환                |
| indexOf()   | 문자열에서 특정 문자나 문자열이 처음으로 등장하는 index의 위치를 반환 |
| concat()    | 해당 문자열의 뒤에 인수로 전달된 문자열을 추가해서 반환               |

## 1. charAt()

charAt()메서드는 해당 문자열의 특정 index에 있는 문자를 char형으로 반환합니다.

```java
String str1 = "Java";

System.out.println(str1.charAt(0)); // str1.charAt(0): 'J'
System.out.println(str1.charAt(1)); // str1.charAt(1): 'a'
System.out.println(str1.charAt(2)); // str1.charAt(2): 'v'
System.out.println(str1.charAt(3)); // str1.charAt(3): 'a'
```

위의 결과를 보면 알 수 있듯이 charAt(index번호)를 사용하면 해당 Index에 할당된 문자를 char형으로 반환받을 수 있습니다.

> 💡 index번호는 0부터 시작합니다.

## 2. length()

length()메서드는 해당 문자열의 길이를 Int형으로 반환합니다.

```java
String str1 = "Java";
String str2 = "Hello World!";

System.out.println(str1.length());	// 4
System.out.println(str2.length());	// 12
```

예제를 보면 `java`는 길이 4, `Hello World!`는 12로 출력되는 걸 알 수 있습니다. `str2`를 출력해보면 공백도 하나의 index공간을 차지하고 있다는 점을 알 수 있습니다.

## 3. subsring()

substring()메서드는 ()에 index 범위를 설정하여 범위에 해당하는 String 문자열을 반환합니다.

```java
String str = "Hello JavaAndSpring!";

System.out.println(str.substring(2, 7));	// "llo J"
System.out.println(str.substring(7, 14));	// "avaAndS"
System.out.println(str.substring(13, 19));	// "Spring"
System.out.println(str.substring(2));		// "llo JavaAndSpring!"
System.out.println(str.substring(10));		// "AndSpring!"
```

예제에서처럼 인자값을 하나만 입력할 시 해당 index번호부터 해당 문자열 끝까지 반환합니다.

## 4. equals()

equals()메서드는 해당 문자열의 내용이 다른 객체와 같은지 확인 후 결과를 boolean형으로 반환합니다.

```java
String str1 = "Spring";
String str2 = new String("Spring");
String str3 = new String("Spring");

System.out.println(str1.equals(str2));		// true
System.out.println(str2.equals(str3));		// true
System.out.println(str2.equals("Spring"));	// true
```

이 예제는 아까 `Spring클래스의 비교`챕터에서 본 예제와 비슷합니다. 아까의 예제와 다르게 equals()메서드를 사용했을 때는 모두 true가 출력된 것을 알 수 있습니다.  
이 전의 예제와 비교해보겠습니다.

```java
String str1 = "abcd e";
String str2 = "abcd e";
String str3 = new String("abcd e");
String str4 = new String("abcd e");

boolean result1 = str1 == str2;
boolean result2 = str1 == str3;
boolean result3 = str3 == str4;

System.out.println(result1);	// str1 == str2: true
System.out.println(result2);	// str1 == str3: false
System.out.println(result3);	// str3 == str4: false
```

확연한 차이가 있습니다. 차이가 나는 이유는 비교 연산자 `==`는 두 대상이 할당되어있는 메모리의 주소값을 비교하고, `equals()`메서드는 두 대상의 Data값 자체를 비교하기 때문에 차이가 발생합니다.

## 5. indexOf()

indexOf()메서드는 해당 문자열에서 특정 문자나 문자열이 처음 등장하는 index의 위치를 반환합니다. 만약 해당 문자열에 포함되어 있지 않다면 `-1`을 반환합니다.

```java
String str = "Java Spring MySQL MariaDB";

System.out.println(str.indexOf("s"));		// -1
System.out.println(str.indexOf("S"));		// 5
System.out.println(str.indexOf("a"));		// 1
System.out.println(str.indexOf("iaD"));		// 21
System.out.println(str.indexOf("Maria"));	// 18
```

출력 값에서 대문자`S`는 5가 출력된거에 반해 소문자`s`는 -1이 출력된 것을 볼 수 있습니다. indexOf()메서드는 대/소문자를 구분합니다.

## 6. concat()

concat()메서드는 해당 문자열의 뒤에 인수로 전달된 문자열을 추가한 새로운 문자열을 반환합니다.

```java
String str = "Hello ";

System.out.println(str.concat("Jgone2"));	// "Hello Jgone2"
System.out.println(str.concat("Java"));		// "Hello Java"
System.out.println(str.concat("Spring"));	// "Hello Spring"
```

# 📚 Reference

- [Java의 정석](https://product.kyobobook.co.kr/detail/S000001550352)
- [TCP School](http://tcpschool.com/java/intro)
- [Random Access Memories](https://starkying.tistory.com/entry/what-is-java-string-pool)
