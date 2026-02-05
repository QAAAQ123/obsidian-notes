## Redis
- Remote dictionary server
- Database,Catch,Message broker
- In-memory Data structure store-메모리에 데이터 저장
- Supports rich data structure-다양한 자료구조 지원
### Cache
- 요청에 대한 결과를 미리 저장했다가 빠르게 사용하는 것

### Redis 자료 구조
- Collection 자료구조 지원
- String(Map.Entry),List(LinkedList),Set(HashSet),Sorted Set(TreeSet),Hash(HashMap)

#### Java의 Collection 사용 안하는 이유
1. 서버가 여러대인 경우 일관성 문제
2. 멀티 스레드 환경에서 Race Condition 문제
	1. Race Condition: 여러 개의 스레드가 경합하면서 Context Switching이 발생하면 원하지 않는 결과가 나올 가능성 있는것

### Race Condition 해결
1. Redis 싱글 스레드 사용
2. 원자성을 가지고 있어서 동시에 접근하면 안되는 영역에서 동기화를 해준다
3. 여러 트랜잭션의 read/write를 동기화 

#### 사용 
1. 여러 서버에서 같은 데이터 공유할때
2. 원자성 자료구조
3. 캐시 사용

### 주의점
1. 싱글 스레드 서버이기 때문에 시간 복잡도를 고려해야 한다
2. In-memory 특성상 OS와 메모리에 대한 지식이 필요하다
3. O(N)인 명령어는 주의하여 사용해야 한다