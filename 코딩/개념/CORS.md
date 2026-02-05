### 🔍 팩트 체크 및 보완 (이것만 주의하세요!)

1. **GET/POST는 무조건 Simple Request인가?** (❌ **아닙니다**)
    - 작성하신 내용에 *"GET, POST에서만 사용(Simple)"*이라고 되어 있는데, **Content-Type이 `application/json`이면 POST라도 Preflight(예비 요청)를 날립니다.**
    - 요즘 API는 대부분 JSON을 쓰기 때문에, **사실상 우리가 만드는 거의 모든 API는 Preflight 과정을 거친다**고 보시면 됩니다.
2. **Simple Request의 차단 시점**
    - **Simple Request**는 요청 자체는 서버로 **전송됩니다.** 서버도 로직을 수행합니다. 하지만 브라우저가 응답을 받았을 때 허용된 출처가 아니면 **데이터를 사용자에게 보여주지 않고 버립니다.** (즉, 서버 쪽에는 이미 요청이 도달했다는 것이 무서운 점입니다.)


#### 1. SOP (Same-Origin Policy) - "기본 원칙: 철벽 방어"
브라우저의 **보안기능(경비원)**입니다.
- **정의:** "같은 출처(Origin)가 아니면 묻지도 따지지도 않고 차단한다."
- **출처(Origin)란?:** `프로토콜` + `도메인` + `포트` 3박자가 다 맞아야 함.
    - `http://localhost:3000` (프론트) vs `http://localhost:8080` (백엔드)
    - 포트가 다르므로 **다른 출처**로 간주 -> **SOP 위반으로 차단됨.**

#### 2. CORS (Cross-Origin Resource Sharing) - "예외 허용: 출입증"
SOP 때문에 개발을 못 하니 만든 **공식적인 구멍(면죄부)**입니다.
- **정의:** "서버가 허락해준 사이트는 SOP 검사를 통과시켜주자."
- **핵심:** **허락의 주체는 '서버(백엔드)'**입니다. 백엔드 개발자가 설정해줘야 합니다.

#### 3. 동작 방식 (시나리오별)

가장 많이 겪게 될 두 가지 상황입니다.

**A. 단순 요청 (Simple Request)**
- **조건:** `GET`, `HEAD`, `POST` 중 하나이면서 + 헤더가 아주 기본적일 때 (`application/json`은 여기 포함 안 됨!)
- **흐름:**
    1. [브라우저] 요청을 그냥 보냄 (`Origin: http://localhost:3000` 부착)
    2. [서버] 응답함 (`Access-Control-Allow-Origin: http://localhost:3000` 부착)
    3. [브라우저] 헤더 비교 -> 일치하면 통과, 불일치하면 **에러 띄움(CORS Error)**
**B. 예비 요청 (Preflight Request) ⭐ 99%의 상황**
- **조건:** `application/json` 데이터를 보내거나, 커스텀 헤더(예: `Authorization: Bearer Token`)를 실었을 때.
- **흐름:**
    1. [브라우저] 본 요청 전에 **`OPTIONS`** 메서드로 먼저 찔러봄. ("나 이거 보내도 돼?")
    2. [서버] "ㅇㅇ 보내도 됨" (`Access-Control-Allow-Origin`, `Allow-Methods` 등으로 응답)
    3. [브라우저] 허락 확인 후, 그제야 **진짜 본 요청(POST/PUT 등)**을 보냄.

#### 4. 인증 정보 포함 시 (Credentials) - "VIP 심사"

로그인 기능(JWT, 세션) 구현 시 가장 많이 틀리는 부분입니다.
- **상황:** 클라이언트가 쿠키나 인증 헤더를 포함해서 요청할 때 (`withCredentials = true`)
- **조건이 더 까다로워짐:**
    1. 백엔드 응답에 `Access-Control-Allow-Credentials: true`가 있어야 함.
    2. 백엔드 허용 출처(`Allow-Origin`)에 **`*` (모두 허용)을 쓰면 안 됨.** 반드시 `http://localhost:3000`처럼 콕 집어서 명시해야 함.


### 💡 캡스톤 프로젝트 적용 가이드

Spring Security 설정 시 아래 코드를 넣게 될 텐데, 이 개념을 떠올리시면 됩니다.

Java

```
configuration.setAllowedOrigins(Arrays.asList("http://localhost:3000")); // 프론트 주소 명시
configuration.setAllowedMethods(Arrays.asList("GET", "POST", "PUT", "DELETE", "OPTIONS")); // OPTIONS 필수!
configuration.setAllowCredentials(true); // 로그인(JWT) 하니까 필수
configuration.setAllowedHeaders(Arrays.asList("Authorization", "Content-Type")); // JSON이랑 토큰 받아야 하니까
```
