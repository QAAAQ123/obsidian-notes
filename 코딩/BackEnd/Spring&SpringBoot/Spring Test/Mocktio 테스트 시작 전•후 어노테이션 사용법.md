# Mockito 테스트 시작 전 어노테이션

## @BeforeEach

각 테스트 메소드 실행 전에 실행

```java
@BeforeEach
void setUp() {
    // Mock 객체 수동 생성
    userRepository = mock(UserRepository.class);
    userService = new UserService(userRepository);
    
    // 공통 설정
    when(userRepository.findById(1L)).thenReturn(defaultUser);
}
```

## @BeforeAll

테스트 클래스 실행 전 한번만 실행 (static 메소드)

```java
@BeforeAll
static void setUpClass() {
    // 데이터베이스 초기화
    // 테스트용 파일 생성 등
    MockedStatic<Utils> mockedUtils = mockStatic(Utils.class);
}
```

## @AfterEach

각 테스트 메소드 실행 후에 실행

```java
@AfterEach
void tearDown() {
    // Mock 상태 초기화
    reset(userRepository);
    
    // 리소스 정리
    clearInvocations(emailService);
}
```

## @AfterAll

테스트 클래스 실행 후 한번만 실행 (static 메소드)

```java
@AfterAll
static void tearDownClass() {
    // 정적 Mock 정리
    mockedUtils.close();
}
```

## @DisplayName

테스트 이름 지정

```java
@DisplayName("사용자 생성 시 이메일 발송 테스트")
@Test
void createUserSendsEmail() {
    // 테스트 코드
}
```

## @Nested

테스트 그룹화

```java
@Nested
@DisplayName("사용자 생성 테스트")
class CreateUserTests {
    
    @BeforeEach
    void setUp() {
        // 이 그룹만의 설정
    }
    
    @Test
    void validUserCreation() {}
    
    @Test
    void invalidUserCreation() {}
}
```

## 실제 사용 예시

```java
@ExtendWith(MockitoExtension.class)
@DisplayName("사용자 서비스 테스트")
class UserServiceTest {
    
    @Mock private UserRepository userRepository;
    @InjectMocks private UserService userService;
    
    private User testUser;
    
    @BeforeAll
    static void setUpClass() {
        System.out.println("테스트 시작");
    }
    
    @BeforeEach
    void setUp() {
        testUser = new User("John", "john@test.com");
        lenient().when(userRepository.existsById(anyLong())).thenReturn(false);
    }
    
    @AfterEach
    void tearDown() {
        verifyNoMoreInteractions(userRepository);
    }
    
    @Test
    @DisplayName("정상적인 사용자 생성")
    void createUser() {
        when(userRepository.save(any())).thenReturn(testUser);
        
        User result = userService.createUser("John", "john@test.com");
        
        assertThat(result.getName()).isEqualTo("John");
    }
}
```