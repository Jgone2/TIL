# 배열(array)

배열은 `같은 타입`의 여러 요소를 묶음으로 다루는 것을 말합니다. 만약에 매일의 기분을 체크하고 싶을 때 한달동안의 기분을 변수에 할당해두려면 우리는 30개의 변수를 생성해야합니다. 만약 1년동안 체크를 하게된다면 365개의 변수가 생성됩니다.  
이런 경우 배열을 사용하면 많은 양의 데이터를 손쉽게 다룰 수 있습니다. 다시한번 강조하고 싶은 점은 저장되는 데이터타입은 `같은 타입`이여야 한다는 것입니다.

```java
april1 = "Happy";
april2 = "Happy";
april3 = "sad";
april4 = "soso";
april5 = "crazy awesome!";
/* => 	배열로 전환 */
String[] todayMyMood = new String[]{"Happy", "Happy", "sad", "soso", "crazy awesome!"}
```

# 1. 배열의 선언과 초기화

## 1. 배열의 선언

배열을 선언하는 방법은 일반적인 변수 선언과 비슷합니다. 배열에 저장하고자 하는 데이터 타입을 선언하고 변수명 혹은 `대괄호[]`를 붙이면 됩니다. 대괄호는 데이터 타입 뒤, 변수명 뒤에 어디에나 붙여도 됩니다.

```java
// 타입[] 변수명
// 타입 변수명[]
int[] arrInt;
String[] name;
int score[];
String city[];
```

## 2. 배열의 초기화

배열을 초기화 하는 방법은 다음과 같습니다.

```java
String[] myMood;		// 배열의 선언
myMood = new String[5];	// 길이가 5인 String배열 초기화
String[] myMood = new String[5];
String[] myMood = {"sad", "happy", ...};
```

배열을 초기화하는 단계를 살펴보면 다음과 같습니다.

- String[] myMood;
  - String형 참조변수 myMood를 선언
- new String[5];
  - 연산자 new에 의해 메모리의 연속된 빈공간에 5개의 String형 데이터를 저장할 수 있는 공간 생성
  - 이 때 배열의 모든 요소는 String형의 기본값 `""`로 초기화
- String[] myMood = new String[5];
  - 대입 연산자에 의해 메모리의 연속된 빈 공간에 생성도니 배열의 첫번째 요소의 주소값이 참조변수 myMood에 할당
    - 참조변수 myMood는 배열의 첫 번째 요소를 가리킴

## 3. 배열이 참조변수인 이유

이 전에 언급한 변수들 중 String타입을 제외한 나머지 타입들은 기본 변수입니다. 하지만 String과 배열이 참조변수인 이유는 기본 타입의 경우, 값을 저장할 변수를 선언하는 시점에 메모리 사용공간을 컴퓨터가 알 수 있지만 참조변수는 컴퓨터가 얼마의 메모리 공간이 필요한지 알 수 없기 때문입니다.  
따라서 참조변수는 먼저 선언된 후에 할당되는 데이터값의 주소값을 가리키게 됩니다.

# 2. 배열의 길이와 인덱스

배열이 가진 각 저장공간을 `배열의 요소(element)`라고 하고 각 요소에는 배열의 `인덱스(index)`로 접근이 가능합니다. 배열의 길이는 배열이 가지고 있는 요소의 개수와 같습니다.

> 💡 인덱스(index)는 0부터 시작하기 때문에 '배열의 길이 -1'과 같은 값입니다.

배열의 길이는 `배열이름.length` 그리고 각 배열의 요소는 `배열이름[Index번호]`로 아래 예제처럼 접근 가능합니다.

```java
int[] arr = new int[] {1, 2, 3, 4, 5};
int sum = 0;
for(int i = 0; i < arr.length; i++) {	// 배열의 길이 -1만큼 순회
	sum += arr[i];	// 해당 index에 해당하는 arr의 요소를 sum에 +하여 대입
}
```

# 3. 다차원 배열

다차원 배열은 2차원 배열 이상을 의미합니다. 배열 속의 각 요소가 또 다른 배열일 뿐 1차원 배열과 크게 다른 점은 없습니다. 본 포스팅에서는 2차원 배열을 기준으로 포스팅 하겠습니다.

## 1. 2차원 배열의 선언과 초기화

2차원 배열을 선언하고 초기화 하는 것도 1차원 배열과 비슷합니다.  
한달 동안 하루에 세끼를 먹었는지 체크하고 기록하기 위한 예제로 2차원 배열의 선언과 초기화를 진행해보겠습니다.

```java
String [][] todayeat;
todayeat = new String[31][3];
// 1줄로 표기
String[][] todayeat = new String[31][3];
```

## 2. 2차원 배열의 탐색

2차원 배열 또한 반복문을 통해서 탐색이 가능합니다. 다만 배여링 중복으로 이루어져 있어 탐색시에도 중첩반복문을 사용해야 합니다.

```java
todayeat = new String[31][3];
for(int i = 0; i < todayeat.length; i++) {
	for(int j = 0; j < todayeat[i].length; j++) {
    	System.out.printf("todayeat[%d][%d] = %s", i, j, todayeat[i][j];
    }
}
```

# 📚 Reference

- [Java의 정석](https://product.kyobobook.co.kr/detail/S000001550352)
