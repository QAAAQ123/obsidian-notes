
`given().willReturn()`과 `when().thenReturn()`은 Mockito에서 **모의(mock) 객체의 동작을 정의(stubbing)하는 데 사용되는 두 가지 방식**입니다. 기능적으로는 거의 동일하지만, 사용 목적과 코드의 가독성 측면에서 차이가 있습니다.

### 1. `given().willReturn()` vs `when().thenReturn()`: 차이점

기능적 차이:

없습니다. 두 메서드는 내부적으로 동일한 Mockito의 stubbing 메커니즘을 사용합니다. 즉, 특정 메서드가 특정 인자로 호출될 때 미리 정해둔 값을 반환하도록 설정하는 역할은 완전히 같습니다.

**문법적/스타일적 차이:**

- **`when().thenReturn()`**: Mockito의 **기존(traditional) API 스타일**입니다. "이 메서드가 호출될 때, 이 값을 반환해라"는 직접적인 명령형 문법입니다.
    
    Java
    
    ```
    when(mockObject.someMethod(arg)).thenReturn(returnValue);
    ```
    
- **`given().willReturn()`**: Mockito의 **BDD(Behavior-Driven Development) 스타일 API**입니다. "주어진 상황(given)에서, 이 메서드가 호출되면(when), 이 값을 반환할 것이다(will)"라는 서술형 문법입니다. BDD는 테스트 코드를 비즈니스 요구사항처럼 읽히도록 만드는 데 중점을 둡니다.
    
    Java
    
    ```
    given(mockObject.someMethod(arg)).willReturn(returnValue);
    ```
    

### 2. 왜 두 가지 방식이 존재해야 하는가?

두 가지 방식이 존재하는 주된 이유는 **다른 테스트 스타일(Testing Style)을 지원하기 위함**입니다.

- **`when().thenReturn()`**: 일반적인 **단위 테스트(Unit Test)** 상황에서 많이 사용됩니다. 메서드의 동작을 명시적으로 제어하고자 할 때 직관적입니다.
    
- **`given().willReturn()`**: **행위 주도 개발(BDD)** 방법론을 따를 때 선호됩니다. BDD는 테스트를 "사용자 스토리" 또는 "명세(Specification)"처럼 작성하여, 코드의 동작뿐만 아니라 "왜 이렇게 동작해야 하는지"에 대한 비즈니스적인 맥락을 강조합니다. BDD 테스트는 보통 `Given-When-Then` 구조를 따르는데, `given().willReturn()`은 이 `Given` 단계와 자연스럽게 어울립니다.
    

### 3. 언제 어떤 것을 사용해야 하는가?

- **`when().thenReturn()` 사용 시점:**
    
    - **전통적인 단위 테스트:** 대부분의 Java, Spring Boot 프로젝트에서 보편적으로 사용되며, 직관적이고 널리 알려진 방식입니다.
        
    - **간결한 테스트:** 테스트의 목적이 단순히 메서드의 반환값을 제어하는 것일 때 효과적입니다.
        
- **`given().willReturn()` 사용 시점:**
    
    - **BDD 스타일 테스트:** 프로젝트나 팀이 BDD 방법론을 채택하고 `Given-When-Then` 구조를 테스트에 적용하고자 할 때 사용합니다. 테스트 코드가 마치 비즈니스 요구사항을 설명하는 문장처럼 읽히도록 돕습니다.
        
    - **가독성 향상:** 특히 복잡한 시나리오에서 `given` 절이 길어질 때, `given().willReturn()`은 테스트의 "준비" 단계를 더 명확하게 표현해줍니다.
        

**결론적으로, 기능적인 차이는 없으므로 둘 중 어느 것을 사용해도 무방합니다.** 하지만 **프로젝트나 팀의 컨벤션(Convention)을 따르는 것이 가장 중요합니다.** 일관된 스타일을 유지하면 코드의 가독성과 유지보수성이 향상됩니다. 만약 BDD를 지향한다면 `given().willReturn()`을, 그렇지 않다면 `when().thenReturn()`을 사용하는 것이 일반적입니다.