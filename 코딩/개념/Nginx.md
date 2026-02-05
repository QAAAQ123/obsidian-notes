## Apache
### 구조
- 요청이 들어올때마다 새로운 프로세스를 생성한다
	- Unix 네트워크 모델 그대로 채택
- 프로세스를 생성하는데 시간이 오래 걸리기 때문에 미리 프로세스를 만들어 놓는 PreFork방식을 사용했다
### 장점
- 다양한 모듈을 서버 빠르게 기능 추가 -> 동적 컨텐츠 생성 가능
- 확장성 좋음
- Request와 Response를 하나의 서버에서 처리

### 한계
- 인터넷 수요가 급증하자 connection을 형성하지 못하는 문제가 생김(C10K)
- 동시에 연결된 커넥션 수와 초당 요청 처리 수는 다르다
- 요청이 없으면 connection 방치

### C10K(Connection 10000 problem)
- 요청을 보낼때 이미 만들어진 커넥션을 재활용하는 Keep-alive를 사용 했지만 커넥션이 만개를 넘어가면 문제 발생

### Apache의 한계-수많은 동시 커넥션에 부적합
- connection마다 프로세스가 있어야하기 때문에 메모리 부족
- Apache 자체가 무거운 프로그램
- 많은 기능을 지원하기 때문에 프로세스가 차지하는 리소스가 많아 CPU 부하

## Nginx
- Apache web server 보완하기 위한 새로운 구조
### 초기 Nginx
- Apache와 같이 사용하던 Apache 보조 도구
- Apache 앞단에 Nginx를 둬서 동시 커넥션에서 발생하는 부하를 Nginx가 받음
- Nginx 자체도 웹서버이기 때문에 정적 파일은 바로 클라이언트로 반환하고 동적 파일만 Appache로 보낸다

### Nginx 구조
- master process: config파일에 맞게 worker process를 생성하는 프로세스
- worker process: listen socket을 배정 받아서 요청을 처리한다
	- 추가 요청이 없으면 connection을 새로 생성하거나 이미 만들어진 connection으로 요청을 처리한다
	- 쉬지 않고 일을 한다

### Nginx 동작
- connection 형성,connection 제거,요청 처리 과정을 event라고 부른다
- OS 커널이 event를 queue구조로 worker process에 전달해준다
- 단일 스레드, 비동기 방식
- Thread Pool
	- I/O와 같이 시간이 많이 걸리는 event를 처리하기 위한 별도 공간
	- worker process가 thread pool에 오래 걸리는 작업을 위임

### Nginx 특징
- CPU 코어개수 만큼 worker process를 생성해서 context switching을 최소화
- 개발자가 기능 추가를 시도했다가 동작하고 있는 worker process를 종료할 수도 있음 -> Nginx는 개발자가 모듈을 만들기 까다롭다
- 프로세스를 적게 만들기 때문에 설정을 동적으로 변경할 수 있다

### Nginx 장점-~={red}동시 커넥션 문제 해결=~
- 동시 커넥션 양 최소 10배 증가
- 동일한 커넥션 수일 때 속도 2배 향상
- 수많은 동시 커넥션을 빠르게 처리하는데 가볍다

### Nginx의 동적 설정
- 프로세스가 적기 때문에 동적으로 프로세스 생성이 가능해진다
- 설정 변경을 초당 수십번 해도 처리 가능
- Nginx의 설정을 변경
	1. master process는 현재 설정에 맞는 worker process를 추가로 생성한다
	2. 기존 worker process에 connection을 추가하지 않는다
	3. 기존 worker process의 event 처리가 모두 끝나면 기존 worker process를 제거한다
	4. 바뀐 설정에 맞는 worker process만 남는다
- 동적 설정 예시
	- 서버가 추가되는 경우:Nginx가 바뀐 서버에 맞게 process를 변경해준다
- 2008년 아이폰과 웹 수요 증가로 Apache의 점유율이 줄어들고 Nginx의 점유율이 상승했다

### Apache의 진화
- Apache도 MPM을 지원하는 방식으로 진화했다
	- MPM: 원하는 요청 처리방식 선택
		- 기존의 PreFork 방식으로 안정성 향상
		- worker thread로 성능 향상

### Apache VS Nginx
- Apache: 호환성,**확장성,안정성**,모둘 종류 많음
- Nginx: web server의 **동시 커넥션** 문제 보완


## Nginx 역할
1. Web server
2. load balancer
3. SSL termination: 비즈니스에 집중 할 수 있도록 암호와/복호화 과정의 부하 줄여줌
	1. Client와 Nginx는 HTTPS 통신
	2. Nginx와 Sever는 HTTP 통신
4. Caching: HTTP 프로토콜을 이용해서 캐싱
5. HSTS
6. CORS 처리
7. TCP/UDP 커넥션 부하 분산
8. HTTP/2 지원