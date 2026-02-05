# Spring Boot Mockito 실전 가이드

## 1. Mockito란 무엇인가?

Mockito는 Java에서 가장 널리 사용되는 모킹(Mocking) 프레임워크입니다. 테스트 시 실제 객체 대신 가짜 객체(Mock)를 만들어 의존성을 제거하고 단위 테스트를 더 효과적으로 작성할 수 있게 해줍니다.

**왜 모킹이 필요한가요?** 실제 서비스 개발에서는 여러 계층이 복잡하게 얽혀있습니다. 예를 들어, Controller → Service → Repository → Database의 흐름에서 Controller만 테스트하고 싶다면, 실제 데이터베이스 연결 없이도 테스트할 수 있어야 합니다.

## 2. Spring Boot에서 Mockito 사용하기

### 2.1 기본적인 어노테이션들

#### `@ExtendWith(MockitoExtension.class)`

```java
@ExtendWith(MockitoExtension.class)
public class PostServiceTest {
    // JUnit 5에서 Mockito를 활성화하는 어노테이션
    // 이 어노테이션이 있어야 @Mock, @InjectMocks 등이 동작함
}
```

#### `@Mock`

```java
@Mock
private PostRepository postRepository;
// 실제 PostRepository 대신 가짜 객체를 생성
// 이 객체의 메서드는 기본적으로 null이나 기본값을 반환
```

#### `@InjectMocks`

```java
@InjectMocks
private PostService postService;
// PostService 객체를 생성하면서 @Mock으로 만든 객체들을 자동으로 주입
// 실제로는 PostService postService = new PostService(postRepository); 와 비슷한 효과
```

### 2.2 Spring Boot 전용 어노테이션들

#### `@WebMvcTest`

```java
@WebMvcTest(PostController.class)
public class PostControllerTest {
    // Controller 계층만 테스트하기 위한 어노테이션
    // Spring Security, MockMvc 등 웹 계층 테스트에 필요한 것들만 로드
    // 전체 ApplicationContext를 로드하지 않아 빠름
}
```

#### `@MockBean` vs `@MockitoBean`

```java
// Spring Boot 2.x까지
@MockBean
private PostService postService;

// Spring Boot 3.2+부터 권장
@MockitoBean  
private PostService postService;
// Spring ApplicationContext에 Mock 객체를 등록
// @Mock과 달리 Spring 컨테이너가 관리하는 Bean을 모킹
```

## 3. 실전 테스트 작성 패턴

### 3.1 Service 계층 테스트 (단위 테스트)

```java
@ExtendWith(MockitoExtension.class)
public class PostServiceTest {
    
    @Mock
    private PostRepository postRepository; // 의존성을 모킹
    
    @InjectMocks
    private PostService postService; // 테스트 대상 객체
    
    @Test
    @DisplayName("게시글 생성 시 성공적으로 저장된다")
    void createPostSuccessfully() {
        // Given (준비) - 테스트 데이터 준비
        PostDTO inputDTO = new PostDTO(null, "제목", "내용", null);
        Post savedPost = new Post(1L, "제목", "내용", LocalDateTime.now());
        
        // When 절에서 repository.save()가 호출되면 savedPost를 반환하도록 설정
        when(postRepository.save(any(Post.class))).thenReturn(savedPost);
        
        // When (실행) - 실제 테스트할 메서드 호출
        PostDTO result = postService.createPost(inputDTO);
        
        // Then (검증) - 결과 확인
        assertThat(result.getPostId()).isEqualTo(1L);
        assertThat(result.getPostTitle()).isEqualTo("제목");
        
        // 메서드가 실제로 호출되었는지 검증
        verify(postRepository, times(1)).save(any(Post.class));
    }
}
```

### 3.2 Controller 계층 테스트 (통합 테스트)

