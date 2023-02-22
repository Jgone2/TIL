# 반복문(for, while, do-while)

반복문은 특정 작업이 반복적으로 수행되도록 할 때 사용되며 반복문의 종류로는 `for`문 `while`문 그리고 `do-while`문이 있습니다. for문과 while문은 서로 상호 변환이 가능하기 때문에 둘 중 어느 쪽을 선택해도 좋습니다.

# 1. for문

for문은 반복 횟수를 알고 있을 때 적합합니다. 조건식이 `true`인 동안 실행문을 반복적으로 수행합니다.

## 1. for문의 구조

for문의 조건식에는 `초기화`, `조건식`, `증감식`이 있습니다. 제일 먼저 '초기화'가 수행되고 '조건식' -> '수행문' -> '증감식'의 순서로 반복된다. 반복하다가 조건식이 `false`가 되면 for문을 빠져나가게 된다.

```java
for(초기화; 조건식; 증감식) {
	// 수행문
}
```

### 1. 초기화

- 반복문에 사용될 변수를 초기화하는 부분이며 처음에 한번만 수행

### 2. 조건식

- 조건식의 값이 `true`이면 반복을 계속, `false`이면 반복을 중단하고 for문을 벗어남
- 조건식을 잘못 작성하면 수행문이 한 번도 수행되지 않거나 무한반복에 빠질 수 있기 때문에 주의 요망

### 3. 증감식

- 반복문을 제어하는 변수의 값을 증가(++)또는 감소(--)시키는 식
- 증감식에 의해서 변수의 값이 변하다가 `false`가 되어 for문을 벗어남
- 보통 증감 연산자가 사용되지만 다양한 연산자들로 증감식 작성 가능

## 2. for문 사용

```java
for(int i = 1; i <= 5; i++) {
	System.out.println("This is for문");
}

for(int i = 1; i <= 5; i++) {
	System.out.print(i);
}

for(int i = 1; i <= 10; i++) {
	int sum += i;
    System.out.printf("1부터 %2d까지의 합: %2d", i, sum);
}
```

## 3. 향상된 for문(Enhanced for문)

향상된 for문은 배열과 컬렉션의 요소 타입일 때 사용됩니다. 배열 및 컬렉션 항목의 개수만큼 반복합니다.

```java
for( 타입 변수명: 배열 또는 컬렉션 ) {
	// 반복할 문장
}
/* == 예제  == */
String[] stacks = {"spring", "mysql", "mariadb"}
for(String stack = stacks) {
	System.out.println("OO님은 " + stack + "을 공부하고 있습니다");
}
```

배열 예제로 향상된 for문을 사용해 보았습니다. 위의 예제처럼 향상된 for문은 배열이나 컬렉션의 요소들을 읽어오는 용도로만 사용할 수 있다는 제약이 있습니다.

# 2. while문

while문은 for문에 비해 구조가 간단합니다. while문은 조건식의 값이 `true`일 경우 무한루프가 되기 때문에 주의해야 합니다.

```java
while(조건식) {
	// 조건식의 연산결과가 `true`인 동안, 반복 수행
}
```

1부터 10까지 덧셈을 하는 예제를 보겠습니다.

```java
int num = 0;
int sum = 0;
while(num < 10){
	num++;
    sum += num;
}
```

위의 예제 처럼 조건문을 잘 입력한다면 설계한대로 while문이 10번 순환하고 종료됩니다.

```java
Scanner sc = new Scanner(System.in);
int result = 0;
while(true) {
	int num1 = sc.nextInt();
    int num2 = sc.nextInt();
	char oper = input.next().charAt(0);
	switch(oper){
    	case '+': return num1 + num2; break;
        case '-': result = num1 - num2; break;
        case '*': result = num1 * num2; break;
        case '/': result = num1 / num2; break;
        default: result = "잘못된 입력값입니다."; break;
    }
    System.out.println(result);
    result = 0;
}
```

위의 예제같은 경우에는 계산기능을 구현한 예제입니다. 계산을 여러번 하려고 while에 true를 입력했지만 종료할 수 없는 무한 루프에 빠지게 되었습니다.  
for문은 조건식과 함께 증감식을 작성하지만 while문은 그렇지 않기에 잊지않고 종료할 수 있는 프로세스를 만들어 주셔야합니다.

# 3. do-while문

do while문은 while문의 변형으로 조건식에 의해 반복실행한다는 점은 while문과 동일합니다. 하지만 do-while의 경우 `{}`안의 내용을 먼저 실행시키고 조건식을 검사한다는 점에서 for문 그리고 while문과 차이가 있습니다.

```java
do{
	// 실행문(처음 한번은 무조건 실행 된다)
} while(조건식);
```

# 📚 Reference

- [Java의 정석](https://product.kyobobook.co.kr/detail/S000001550352)
- [TCP School](http://tcpschool.com/java/intro)
