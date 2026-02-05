## Scale-Out
- 머신을 추가하는 것
- Scale-out 하게되면 머신이 2개 이상이 되기 때문에 **분산 처리를 해줘야 한다**. -> 로드 밸런싱의 필요성 대두

### Scale-Up
- 머신 자체의 성능을 증가시키는 것
- 램 용량 증가,CPU 업그레이드 등..

## Load balancing 전략
1. Round robin: 정해진 순서를 반복하는 방법(s1 -> s2 -> s3 -> s1 -> s2....)
2. Random select:랜덤한 서버에 요청을 보내는 방법
3. Least connection: 서버의 상태를 load balancer에 알려주고, load balancer가 동적으로 판단을 하는 방법(App sever와 Load balancer간의 실시간 통신 필수)
4. Ratio: 물리 머신의 성능 자체를 기반으로 Ratio를 미리 정해서 요청을 보내는 방법

## Load balancing 구축 전략
1. 소프트웨어적 방식: 로직을 구현해서 요청을 분산시키는 방식
	- HA Proxy,Reverse Proxy(Nginx,Apache 탑재 기능)
	- 저렴한 가격,Scale-out할때 설정만 바꾸면 쉽게 변경 가능
2. 하드웨어적 방식: 물리 서버를 묶는것
	- L4,L7 Switch(어떤 메타데이터를 어느 정도 범위까지 고려를 해서 Load balancing을 할거냐의 차이)
	- 비싼 가격,안정성이 높다,보안 측면에서 좋다(데이터 센터에 아무나 출입할 수 없기 때문에)

- **~={red} Single Point of Failure(SPOF): 한 가지 포인트에서 실패하면 전체 시스템이 다운되는 경우=~**
## Load balancer가 다운
- Load balancer도 SPOF이기 때문에 Load balancer 자체를 Sacle-out할 필요가 있다
	- 방식: Master-Slave 등..