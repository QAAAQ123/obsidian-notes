## Matcher 사용 규칙
- ~={red}메서드에서 Matcher를 하나라도 사용했다면, 해당 메서드의 모든 인자가 반드시 Matcher로 제공되어야 한다.=~


## 주요 Matcher
-  any()-어떤 값이든 매칭
	- 특정 타입의 어떤 값이든 인자로 전달될 때
	- any(Class<T> type)또는 anyInt(),anyString(),anyLong()등 기본 타입별 변형 존재
- eq()-특정 값과 정확히 매칭
	- equals()메서드를 사용하여 정확히 일치하는 값일 때 매칭된다. 
	- 원시 타입이나 String,Integer 등 **equals()메서드가 있는 객체에서 사용이 가능**하다.
	- eq(T value)
- argThat()-커스텀 조건으로 매칭
	- 사용자 정의 조건에 따라 인자 매칭
	- argThat(ArgumentMatcher<T> matcher) 또는 argThat(T value -> condition)(람다)
- isNull()/notNull -null여부 매칭
	- 인자가 null인지 아닌지 매칭
	- isNull(String.class),notNull 등
- anyList(),anySet(),anyMap,anyCollection() -컬렉션 타입 매칭
	- 특정 타입의 컬렉션 인스턴스가 전달될 때 사용