```java
@WebMvcTest(PostController.class)
public class PostControllerTest {
    
    @Autowired
    private MockMvc mockMvc; // HTTP 요청을 모킹하는 객체
    
    @MockitoBean
    private PostService postService; // Service 계층을 모킹
    
    @Test
    @DisplayName("POST /posts 요청 시 게시글이 성공적으로 생성된다")
    void createPostEndpointSuccess() throws Exception {
        // Given
        PostDTO inputDTO = new PostDTO(null, "제목", "내용", null);
        PostDTO savedDTO = new PostDTO(1L, "제목", "내용", LocalDateTime.now());
        
        when(postService.createPost(any(PostDTO.class))).thenReturn(savedDTO);
        
        // When & Then
        mockMvc.perform(post("/posts")
                .contentType(MediaType.APPLICATION_JSON)
                .content("{\"postTitle\":\"제목\",\"postContent\":\"내용\"}")
                .with(user("testuser").roles("USER"))) // Spring Security 모킹
                .andDo(print()) // 요청/응답 내용 콘솔에 출력
                .andExpect(status().isCreated()) // HTTP 201 상태 코드 확인
                .andExpect(jsonPath("$.postId").value(1L)) // JSON 응답 내용 검증
                .andExpect(jsonPath("$.postTitle").value("제목"));
    }
}
```

## 4. 고급 Mockito 사용법

### 4.1 다양한 Stubbing 방법

```java
// 단순 반환값 설정
when(postRepository.findById(1L)).thenReturn(Optional.of(post));

// 예외 발생 설정
when(postRepository.findById(999L)).thenThrow(new PostNotFoundException());

// 메서드 여러 번 호출 시 다른 값 반환
when(postRepository.count())
    .thenReturn(5L)      // 첫 번째 호출
    .thenReturn(6L)      // 두 번째 호출
    .thenThrow(new RuntimeException()); // 세 번째 호출

// 조건부 반환 (Answer 사용)
when(postRepository.save(any(Post.class))).thenAnswer(invocation -> {
    Post post = invocation.getArgument(0);
    post.setId(1L); // ID 설정
    return post;
});
```

### 4.2 ArgumentCaptor 사용

```java
@Test
void testWithArgumentCaptor() {
    // ArgumentCaptor로 실제 전달된 인수 캡처
    ArgumentCaptor<Post> postCaptor = ArgumentCaptor.forClass(Post.class);
    
    postService.createPost(inputDTO);
    
    // save 메서드에 전달된 Post 객체 캡처
    verify(postRepository).save(postCaptor.capture());
    
    // 캡처된 객체 검증
    Post capturedPost = postCaptor.getValue();
    assertThat(capturedPost.getTitle()).isEqualTo("제목");
}
```

### 4.3 Verification (검증) 방법들

```java
// 메서드가 정확히 1번 호출되었는지 확인
verify(postRepository, times(1)).save(any(Post.class));

// 메서드가 호출되지 않았는지 확인
verify(postRepository, never()).delete(any(Post.class));

// 메서드가 최소 2번 호출되었는지 확인
verify(postRepository, atLeast(2)).findAll();

// 메서드가 최대 3번 호출되었는지 확인
verify(postRepository, atMost(3)).findById(anyLong());

// 특정 순서로 호출되었는지 확인
InOrder inOrder = inOrder(postRepository, userRepository);
inOrder.verify(postRepository).save(any(Post.class));
inOrder.verify(userRepository).findById(anyLong());
```

## 5. 실제 프로젝트에서 자주 사용하는 패턴

### 5.1 데이터베이스 관련 테스트

```java
@Test
@DisplayName("존재하지 않는 게시글 조회 시 예외가 발생한다")
void findPostNotFound() {
    // Given
    when(postRepository.findById(999L)).thenReturn(Optional.empty());
    
    // When & Then
    assertThrows(PostNotFoundException.class, () -> {
        postService.findById(999L);
    });
}

@Test
@DisplayName("게시글 목록 조회 시 빈 목록이 반환된다")
void findAllPostsEmpty() {
    // Given
    when(postRepository.findAll()).thenReturn(Collections.emptyList());
    
    // When
    List<PostDTO> result = postService.findAllPosts();
    
    // Then
    assertThat(result).isEmpty();
    verify(postRepository).findAll();
}
```

