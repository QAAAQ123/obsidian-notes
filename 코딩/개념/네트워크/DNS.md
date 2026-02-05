- Domain Name System
- host name + domain
- 다층적으로 구성된 네트워크에 분산되어서 저장되어있다
![[Pasted image 20251115203605.png]]
- 순서
	1. 로컬 DNS 서버에 해당 도메인과 호스트명의 IP를 가지고 있는지 물어본다(통신사마다 지정된 곳이 있다)
	2. 가지고 있다면 바로 반환 없다면 Root DNS 서버에 이 주소의 IP를 어디서 찾을 수 있는지 물어본다
	3. Root DNS서버는 .xxx로 끝나는 도메인을 담당하는 서버의 IP주소 반환한다
	4. 로컬 서버는 .xxx담당 서버로 찾아간다
	5. .xxx담당 서버는 abcde.xxx의 도메인 정보를 가진 서버의 IP주소 반환한다
	6. 로컬 서버는 abcde.xxx로 찾아가면 여러 Host name별 IP주소가 있다.
	   `www.abcde.xxx`,`mail.abcde.xxx`,`map.abcde.xxx`등 
	7. 여기 에서 `www.abcde.xxx`에 해당하는 IP주소를 반환한다
	8. 로컬 서버에서 브라우저에게 반환한다
	9. 브라우저가 `www.abcde.xxx`에 접속한다
- DNS 스푸핑: DNS로 가는 정보를 가로채서 악성 IP주소를 알려주는 행위
### A Record와 CName
- A Recode: 도메인 서버의 IP로 직접 연결-IP로의 직접 연결
- CName(Canonical name): IP가 유동적으로 변하는 서버에 바뀌는 IP에 고정된 도메인을 도메인 서버와 연결-IP/CName/도메인 서버로 3단계에 거쳐서 연결됨