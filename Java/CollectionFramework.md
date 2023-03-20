# 컬렉션 프레임워크(Collection Framework)

컬렉션 프레임워크는 데이터를 저장하는 클래스들을 표준화하여 설계해놓은 그룹입니다. 컬렉션 프레임워크에서는 특정 자료구조에 데이터를 추가하고, 삭제하는 등의 동작을 수행하는 메서드들을 제공합니다.

컬렉션 프레임워크는 인터페이스와 다형성을 이용한 객체지향적 설계를 통해 표준화 되어 있기 때문에 컬렉션 프레임워크를 사용하면 코드의 재사용성을 높일 수 있습니다.

# 1. 컬렉션 프레임워크의 구조

![컬렉션 프레임워크의 구조](https://velog.velcdn.com/images/jgone2/post/c33a8098-58f5-4382-b9e9-27b8f6ec6091/image.png)

- Colelction
  - List
    - 데이터의 순서 유지
    - 데이터 중복 허용
    - **구현 클래스**: ArrayList, LinkedList, Stack, Vector 등
  - Set
    - 데이터 순서 유지 ❌
    - 데이터 중복 허용 ❌
    - **구현 클래스**: HashSet, TreeSet 등
- Map
  - 키(key)와 값(value)의 쌍으로 이루어진 데이터 집합
  - 데이터 순서 유지 ❌
  - `키(key)`로 값을 식별
  - `키(key)`는 중복 허용 ❌
  - `값(value)`는 중복 허용
  - **구현 클래스**: HashMap, HashTable, TreeMap, Properites 등

`List`와 `Map`은 `Collection` 인터페이스에 포함되어 있지만 `Map`은 그렇지 않은 이유는 List와 Set은 많은 공통 부분을 공유하고 있기 때문에 Collection 인터페이스로 정의 가능하지만 Map은 key와 value로 구성되어있는 등 차이점이 있어서 List와 Set과 함께 Collection의 상속계층도에는 포함되어 있지 않습니다.

## 1. Collection Interface

`Collection` 인터페이스에서는 `List`와 `Set`에서 공통적으로 사용할 수 있는 메서드들이 정의되어 있습니다.

|   기능    | 반환 타입 | 메서드                                            | 설명                                                                                 |
| :-------: | --------- | ------------------------------------------------- | ------------------------------------------------------------------------------------ |
| 객체 추가 | boolean   | add(Object o)<br /> addAll(Collection c)          | 지정된 객체 및 컬렉션의 객체들을 Collection에 추가                                   |
| 객체 검색 | boolean   | contains(Obect o)<br /> containsAll(Collection c) | 지정된 객체 또는 컬렉션의 객체들이 Collection에 포함되어 있는지 확인                 |
| 객체 검색 | Iterator  | iterator()                                        | 컬렉션의 iterator를 반환                                                             |
| 객체 검색 | boolean   | equals(Object o)                                  | 동일한 컬렉션인지 여부 확인                                                          |
| 객체 검색 | boolean   | isEmpty()                                         | 빈 컬렉션인지 확인                                                                   |
| 객체 검색 | int       | size()                                            | 컬렉션에 저장된 객체의 개수 반환                                                     |
| 객체 삭제 | void      | clear()                                           | 컬렉션의 모든 객체 삭제                                                              |
| 객체 삭제 | boolean   | remove(Object o)<br /> removeAll(Collection c)    | 지정된 객체 및 컬렉션을 삭제                                                         |
| 객체 삭제 | boolean   | retainAll(Collection c)                           | 지정된 컬렉션에 포함된 객체를 제외한 모든 객체를 삭제하고, 컬렉션의 변화 유무를 반환 |
| 객체 변환 | Object[]  | toArray()                                         | 컬렉션에 저장된 객체를 객체배열로 반환                                               |
| 객체 변환 | Object[]  | toArray(Object[] o)                               | 주어진 배열에 컬렉션 객체를 저장해서 반환                                            |

## 2. List<E\>

List 인터페이스는 배열과 같이 객체를 일렬로 늘어놓은 구조를 하고 있습니다. List인터페이스는 데이터의 중복을 허용하며 저장순서가 유지됩니다.

### 1. ArrayList<E\>

`ArrayList`는 Object배열을 이용해서 데이터를 순차적으로 저장합니다. 저장된 객체는 마찬가지로 인덱스로 관리가 됩니다. 배열과의 차이점은 배열은 고정된 크기를 유지하지만 ArrayList는 초기 지정 용량을 초과하게되면 자동으로 용량이 늘어나며, 다음과 같이 생성할 수 있습니다.

```java
ArrayList<T> arrList = new ArrayList<T>();	// T에는 데이터 타입을 작성해주시면 됩니다. Ex) String, Integer ...
```

ArrayList에서는 객체를 추가하면 기본적으로 순차적으로 저장되며 특정 인덱스의 객체를 삭제하면 바로 뒤 인덱스부터 마지막 인덱스까지 모두 당겨지는 특징이 있습니다. 이러한 특징으로 인해 데이터를 순차적으로 입력/삭제할 때는 효율적이지만 중간의 데이터를 삭제, 추가 할 때는 뒤이어 언급할 `LinkedList`보다 처리속도가 느리다는 단점이 있습니다.

다음 로또번호 선택 예제를 통해 ArrayList 사용법을 알아보겠습니다.

```java
private static ArrayList<Integer> choiceMyNum(ArrayList<Integer> lotto, Scanner sc) {
        System.out.println("1부터 45까지의 숫자 6가지를 입력해 주세요.");
        for(int i = 0; i < 6;) {
            int num = sc.nextInt();
            if (num < 0 || num > 45) {
                System.out.println("범위 내의 수를 입력해 주세요.(1-45)");
            } else {
                if(!lotto.contains(num)) {
                    lotto.add(num);
                    i++;
                } else {
                    System.out.println("중복된 값입니다. 다시 입력해주세요.");
                }
            }
        }
        return lotto;
    }
```

위의 예제에서 `lotto.add(num)`을 사용하여 `ArrayList`에 추가 하고 있습니다. 다음은 `ArrayList`에 입력한 수를 수정하는 예제입니다.

```java
ArrayList<Integer> lotto = new ArrayList<>();
ArrayList<Integer> win = new ArrayList<>();
private static ArrayList<Integer> setNewValue(ArrayList<Integer> lotto, boolean isValid, Scanner sc) {
        while(isValid) {
            getMyNum(lotto);
            System.out.println("변경할 번호를 선택해주세요: (변경을 완료하셨다면 '0'을 눌러주세요.)");
            String nowValue = sc.next();
            if(nowValue.equals("0")) {
                isValid = false;
                break;
            }
            System.out.println("변경할 번호를 입력해주세요: ");
            String changeValue = sc.next();
            lotto.set(lotto.indexOf(Integer.parseInt(nowValue)), Integer.parseInt(changeValue));
            Collections.sort(lotto);
        }
        return  lotto;
    }
```

위의 예제에서 `lotto.set(lotto.indexOf(Integer.parseInt(nowValue)), Integer.parseInt(changeValue))`를 보시면 입력받은 nowValue를 Integer으로 형변환을 진행한다음 해당 value가 있는 index번호를 추출하고, 숫자를 변경하기 위해 입력한 changeValue를 Integer으로 형변환하여 매개변수로 넣어줍니다.  
즉, `.set(index번호, 수정할 value)`의 형태로 ArrayList의 특정 index의 값을 수정할 수 있습니다.

다음은 로또번호를 선택하고 1등 당첨여부를 알 수 있는 예제입니다.

```java
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        ArrayList<Integer> lotto = new ArrayList<>();
        ArrayList<Integer> win = new ArrayList<>();
        boolean isValid = true;

        printInfo();
        String checkChoiceType = sc.nextLine();

        while(isValid) {
            switch (checkChoiceType) {
                case "1":
                    getAutoNum(lotto);
                    isValid = false;
                    break;
                case "2":
                    choiceMyNum(lotto, sc);
                    isValid = false;
                    break;
                default:
                    System.out.println("잘못된 입력입니다. 로또번호를 선택하시겠습니까? (1)_예 (2)_아니오");
                    String input = sc.next();
                    if(input.equals("1")) {
                        printInfo();
                    } else {
                        isValid = false;
                    }
                    break;
            }
        }


        Collections.sort(lotto);
        checkMyNum(lotto);
        String fixNum = sc.next();
        if(fixNum.equals("2")) {
            System.out.println("변경을 완료하시면 '0'을 눌러주세요.");
            isValid = true;
            setNewValue(lotto, isValid, sc);
        }

        getAutoNum(win);
        Collections.sort(win);
        if(win.equals(lotto)) {
            System.out.println("1등 당첨을 축하드립니다!");
            printWinNum(lotto, win);
        } else {
            System.out.println("다음에 다시 시도해주세요 :)");
            printWinNum(lotto, win);
        }
    }

    private static void printWinNum(ArrayList<Integer> lotto,ArrayList<Integer> win) {
        getMyNum(lotto);
        System.out.println("\n=== 당첨번호 ===");
        for (Integer winNum : win) {
            System.out.print(winNum + " ");
        }
    }

    private static void getMyNum(ArrayList<Integer> lotto) {
        System.out.println("=== 내 번호 ===");
        for (Integer myNum : lotto) {
            System.out.print(myNum + " ");
        }
    }

    private static ArrayList<Integer> setNewValue(ArrayList<Integer> lotto, boolean isValid, Scanner sc) {
        while(isValid) {
            getMyNum(lotto);
            System.out.println();
            System.out.println("변경할 번호를 선택해주세요: (변경을 완료하셨다면 '0'을 눌러주세요.)");
            String nowValue = sc.next();
            if(nowValue.equals("0")) {
                isValid = false;
                break;
            }
            System.out.println("변경할 번호를 입력해주세요: ");
            String changeValue = sc.next();
            lotto.set(lotto.indexOf(Integer.parseInt(nowValue)), Integer.parseInt(changeValue));
            Collections.sort(lotto);
        }
        return  lotto;
    }

    private static void checkMyNum(ArrayList<Integer> lotto) {
        System.out.println("선택된 번호 입니다. 진행하시겠습니까? (1)_예 (2)_번호 변경");
        for (Integer myNum : lotto) {
            System.out.printf("%d ", myNum);
        }
        System.out.println();
    }

    private static void printInfo() {
        System.out.println("로또 번호 선택 타입을 지정해 주세요. (1)_자동 (2)_수동");
    }

    private static ArrayList<Integer> choiceMyNum(ArrayList<Integer> lotto, Scanner sc) {
        System.out.println("1부터 45까지의 숫자 6가지를 입력해 주세요.");
        for(int i = 0; i < 6;) {
            int num = sc.nextInt();
            if (num < 0 || num > 45) {
                System.out.println("범위 내의 수를 입력해 주세요.(1-45)");
            } else {
                if(!lotto.contains(num)) {
                    lotto.add(num);
                    i++;
                } else {
                    System.out.println("중복된 값입니다. 다시 입력해주세요.");
                }
            }
        }
        return lotto;
    }

    private static ArrayList<Integer> getAutoNum(ArrayList<Integer> lotto) {
        for (int i = 0; i < 6;) {
            int num = (int)(Math.random()* 45 + 1);
            if(!lotto.contains(num)) {
                lotto.add(num);
                i++;
            }
        }
        return lotto;
    }
```

수정할 때는 중복이 허용되는 등 오류가 있지만 arrayList에 값을 추가하고 수정해보는 용도로 봐주시면 감사하겠습니다.

### 2. LinkedList

`LinkedList`는 위에서 언급했듯이 `ArrayList`보다 값의 수정함에 있어서 빠르게 처리할 수 있어 효율적으로 수정할 수 있는 리스트 입니다. 앞서 배열과 `ArrayList`에서는 배열 내의 데이터들이 연속적으로 존재했지만 `LinkedList`내의 데이터들은 불연속적으로 존재합니다.

LinkedList에서 배열에서 index에 해당하는 것을 `Node`라고 하는데 이 각각의 `Node`들은 자신과 연결된 이전 요소 및 다음 요소의 주소값과 데이터로 구성되어 있습니다.

`Node`에 이전 요소 및 다음 요소의 주소값이 포함되어 있기 때문에 정보 수정, 추가, 삭제 시 `prev(이전 요소)`와 `next(다음 요소)`의 참조값만 변경해주면 되기 때문에 리스트 중간 부분의 데이터를 수정, 추가, 삭제 등을 수행할 때 효율적으로 수행가능 합니다.

그러나 데이터 검색 시에는 `LinkedList`를 사용하면 `prev`, `next`의 참조값을 사용하여 데이터를 순차적으로 탐색해야 하기 때문에 `ArrayList`를 사용할 때보다 비효율적이라는 단점도 존재합니다.

> 💡 **List**

- 데이터의 중복 허용 ⭕️
- 저장 순서 유지 ⭕️
- 중간 데이터의 잦은 수정: LinkedList
- 순차적 수정 및 탐색: ArrayList

## 3.Set<E\>

`Set()`은 `List`와 다르게 데이터의 중복을 허용하지 않고, 저장 순서를 유지하지 않는 컬렉션입니다.

### 1. HashSet

`HashSet`은 `Set`인터페이스를 구현한 가장 대표적인 컬렉션입니다.

```java
HashSet<String> hashSetEx = new HashSet<String>();

hashSetEx.add("Java");
hashSetEx.add("Java");

for(String s : hashSetEx) {
	System.out.println(s);
}

/* ====== result ======
Java
=======================*/
```

### 2. TreeSet

`TreeSet`은 **이진 탐색 트리(Binary Search Tree)** 형태로 데이터를 저장합니다. 마찬가지로 데이터 중복 저장을 허용하지 않고 저장 순서를 유지하지 않습니다.  
![](https://velog.velcdn.com/images/jgone2/post/75465c8b-88a1-4547-b214-c3fe1ee9b35a/image.jpg)

이진 탐색트리는 **하나의 부모 노드에 최대 두 개의 자식 노드와 연결되는 이진트리의 일종으로 정렬과 검색에 특화**된 자료구조 입니다.

왼쪽 자식 노드의 값이 부모 노드 보다 값이 작으며 오른쪽 자식의 값은 부모 노드보다 큰 값을 가지는 특징이 있습니다.

```java
TreeSet<String> lang = new TreeSet<>();

        lang.add("Java");
        lang.add("Spring");
        lang.add("Thymeleaf");

        System.out.println(lang);
        System.out.println(lang.first());
        System.out.println(lang.last());

/* ====== result ======
[Java, Spring, Thymeleaf]
Java
Thymeleaf
======================= */
```

> 💡 **Set**

- 데이터 중복 허용 ❌
- 저장 순서 유지 ❌

## 4. Key<K, V\>

`Map`은 `키(key)`와 `값(value)`으로 구성된 저장하는 구조를 가지고 있습니다. `Key`값을 가지고 있다는 점이 앞서 언급한 `List`와 `Set`과의 가장 큰 차이점입니다. 여기서 `key`와 `value`를 객체라고하고 이 객체를 **Entry객체**라고 합니다.

`Map`에서 중요한 점은 `Key`값은 Set처럼 데이터 중복 저장이 불가능하지만, `Value`는 중복 저장이 가능하다는 점입니다.

### 1. HashMap

`HashMap`은 해시 함수를 통해 `key`와 `value`가 저장되는 위치를 결정하므로, 사용자는 저장되는 위치를 알 수 없고 삽입되는 순서와 위치 또한 관계가 없습니다. HashMap은 해싱(Hashing)을 사용하기 때문에 **많은 양의 데이터를 검색하는데 있어서 뛰어난 성능**을 보입니다.

```java
HashMap<String, Integer> hashMap = new HashMap<>();
```

### 2. Hashtable

`Hashtable`은 `HashMap`과 내부 구조가 동일하며 사용방법도 유사합니다.

## 5. Iterator

`Iterator`는 컬렉션에 저장된 요소들을 순차적으로 읽어오는 역할을 수행합니다. Iterator인터페이스에 인터페이스를 반환하는 메서드인 `iterator()`를 호출하면, 컬렉션을 순회하여 반환합니다.

| 메서드    | 설명                                                                                      |
| --------- | ----------------------------------------------------------------------------------------- |
| hasNext() | 읽어올 객체가 남아 있으면 true를 리턴하고, 없으면 false를 리턴.                           |
| next()    | 하나의 객체를 읽음. next()를 호출하기 전 hasNext()로 다음 요소가 있는지 확인필요          |
| remove()  | next()를 통해 읽어온 객체를 삭제. remove()를 호출하기 전 next()를 호출한 다음에 호출필요. |

## 6. Collection Class

![](https://velog.velcdn.com/images/jgone2/post/8235cb3e-fed1-4ff9-a9a8-69b8a4c734a8/image.png)
