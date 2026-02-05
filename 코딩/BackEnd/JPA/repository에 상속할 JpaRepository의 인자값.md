- 어떤 repository에 JpaRepository를 상속할 때 JpaRepository에 들어갈 인자값 2개
1. **==해당 repository를 사용하는 Entity==**(repository에 종속될 Entity)
2. **==해당 Entity의 Id의 데이터 타입**==
```java
public interface UserRepository extends JpaRepository<UserEntity, Long>{}
//첫번째 인자-> UserEntity
//두번째 인자-> UserEntity의 Id 타입 Long
```