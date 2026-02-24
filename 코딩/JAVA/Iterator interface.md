
- 컬렉션의 요소들의 ~={red}**내부 구조를 전혀 알지 못해도 순차적으로 접근 가능**=~하게 해주는 인터페이스이다
- Iterator는 안전한 순회이다(일반적인 for loops에서 컬렉션 요소 제거 작업을 하면 ConcurrnetModificationException 발생 할 수 있다. 하지만 Iterator의 remove()를 사용하면 Exception발생하지 않는다.)
- Java 5부터 도입된 enhanced for loop(for-each)는 실제로 Iterator를 사용한다<br><br>

***~={red}Iterator의 핵심 메소드=~***
1. **hasNext(): 다음 요소가 있는지 확인(boolean 리턴)**
2. **next(): 다음 요소 반환하고 커서 한 칸 앞으로 이동**
3. **remove(): 현재 가리키고 있는 요소를 제거(특정 Iterator만 지원)**

```java
//for-each문의 내부 동작
List<String> cololrs = Arrays.asList("빨강","파랑","노랑");

//for-each문
for(String color: colors){
	System.out.println(color);
}

//내부 동작 과정
Iterator<String> iterator = cololrs.iterator();
while(iterator.hasNext()){
	String color = iterator.next();
	System.out.println(color);
}
```

```java
//iterator 사용 예시
import java.util.*;

public class IteratorExample {
    public static void main(String[] args) {
        // 다양한 컬렉션 타입들
        List<String> list = Arrays.asList("사과", "바나나", "오렌지");
        Set<Integer> set = new HashSet<>(Arrays.asList(1, 2, 3, 4, 5));
        
        // List에서 Iterator 사용
        System.out.println("List 순회:");
        Iterator<String> listIterator = list.iterator();
        while (listIterator.hasNext()) {
            String fruit = listIterator.next();  // 다음 요소를 가져옴
            System.out.println("과일: " + fruit);
        }
        
        // Set에서 Iterator 사용 (동일한 패턴)
        System.out.println("\nSet 순회:");
        Iterator<Integer> setIterator = set.iterator();
        while (setIterator.hasNext()) {
            Integer number = setIterator.next();  // 다음 요소를 가져옴
            System.out.println("숫자: " + number);
        }
    }
}
```

#### 연습 문제

## 문제 1: 조건부 필터링 (쉬움)

주어진 정수 List에서 짝수만 남기고 홀수는 모두 제거하는 메서드를 작성하세요.

```java
public static void removeOddNumbers(List<Integer> numbers) {
    // Iterator를 사용하여 홀수를 안전하게 제거하세요
    // 힌트: hasNext(), next(), remove() 사용
}

// 테스트 예시
List<Integer> numbers = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10));
removeOddNumbers(numbers);
System.out.println(numbers); // [2, 4, 6, 8, 10]
```

## 문제 2: 중복 요소 제거 (보통)

연속된 중복 문자열을 제거하는 메서드를 작성하세요. 예를 들어 ["a", "a", "b", "c", "c", "c", "d"]에서 연속된 중복을 제거하면 ["a", "b", "c", "d"]가 되어야 합니다.

```java
public static void removeDuplicates(List<String> strings) {
    // Iterator를 사용하여 연속된 중복 요소를 제거하세요
    // 힌트: 이전 요소를 기억해두고 현재 요소와 비교
}

// 테스트 예시
List<String> words = new ArrayList<>(Arrays.asList("apple", "apple", "banana", "cherry", "cherry", "cherry", "date"));
removeDuplicates(words);
System.out.println(words); // [apple, banana, cherry, date]
```

## 문제 3: 교차 병합 (어려움)

두 개의 정렬된 List를 하나의 정렬된 List로 병합하는 메서드를 작성하세요. 각 List의 Iterator를 사용해야 하며, 결과는 새로운 List에 저장해야 합니다.

```java
public static List<Integer> mergeSortedLists(List<Integer> list1, List<Integer> list2) {
    // 두 List의 Iterator를 사용하여 정렬 순서를 유지하면서 병합하세요
    // 힌트: 두 Iterator를 동시에 사용하고, 현재 값을 비교하여 더 작은 값을 먼저 추가
    return new ArrayList<>();
}

// 테스트 예시
List<Integer> list1 = Arrays.asList(1, 3, 5, 7, 9);
List<Integer> list2 = Arrays.asList(2, 4, 6, 8, 10, 11, 12);
List<Integer> merged = mergeSortedLists(list1, list2);
System.out.println(merged); // [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
```