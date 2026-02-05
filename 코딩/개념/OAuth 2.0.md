## OAuth 사용 이전
1. 유저 --구글 ID,PW --> 서버 -- 구글 로그인 --> 구글
2. 구글 -- 사용자 정보 --> 서버 -- 서비스 제공 --> 사용자
- 사용자:유저가 서비스 제공자를 믿을 수 없음
	- 구글 로그인 정보를 받아서 구글의 다양한 서비스에 접근 가능하기 때문
- 서버: 유저의 ID,PW를 관리하기 부담스러움
- 구글: 보안을 높게 해도 개인 서비스에 ID와 PW를 노출하면 소용 없음
- **~={red}OAuth 흐름:유저가 인증을 수행하고 권한을 서버가 가진다=~**


## OAuth 사용 이후
1. 유저 --- 구글 ID,PW ---> 구글: 서버가 인증 과정에 전혀 포함되지 않는다
2. 서버 < --- 접근 권한 ---> 구글: 구글이 서버에 접근 권한 부여/유저에게 권한을 전혀 주지 않는다(과정 최소화하여 보안 향상)

## OAuth 
~={red}**인증은 유저가 직접하고 권한은 서비스에게 준다**=~
인가를 위해서 탄생한 기술

## OAuth flow
1. **Authorization Code**
2. Implicit
3. Resource Owner Password Credentials
4. Client Credentials

## OAuth Role
1. 유저: Resource Owner-인증 수행 주체
2. 서비스: Client/Thrid-party Application-권한 위임 받음
3. OAuth 제공자
	1. Authorization Server-인증을 검토하고 권한 부여
	2. Resource Server-인가를 수행하고 리소스를 제공

## OAuth 서비스 등록
- 개발한 서비스를 OAuth 제공자 측에 등록해야함
- 클라이언트 ID: 서비스 식별자
- 클라이언트 보안 비밀번호: 실제 서비스인지 판별
- 승인된 리디렉션 URI: 서비스가 권한을 받는 URI(리다이렉션 필수)
- **2.0에서는 권한을 분류한다**

## OAuth 인증/권한 부여 과정
User-Agent: resource onwer가 인증을 수행하기 위한 매개체(브라우저)
**결과물**: Access Token (자원 접근용).
![[Pasted image 20251126183245.png]]
- 인증할 때 클라이언트는 로그인 페이지의 URI만 제공해준다
- 권한 부여할 때는 클라이언트와 Authorization Server만 참여한다
- HTTPS권장

## OAuth 리소스 요청 과정
**결과물**: Access Token (자원 접근용)
![[Pasted image 20251126183518.png]]

## OAuth 2.0에 인증을 위해 추가된 Open ID Connect
**결과물**: Access Token + **ID Token** (사용자 정보 포함)
![[Pasted image 20251126183730.png]]