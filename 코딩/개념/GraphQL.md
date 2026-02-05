- GraphQL은 쿼리언어 
- 클라이언트가 서버로부터 데이터를 가져오는 것이 목적
- endpoint 하나로 모든 쿼리를 처리함
- 보통 REST와 GraphQL 모두 사용함
```javascript
{
	user(id: "1000") {
	name
	createDt
	}
}
```