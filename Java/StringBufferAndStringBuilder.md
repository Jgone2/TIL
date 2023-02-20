# String과 StringBuffer/StringBuilder의 차이

이 전 게시글 '[문자열](https://velog.io/@jgone2/Java-%EB%AC%B8%EC%9E%90%EC%97%B4%EA%B3%BC-%EB%AC%B8%EC%9E%90-%EC%9E%85%EC%B6%9C%EB%A0%A5String-IO)'에서 String의 특성과 함께 StringBuilder와 StringBuffer를 언급한 적이 있습니다. 이 전 게시글에서 소개했다시피 StringBuilder와 StringBuffer는 String인스턴스의 결합 작업이 자주 발생할 때 메모리공간을 추가적으로 계속 사용하게됩니다.  
반면에 StringBuilder와 StringBuffer는 내부적으로 문자열 편집을 위한 버퍼(buffer)를 가지고 있으며 인스턴스를 생성할 때 그 크기를 지정할 수 있습니다.

# 1. StringBuffer

StringBuffer클래스는 인스턴스를 생성할 때 문자열을 저장하고 편집하기 위한 공간(buffer)으로 사용됩니다. 인스턴스의 버퍼 크기를 지정해주지 않으면 default값으로 인스턴스 생성 시 사용자가 설정한 크기보다 언제나 16개의 문자를 더 저장할 수 있도록 여유있게 생성됩니다.  
String처럼 인스턴스를 계속 생성하지 않고 StringBuffer인스턴스를 사용해서 문자열을 바로 수정하고 저장하면 공간의 낭비가 없고 속도도 매우 빠릅니다.다음으로 StringBuffer과 함께 사용할 수 있는 메서드를 알아보겠습니다.

# 2. StringBuffer클래스의 인스턴스화

StringBuffer클래스는 아래의 모양으로 인스턴스화 시킬 수 있습니다.

```java
StringBuffer sb = new StringBuffer("java");
```

# 3. StringBuffer클래스의 메서드

## 1. append()

append() 메서드는 매개변수로 전달된 값을 문자열로 변환 후, 해당 문자열의 뒤에 추가합니다. 이 전에 본 String클래스의 concat() 메서드와 같은 결과를 반환합니다. 하지만 내부적인 처리 속돡 append()메서드가 훨씬 빠릅니다.

```java
StringBuffer str = new StringBuffer("Java");
str.append(" SpringFramework");
System.out.println(str);	// "Java SpringFramework"
```

## 2. capacity()

capacity() 메서드는 StringBuffer 인스턴스의 현재 버퍼 크기를 반환하는 메서드 입니다.

```java
StringBuffer sb1 = new StringBuffer();
StringBuffer sb2 = new StringBuffer("Java");
StringBuffer sb3 = new StringBuffer(10);
System.out.println(sb1.capacity());		// 16
System.out.println(sb2.capacity());		// 20
System.out.println(sb3.capacity());		// 10
```

## 3. delete()

delete() 메서드는 매개변수에 해당하는 인덱스 사이에 있는 문자열을 제거합니다. deleteCharAt() 메서드를 사용하면 지정된 index의 문자를 제거할 수 있습니다.

```java
StringBuffer sb = new StringBuffer("Java Spring");
System.out.println(sb.delete(4, 8));		// "Javaing"
System.out.println(sb.deleteCharAt(1));		// "Jvaing"
```

## 4. insert()

insert() 메서드는 매개변수로 전달된 인덱스 위치에 전달된 문자열을 추가합니다.

```java
StringBuffer sb = new StringBuffer("Java React!");
sb.insert(4, "Script");
System.out.println(sb);	// "JavaScript React!"
```

# 4. StringBuilder

StringBuffer는 멀티쓰레드(Multi-thread)에 안전하도록 동기화되어 있습니다. 그러나 동기화는 StringBuffer의 성능을 떨어뜨릴 수 밖에 없습니다. 그래서 멀티스레드로 작성된 프로그램이 아닌 경우 불필요하게 성능만 떨어뜨리게 됩니다.  
StringBuilder는 StringBuffer에서 쓰레드의 동기화만 제외하고 완전히 똑같은 기능으로 구성되어 있습니다. 하지만 반드시 필요한 경우가 아니고서는 일부러 사용할 필요는 없습니다.

```java
StringBuilder sb = new StringBuilder();
```
