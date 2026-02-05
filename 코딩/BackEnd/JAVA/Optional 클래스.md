## Optional 클래스란?
- 값이 있을 수도 있고 없을 수도 있는 상황을 안전하게 처리하기 위한 Java클래스
- Optional은 **"이 값이 null일 수도 있다"는 것을 코드로 명시**하여 개발자가 안전하게 처리하도록 도와주는 도구


## Optional 주요 메소드
```java
// Optional 생성
Optional.of("값")           // 값이 반드시 있어야 함 (null이면 에러)
Optional.ofNullable(값)     // null일 수도 있는 값
Optional.empty()           // 빈 Optional -> 반환값: Optional.empty

// 값 확인
optional.isPresent()       // 값이 있으면 true
optional.isEmpty()         // 값이 없으면 true

// 값 꺼내기
optional.get()            // 값 반환 (없으면 에러)
optional.orElse("기본값")   // 값이 없으면 기본값 반환
```