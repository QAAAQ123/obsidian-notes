## Nginx가 나온 계기
- Nginx는 Apache의 C10K를 해결하기 위해서 개발
- C10K: 구형 소켓 I/O방식 때문에 만개 이상의 소켓을 열었을 때 서버가 다운되는 현상
	- Apache의 I/O방식: 싱글 스레드에서 CPU와 I/O작업을 모두 처리 
- C10K를 Nginx에서 해결 했기 때문에 트래픽이 많은 곳에서 Apache보다 Nginx가 유리하다

## Nginx 사용-Nexflix
- Nexflix는 Nginx를 Proxy서버와 Load balancing 용도로 사용한다

## Reverse Proxy
- 클라이언트와 서버 사이에 위치한 **중간 관문 서버**
- 일반적인 Foward Porxy와 역할이 반대이기 때문에 Reverse이다
	- Foward Proxy: 클라이언트를 위해 존재하며, "클라이언트가 누구인지"를 숨겨준다
	- Reverse Proxy: 서버를 위해 존재하며, "요청을 처리하는 서버가 누구인지,어디에 있는지"를 숨겨준다
#### Reverse Proxy를 사용하는 이유
1. Load Balancing: 요청 분산
2. Security & Anonymity: 클라이언트가 리버스 프록시 서버의 IP주소만 알기 때문에 실제 벡엔드 서버의 정보를 숨김(방화벽 역할)
3. SSL/TLS 암호화 처리(SSL Termination): HTTPS 암호화/복호화 작업을 전담-백엔드 서버(WAS)는 비즈니스 로직에 집중
4. 캐싱: Caching해야 하는 정보를 리버스 프록시가 cache에 저장해둠
