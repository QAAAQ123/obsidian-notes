## 흐름
1. RequestInterceptor를 빈으로 등록한다.
	1. @Bean의 RequestInterceptor에 return 값으로 URL뒤에 들어갈 query를 작성한다.
2. @FeignClinet로 어떤 url과 configuration을 가져올지 결정한다
3. @FeignClinet를 작성한 interface에 API에 보낼 요청을 작성한다
4. 3의 interface의 의존성을 주입받아서 동작하는 service를 만든다
5. controller에서 service를 의존성 주입해서 사용한다.

## 동작 과정
1. 컨트롤러에서 요청이 들어오면 @FeignClient에서 url을 확인하고 정해놓은 Configuration으로 이동한다.
2. **인터페이스 함수 호출**이 **트리거**가 되어 요청이 만들어지고, **인터셉터**는 그 요청이 발송되기 직전에 **끼어들어 수정**한 url뒤에 쿼리가 작성된 URI를 interface의 함수에서 받아서 해당 함수에 맞는 HTTP Method 타입으로 API 요청을 보낸다
3. interface의 함수에서 받은 API응답 정보를 service나 controller에서 사용한다.

---

### ## 흐름 (아키텍처 구성 순서)

애플리케이션의 구조를 만들고 코드를 작성하는 순서입니다.

1. **설정 클래스 작성**: `RequestInterceptor` 빈을 정의하는 Feign 전용 설정 클래스(`GNAConfig` 등)를 API별로 각각 만듭니다. `@Configuration` 어노테이션은 붙이지 않습니다.
    
2. **Feign 인터페이스 정의**: `@FeignClient` 어노테이션을 사용하여 통신할 API의 기본 정보(url, name)와 위에서 만든 설정 클래스(`configuration`)를 지정합니다.
    
3. **API 요청 메서드 선언**: Feign 인터페이스 내부에 `@GetMapping` 등을 사용하여 실제 호출할 API의 세부 경로와 파라미터를 정의하는 메서드를 선언합니다.
    
4. **서비스 계층 구현**: 비즈니스 로직을 처리할 `@Service` 클래스를 만들고, 그 안에서 Feign 인터페이스를 의존성 주입(DI)받습니다.
    
5. **컨트롤러 계층 구현**: `@RestController`가 서비스 계층을 의존성 주입받아, 최종적으로 외부 요청에 따라 서비스의 메서드를 호출하고 결과를 반환하도록 합니다.
    

---

### ## 동작 과정 (API 호출 시 실제 실행 순서)

사용자가 API를 요청했을 때 내부에서 코드가 실행되는 순서입니다.

1. 요청 접수 (Controller → Service)
    
    사용자 요청이 들어오면, Controller는 요청에 맞는 Service의 메서드를 호출합니다.
    
2. API 호출 시작 (Service → Feign Interface)
    
    Service는 주입받은 Feign 인터페이스의 메서드를 호출합니다. 이 호출이 실제 API 통신의 '방아쇠(Trigger)' 역할을 합니다.
    
3. **HTTP 요청 생성 및 수정 (Feign Proxy + RequestInterceptor)**
    
    - **(생성)** Spring이 내부적으로 만든 Feign의 프록시(Proxy) 객체가 메서드 호출을 받아, `@FeignClient`와 `@GetMapping`의 정보를 조합하여 HTTP 요청을 생성하기 시작합니다.
        
    - **(수정)** 실제 서버로 요청을 보내기 **직전**, `configuration`에 지정된 `RequestInterceptor`가 이 요청을 가로채서 `accessToken` 같은 동적 파라미터를 추가하며 최종적으로 요청을 완성합니다.
        
4. API 통신 및 응답
    
    완성된 HTTP 요청을 API 서버로 전송하고 응답을 받습니다. Feign은 이 응답을 인터페이스 메서드에 정의된 반환 타입(예: String, DTO)으로 변환하여 Service에 돌려줍니다.
    
5. 최종 결과 반환
    
    Service는 응답받은 데이터를 필요에 따라 가공한 후 Controller에 전달하고, Controller는 이를 사용자에게 최종 응답합니다.