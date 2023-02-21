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

향상된 for문은 배열과 컬렉션의 요소 타입일 때 사용됩니다.

### 1. 향상된 for문의 구조

```java
for( 타입 변수명: 배열 또는 컬렉션 ) {
	// 반복할 문장
}
```

# 2. while문

# 3. do-while문

# 📚 Reference

- [Java의 정석](https://product.kyobobook.co.kr/detail/S000001550352)
- [TCP School](http://tcpschool.com/java/intro)
