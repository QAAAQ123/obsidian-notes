1. thenReturn(): 메소드 호출 시 특정 값을 반환하도록 설정
`when(mock객체.메서드호출(인자)).thenReturn(반환값1,반환값2,...)`

2. thenThrow(): 메소드 호출 시 예외를 던지도록 설정
`when(mock객체.메서드호출(인자)).thenThrow(new 예외("예외메시지"));`

3. thenAnswer(): 복잡한 로직이나 동적인 응답이 필요할 때 사용
```java
// `process(String input)` 호출 시, 입력된 문자열에 "processed-"를 붙여 반환
when(mockProcessor.process(anyString())).thenAnswer(invocation -> {
    String arg = invocation.getArgument(0); // 첫 번째 인자 가져오기
    return "processed-" + arg;
});
```
4. thenCallRealmethod(): 실제 메소드 호출하도록 설정
`when(mockCalculator.calculateSum(anyInt(), anyInt())).thenCallRealMethod();`

#### void 메서드
**==void 메서드에는 when().thenReturn()을 사용할 수 없다.==**
1. doReturn(): thenReturn()과 유사하지만 void메소드나 spy객체에 사용
`doReturn(반환값).when(mock객체 또는 스파이객체).메서드호출(인자);`-나중에 필요할 때 검색

2. doThrow(): Mock 객체의 `void` 메서드가 호출될 때 지정된 예외를 발생
`doThrow(new IllegalStateException("저장할 수 없는 상태")).when(mockService).save(any(Data.class));`

3. doNothing(): `doNothing().when().method()`: 아무것도 하지 않음
`doNothing().when(mockRepository).delete(any(Entity.class));`

4. doAnswer(): `void` 메서드가 호출될 때 특정 로직을 수행하도록 정의
`doAnswer(invocation -> { List<String> list = invocation.getArgument(0); list.add("processed"); // 인자 변경 return null; // void 메서드이므로 null 반환 }).when(mockProcessor).process(anyList());`