# Java 8 default 키워드 완전 가이드

## default 키워드란?

Java 8에서 추가된 `default` 키워드는 **인터페이스에 구현된 메소드를 정의**할 수 있게 해주는 기능입니다. 기존에는 인터페이스에 추상 메소드만 선언할 수 있었지만, 이제 실제 구현도 포함할 수 있습니다.

## 기존 인터페이스의 문제점

```java
// Java 7 이전 방식
interface Vehicle {
    void start();
    void stop();
    // 새로운 메소드를 추가하면?
    // void honk(); // 모든 구현 클래스에서 컴파일 에러!
}

class Car implements Vehicle {
    public void start() { System.out.println("자동차 시동"); }
    public void stop() { System.out.println("자동차 정지"); }
}
```

**문제:** 인터페이스에 새 메소드를 추가하면 모든 구현 클래스가 깨집니다.

## default 키워드로 해결

```java
// Java 8 방식
interface Vehicle {
    void start();
    void stop();
    
    // default 메소드 - 기본 구현 제공
    default void honk() {
        System.out.println("빵빵!");
    }
    
    default void showInfo() {
        System.out.println("이것은 탈것입니다.");
    }
}

class Car implements Vehicle {
    public void start() { System.out.println("자동차 시동"); }
    public void stop() { System.out.println("자동차 정지"); }
    // honk() 메소드 구현 안 해도 됨!
}

class Motorcycle implements Vehicle {
    public void start() { System.out.println("오토바이 시동"); }
    public void stop() { System.out.println("오토바이 정지"); }
    
    // 원한다면 오버라이드 가능
    @Override
    public void honk() {
        System.out.println("빵! 빵!");
    }
}
```

## 실제 사용 예시

```java
public class Main {
    public static void main(String[] args) {
        Vehicle car = new Car();
        Vehicle motorcycle = new Motorcycle();
        
        car.start();        // 자동차 시동
        car.honk();         // 빵빵! (default 구현 사용)
        car.showInfo();     // 이것은 탈것입니다.
        
        motorcycle.start(); // 오토바이 시동
        motorcycle.honk();  // 빵! 빵! (오버라이드된 구현)
    }
}
```

## default 메소드의 특징

### 1. 인터페이스 진화 가능

```java
interface PaymentProcessor {
    void processPayment(double amount);
    
    // 나중에 추가된 메소드
    default void sendNotification() {
        System.out.println("결제 완료 알림 발송");
    }
}
```

### 2. 다중 상속 문제 해결

```java
interface A {
    default void hello() { System.out.println("A에서 안녕"); }
}

interface B {
    default void hello() { System.out.println("B에서 안녕"); }
}

// 충돌 발생 시 명시적으로 선택해야 함
class MyClass implements A, B {
    @Override
    public void hello() {
        A.super.hello(); // A의 구현 선택
        // 또는 B.super.hello(); // B의 구현 선택
        // 또는 새로운 구현 작성
    }
}
```

### 3. 유틸리티 메소드 제공

```java
interface MathUtils {
    // 추상 메소드
    double calculate(double x, double y);
    
    // 공통 유틸리티 메소드
    default boolean isPositive(double number) {
        return number > 0;
    }
    
    default double abs(double number) {
        return number < 0 ? -number : number;
    }
}
```

## 실무에서의 활용

### Collection 인터페이스 예시

```java
List<String> names = Arrays.asList("김철수", "이영희", "박민수");

// Java 8에서 추가된 default 메소드들
names.forEach(System.out::println);  // forEach는 default 메소드
names.stream().filter(name -> name.contains("김"));  // stream도 default 메소드
```

### 사용자 정의 인터페이스 예시

```java
interface Logger {
    void log(String message);
    
    // 레벨별 로깅 - default 메소드로 편의성 제공
    default void info(String message) {
        log("[INFO] " + message);
    }
    
    default void error(String message) {
        log("[ERROR] " + message);
    }
    
    default void debug(String message) {
        log("[DEBUG] " + message);
    }
}

class ConsoleLogger implements Logger {
    @Override
    public void log(String message) {
        System.out.println(message);
    }
    // info, error, debug 메소드는 자동으로 사용 가능
}
```

## 주의사항

### 1. static 메소드와의 차이

```java
interface Utils {
    // static 메소드 - 인터페이스명으로 직접 호출
    static void staticMethod() {
        System.out.println("정적 메소드");
    }
    
    // default 메소드 - 구현체 인스턴스로 호출
    default void defaultMethod() {
        System.out.println("디폴트 메소드");
    }
}

// 사용법
Utils.staticMethod();           // 정적 메소드 호출
MyClass obj = new MyClass();
obj.defaultMethod();            // 디폴트 메소드 호출
```

### 2. Object 클래스 메소드는 재정의 불가

```java
interface MyInterface {
    // 컴파일 에러! Object의 메소드는 default로 재정의 불가
    // default String toString() { return "MyInterface"; }
    
    // 이건 가능
    default void myMethod() { }
}
```

## 언제 사용해야 할까?

✅ **좋은 사용 사례:**

- 기존 인터페이스에 새 기능 추가시
- 공통된 구현이 있는 편의 메소드
- 하위 호환성 유지가 필요할 때

❌ **피해야 할 사용 사례:**

- 복잡한 비즈니스 로직
- 상태를 변경하는 메소드
- 너무 많은 default 메소드 (인터페이스가 클래스화됨)

## 마무리

`default` 키워드는 인터페이스의 진화를 가능하게 하면서도 하위 호환성을 유지하는 Java 8의 핵심 기능입니다. 적절히 사용하면 코드의 재사용성과 유지보수성을 크게 향상시킬 수 있습니다.