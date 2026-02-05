[hashcode,equals 메소드 공식 문서](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html)

- hashcode,equals 두 메소드는 java.lang.Object에 있다

## public boolean equals(Object obj)
- 다른 객체가 이 객체와 "동일한 객체를 참조하는지" 여부를 나타낸다
- equals method를 override할 때마다 hashcode method도 override해야 한다
- 매개변수: Object,리턴: boolean(ture,false)

## public int hashcode()
- Java 애플리케이션 실행 중에 동일한 객체는 항상 같은 hashcode 리턴 값을 가진다
- hashcode()는 객체의 내용(equlas() 비교에 쓰이는 값)을 기반으로 정수 값을 계산한다
- equlas(Object)메소드가 false면, 반드시 두 객체의 hashcode가 다른 것이 아니다

## hashcode
- 객체를 식별하기 위한 정수 값
- hash기반 컬렉션에서 객체를 빠르게 검색,저장하기 위한 용도로 사용된다
- 두 객체가 equals로 같다면 hashcode() return 값도 같아야한다
