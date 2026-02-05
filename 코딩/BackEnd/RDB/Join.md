## Join 개념
- ==**두 개의 테이블을 엮어서 원하는 데이터를 추출**==
- 조인의 종류
	- Inner Join(내부 조인)
	- Outer Join(외부 조인)
	- Cross Join(상호 조인)
	- Self Join(자체 조인)

## INNER JOIN(내부 조인)
- 가장 많이 사용하는 조인
- 두 테이블 사이의 ==교집합==에서 원하는 데이터 추출
```sql
SELECT <열 목록> 
	FROM <첫 번째 테이블>     
	INNER JOIN <두 번째 테이블>     
	ON <조인 조건> 
[WHERE 검색 조건]  

```
![[inner join.png]]


## OUTER JOIN(외부 조인)
- 교집합 + ==조건에 맞지 않는 데이터도 표시==하고 싶을 때 사용
- 외부 조인의 종류
	1. LEFT OUTER JOIN: 왼쪽 테이블의 모든 값이 출력되는 조인
	2. RIGHT OUTER JOIN: 오른쪽 테이블의 모든 값이 출력되는 조인
	3. FULL OUTER JOIN: 양쪽 테이블의 모든 값이 출력되는 조인
```sql
SELECT <열 목록> 
FROM <첫 번째 테이블(LEFT 테이블)>     
	<LEFT | RIGHT | FULL> OUTER JOIN <두 번째 테이블(RIGHT 테이블)>      ON <조인 조건> 
[WHERE 검색 조건]
```
![[outer join.png]]

## CROSS JOIN(상호 조인)
- 카티션 프로덕트(카티션 곱)
```sql
SELECT *
	FROM <첫 번째 테이블>
		CROSS JOIN <두 번째 테이블>
```

## SELF JOIN(자체 조인)
- 자기 자신과 조인
```sql
SELECT <열 목록>
FROM <테이블> 별칭A
	INNER JOIN <테이블> 별칭B
[WHERE 검색 조건]
```

[출처](https://hongong.hanbit.co.kr/sql-%EA%B8%B0%EB%B3%B8-%EB%AC%B8%EB%B2%95-joininner-outer-cross-self-join/)