### 5.2 외부 API 호출 테스트

```java
@ExtendWith(MockitoExtension.class)
public class ExternalServiceTest {
    
    @Mock
    private RestTemplate restTemplate;
    
    @InjectMocks
    private ExternalApiService externalApiService;
    
    @Test
    void callExternalApiSuccessfully() {
        // Given
        String expectedResponse = "{\"status\":\"success\"}";
        when(restTemplate.getForObject(anyString(), eq(String.class)))
            .thenReturn(expectedResponse);
        
        // When
        String result = externalApiService.fetchData();
        
        // Then
        assertThat(result).contains("success");
        verify(restTemplate).getForObject(contains("api"), eq(String.class));
    }
}
```

## 6. 테스트 작성 시 주의사항과 베스트 프랙티스

### 6.1 Given-When-Then 패턴 준수

모든 테스트는 명확한 구조를 가져야 합니다:

- **Given**: 테스트 데이터와 모킹 설정
- **When**: 실제 테스트할 메서드 호출
- **Then**: 결과 검증과 동작 확인

### 6.2 테스트 메서드 이름 규칙

```java
// 나쁜 예
@Test
void test1() { }

// 좋은 예
@Test
@DisplayName("유효하지 않은 이메일로 회원가입 시 예외가 발생한다")
void registerUserWithInvalidEmailThrowsException() { }
```

### 6.3 Mock 객체는 최소한으로 사용

너무 많은 의존성을 모킹하면 테스트가 복잡해지고 실제 동작과 달라질 수 있습니다. 꼭 필요한 의존성만 모킹하세요.

### 6.4 @DirtyContext 주의

```java
// Spring Context를 재사용하지 않고 새로 생성
@DirtyContext(classMode = DirtyContext.ClassMode.AFTER_EACH_TEST_METHOD)
// 성능상 이유로 꼭 필요한 경우에만 사용
```

## 7. 실전 예제: 복잡한 비즈니스 로직 테스트

```java
@ExtendWith(MockitoExtension.class)
public class PostServiceComplexTest {
    
    @Mock private PostRepository postRepository;
    @Mock private UserRepository userRepository;
    @Mock private NotificationService notificationService;
    
    @InjectMocks
    private PostService postService;
    
    @Test
    @DisplayName("게시글 작성 시 작성자에게 알림이 전송된다")
    void createPostSendsNotificationToAuthor() {
        // Given
        Long authorId = 1L;
        User author = new User(authorId, "작성자", "author@test.com");
        PostDTO inputDTO = new PostDTO(null, "제목", "내용", null);
        Post savedPost = new Post(1L, "제목", "내용", LocalDateTime.now());
        
        when(userRepository.findById(authorId)).thenReturn(Optional.of(author));
        when(postRepository.save(any(Post.class))).thenReturn(savedPost);
        doNothing().when(notificationService).sendNotification(any(), any());
        
        // When
        PostDTO result = postService.createPostWithNotification(inputDTO, authorId);
        
        // Then
        assertThat(result.getPostId()).isEqualTo(1L);
        
        // 모든 의존성이 올바르게 호출되었는지 확인
        verify(userRepository).findById(authorId);
        verify(postRepository).save(any(Post.class));
        verify(notificationService).sendNotification(
            eq(author.getEmail()), 
            contains("게시글이 작성되었습니다")
        );
    }
}
```

이처럼 Mockito를 사용하면 복잡한 의존성을 가진 클래스도 효과적으로 단위 테스트할 수 있습니다. 각 계층을 독립적으로 테스트함으로써 버그를 빠르게 찾고 코드의 품질을 높일 수 있습니다.