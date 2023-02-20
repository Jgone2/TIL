# 조건문(if, switch)

제어문은 코드의 실행흐름을 조건에 따라 변경하거나 반복함으로서 코드를 수행하는 역할을 하는 문장들을 말합니다. 그 중에서 조건문에는 if와 switch문 두가지가 존재합니다. 주로 if문이 많이 사용되지만, 상황에 따라 switch가 편리할 때가 있으므로 상황에 따라 사용해 주시면 됩니다.

# 1. if문

가장 기본적으로 많이 사용되는 조건문이며 if의 조건식이 `true`이면 if문 내의 문장를 수행합니다. if문은 다음과 같은 형태로 구성되어 있습니다.

```java
if (조건식) {
	// 조건식이 `true`일 때 수행되는 문장
}
```

예시로 정보처리기사 실기시험에 응시했을 때 60점 이상일 때 "합격입니다"를 출력하는 예제를 보겠습니다.

```java
if (score >= 60) {
	System.out.println("합격입니다");
}
```

if의 조건식에 `socre >= 60`이라는 조건을 입력했습니다. 이때 조건식에는 `boolean`값으로 평가될 수 있는 조건식을 입력해야합니다.

그렇다면 학점의 경우에 `A부터D, F`까지 학점이 있을 때 if문을 다음과 같이 5개를 사용해야합니다.

```java
if (90 <= score && score <= 100) System.out.println("A");	// if문의 수행문이 한 줄로 구성되어 있는 경우 {}를 사용하지 않아도 됩니다.
if(80 <= score && score < 90) System.out.println("B");
if(70 <= score && score < 80) System.out.println("C");
if(60 <= score && score < 70) System.out.println("D");
if(score < 60) System.out.println("F");
```

하지만 똑같이 if문을 계속 반복할 필요는 없습니다. Java에는 if문의 조건식이 `false`일 때 수행하는 역할을 할 수 있는 기능이 있습니다. 바로 if-else문과 if-else if문 입니다.

# 2. if-else문과 if-else if문

if-else문은 조건식의 결과에 따라 실행블록을 선택해서 실행합니다. if문의 조건식이 `true`이면 해당 블록의 코드가 수행되고, 조건식이 false면 다음으로 넘어가 else문의 수행문을 수행하게 됩니다.  
다시 정보처리기사 실기시험예제로 알아보겠습니다. 아까의 예제에 60점이상이 아닐 때 "불합격"을 출력해보겠습니다.

```java
if (score >= 60) {
	System.out.println("합격입니다");
} else {
	System.out.println("불합격");
}
```

이렇게 사용하더라도 하나의 조건식에 두 가지의 출력값을 출력할 수 있습니다. 하지만 학점과 같이 2개 이상의 조건에 대한 출력값이 필요할 때는 if-else문으로는 부족함이 느껴집니다. 이럴 때는 여러개의 조건식을 사용할 수 있는 `if-else if`문을 사용하면 됩니다.

```java
if(90 <= score && score <= 100) {
	System.out.println("A");
} else if(80 <= score) {
	System.out.println("B");
} else if(70 <= score) {
	System.out.println("C");
} else if(60 <= score) {
	System.out.println("D");
} else {
	System.out.println("F");
}
```

if문을 5개 사용할 때보다 훨씬 간결해진 것을 알 수 있습니다. if문의 조건식이 변경된이유는 위의 if문 예제에서는 100점을 입력했을 때 모든 if문을 하나씩 참조하게 됩니다.

```java
if (90 <= score && score <= 100) System.out.println("A");
if(80 <= score) System.out.println("B");
if(70 <= score) System.out.println("C");
if(60 <= score) System.out.println("D");
if(score < 60) System.out.println("F");
```

if-else if문처럼 위의 조건식을 적용 했을 때 하나씩 참조하면 `80 <= score`, `70 <= score`, `60 <= score`에 모두 해당이 되기 때문에 A부터 D까지 모두 출력이 됩니다.  
그러나 if-else if문을 사용하면 if의 조건식이 `false`이면 else if의 조건식을 참조하기 때문에 조건도 훨씬 간단하게 작성할 수 있고 `80 <= score`조건식에 부합한다면 "B"를 출력하고 아래의 else-if문은 순회하지 않게 됩니다. 그러므로 if-else if문을 사용하는 것이 훨씬 효율적인 방식이라고 할 수 있습니다.

> 💡 중첩 if문
> if문은 중첩으로 사용이 가능합니다

```java
if(조건식) {
	if(조건식) {
    	// 수행문
    } else {
    	// 수행문
    }
}
```

# 3. switch문

switch문도 if문과 마찬가지로 조건 제어문입니다. 하지만 사용상황이 다릅니다. if문이 조건식의 `true/false`일 경우에 따라 내부의 실행문이 수행되는 반면 switch는 매개변수가 어떤 값을 갖느냐에 따라 실행문이 수행됩니다.  
또한, 조건식의 경우의 수가 많을 때 if문을 사용하는 것보다 훨씬 간결하게 표현할 수 있습니다. 그러나 switch문은 사용함에 있어서 제약조건이 있기 때문에, 경우에 따라서 경우의 수가 많아도 if문으로 작성해야 하는 경우가 있다.

```java
int score = 97;
switch(score) {
	case 90: System.out.print("A-");
    case 91: System.out.print("A-");
    case 92: System.out.print("A-");
    case 93: System.out.print("A-");
    case 94: System.out.print("A0");
    case 95: System.out.print("A0");
    case 96: System.out.print("A0");
    case 97: System.out.print("A+");
    case 98: System.out.print("A+");
    case 99: System.out.print("A+");
    case 100: System.out.print("A+");
}
```

위의 예제를 출력하면 어떻게 될까요? "A+"이 출력될 것 같지만 A+A+A+A+ 이렇게 출력됩니다. case 97부터 100까지 순회하면서 모두 출력하기 때문입니다. 이를 방지하기위해 switch문에는 `break`를 사용해서 switch문 전체를 빠져나가야 합니다.

```java
case 97: System.out.print("A+"); break;
```

이렇게 `break`를 선언해야 조건에 적합한 case문을 수행하고 switch문을 탈출 할 수 있습니다. 또한 switch문 내의 조건식에 부합한 인자값이 들어왔을 때 처리할 수 있는 `default`문이 있습니다.

```java
case 100: System.out.print("A+"); break;
default: System.out.print("잘못된 입력값"); break;
```

사칙연산을 예제로 `break`와 `defaul`를 추가 해보겠습니다.

```java
char oper = '+';
int num1 = 1;
int num2 = 2;
switch(oper) {
	case '+': return nm1 + num2;
    case '-': return nm1 - num2;
    case '*': return nm1 * num2;
    case '/': return nm1 / num2;
    default: return "잘못된 입력값";
}
```

위의 예제에서는 break를 사용하지 않았는데 따로 변수에 값을 담지 않고 return을 사용해서 메서드를 종료시켰기 때문에 break를 따로 사용할 필요가 없습니다.

# 📚 Reference

- [Java의 정석](https://product.kyobobook.co.kr/detail/S000001550352)
- [TCP School](http://tcpschool.com/java/intro)
