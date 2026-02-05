## 어노테이션

1. @ExtendWith(MockitoExtension.class): JUnit 5에서 Mockito 기능을 활성화하는 어노테이션으로, @Mock, @InjectMocks 등의 어노테이션이 동작하도록 함
2. @WebMvcTest(xxxController.class): 웹 계층(Controller)만 테스트하기 위한 어노테이션으로, 전체 ApplicationContext 대신 웹 관련 Bean들만 로드하여 빠른 테스트 가능
3. @Mock: 실제 객체 대신 가짜 Mock 객체를 생성하는 어노테이션으로, 의존성을 제거하고 단위 테스트를 위한 독립적인 환경 구성
4. @InjectMocks: @Mock으로 생성된 객체들을 자동으로 주입받아 테스트 대상 객체를 생성하는 어노테이션
5. @DisplayName: 테스트 메서드의 이름을 한글이나 더 구체적인 설명으로 표시할 수 있게 해주는 어노테이션
6. @Test: JUnit에서 해당 메서드가 테스트 메서드임을 나타내는 기본 어노테이션
7. @MockitoBean: Spring Boot 3.2+에서 권장하는 어노테이션으로, Spring ApplicationContext에 Mock Bean을 등록하여 실제 Bean을 대체

## 메소드

1. when(): Mockito에서 Mock 객체의 특정 메서드 호출 시의 동작을 정의하기 시작하는 메서드
2. when().thenReturn(): Mock 객체의 메서드가 호출되었을 때 특정 값을 반환하도록 설정하는 메서드
3. assertThat(): AssertJ 라이브러리의 검증 메서드로, 더 읽기 쉽고 다양한 검증 기능을 제공
4. mockMvc: Spring에서 제공하는 MockMvc 객체로, 실제 서버를 띄우지 않고 HTTP 요청을 시뮬레이션할 수 있게 해주는 테스트 도구
5. mockMvc.perform(): HTTP 요청을 실행하는 메서드로, GET, POST 등의 요청을 시뮬레이션
6. mockMvc.perform().contentType(): HTTP 요청의 Content-Type 헤더를 설정하는 메서드 (예: APPLICATION_JSON)
7. mockMvc.perform().contentType().with(): Spring Security 테스트를 위한 추가 설정을 하는 메서드 (예: 사용자 인증 정보 설정)
8. mockMvc.perform().andDo(): 요청 실행 후 추가 작업을 수행하는 메서드 (예: print()로 요청/응답 내용 출력)
9. mockMvc.perform().andDo().andExpect(): HTTP 응답에 대한 검증을 수행하는 메서드 (상태 코드, JSON 내용 등 검증)
10. mockMvc.perform().andDo().andExpect().value(): JSON Path나 특정 필드의 값이 예상한 값과 일치하는지 검증하는 메서드