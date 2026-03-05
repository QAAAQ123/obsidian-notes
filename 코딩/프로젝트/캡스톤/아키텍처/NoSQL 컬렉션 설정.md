## 데이터
1. user
    1. id
    2. role(RBAC)
    3. createdAt
    4. updatedAt
    5. deletedAt
    6. email
    7. provider
    8. providerId
    9. lang
    10. name
    11. birthdate
    12. userImgUrl
    13. userImgAsMarker(true|false)
    14. companionType
    15. mbtiType
    16. sasangType
    17. selectedPersonaId-선택한 페르소나 
    18. tourCourses(array형태,최대 개수 정하기)
	    1. id
        2. createdAt
        3. updatedAt
        4. name
        5. address
        6. detailInfo-길어지면 분리하거나 Limit걸기
        7. googleMapUrl
    19. restaurantCourses(array형태,최대 개수 정하기)
        1. id
        2. createdAt
        3. updatedAt
        4. name
        5. address
        6. detailInfo-길어지면 분리하거나 Limit걸기
        7. googleMapUrl
    20. favoritePlaces(array형태,최대 개수 정하기)
        1. id
        2. createdAt
        3. googlePlaceId
        4. name
        5. location-GeoJSON(2dsphere)
        6. address
        7. rating
        8. googleMapUrl
    21. recentPlaceSearches(array형태,최근 5개)
        1. id
        2. createdAt
        3. searchedText
    22. recentDirections(array형태,최근 5개)
        1. id
        2. createdAt
        3. origin
        4. destination
2. authToken
	1. id
    2. userId
    3. refreshToken
    4. createdAt
    5. expireAt-TTL적용
3. persona
    1. id
    2. createdAt
    3. updatedAt
    4. name
    5. toneStyle
    6. overlayImgUrl
    7. mapImgUrl
    8. selectImgUrl
    9. chatImgUrl
4. chatSession-채팅 메시지가 너무 길어지면 새로운 세션 생성해야함
    1. id
    2. userId
    3. title-MyInfo에 보여줄때 내용 요약용(선택)
    4. createdAt
    5. lastMessageAt
    6. messages(array형태)
        1. id
        2. createdAt
        3. content
        4. authorType
        5. **messageVector**
5. recommendedPlace
    1. id
    2. googlePlaceId
    3. createdAt
    4. updatedAt
    5. name
    6. category
    7. detailInfo-길어지면 분리하거나 Limit걸기
    8. address
    9. location-GeoJSON(2dsphere)
    10. mbtiTag
    11. sasangTag
    12. recommendedPlaceImgUrl
    13. googleMapUrl
    14. **placeVector**
    15. **mergedTextForVector**

### 페르소나 메인에 띄워주는 방법 선택
1. 프론트에서 실행할때 한번 persona doc의 정보를 가져가서 가지고 있다가 json으로 personaId에 맞는 것만 띄워주는 방식
2. 백에서 DTO에 매번 persona 데이터를 보내주는 방법

### Chat session에 이미지 띄우기
- chat session에 입장할 때 한번만 userImg와 personaImg의 URL을 state로 저장하고 채팅 세션이 끝날때까지만 가지고 있는다

```plantuml
@startuml

' 스타일 설정
!theme plain
hide circle
hide methods
skinparam linetype ortho
skinparam roundcorner 10
skinparam shadowing false
skinparam class {
    BorderColor Gray
    ArrowColor Gray
}

' 1. User Collection (메인) - 파란색 계열
class "User (사용자)" as User #E3F2FD {
    id
    createdAt
    updatedAt
    deletedAt
    email
    provider
    providerId
    lang
    nickname
    birthdate
    userImgUrl
    userImgAsMarker
    companionType
    mbtiType
    sasangType
    selectedPersonaId
}

' 1-1. User 내장 객체들 (Array) - 보라색 계열
class "TourCourse\n(여행 코스)" as TourCourse #F3E5F5 {
    id
    createdAt
    updatedAt
    name
    address
    detailInfo
    googleMapUrl
}

class "RestaurantCourse\n(맛집 코스)" as RestaurantCourse #F3E5F5 {
    id
    createdAt
    updatedAt
    name
    address
    detailInfo
    googleMapUrl
}

class "FavoritePlace\n(즐겨찾기)" as FavoritePlace #F3E5F5 {
    id
    createdAt
    googlePlaceId
    name
    location (GeoJSON)
    address
    rating
    googleMapUrl
}

class "RecentSearch\n(최근 검색)" as RecentSearch #F3E5F5 {
    id
    createdAt
    searchedText
}

class "RecentDirection\n(최근 길찾기)" as RecentDirection #F3E5F5 {
    id
    createdAt
    origin
    destination
}

' 2. AuthToken Collection - 노란색 계열
class "AuthToken\n(인증 토큰)" as AuthToken #FFF8E1 {
    id
    userId
    refreshToken
    createdAt
    expireAt (TTL)
}

' 3. Persona Collection - 초록색 계열
class "Persona\n(페르소나)" as Persona #E8F5E9 {
    id
    createdAt
    updatedAt
    name
    toneStyle
    overlayImgUrl
    mapImgUrl
    selectImgUrl
    chatImgUrl
}

' 4. ChatSession Collection - 붉은색 계열
class "ChatSession\n(채팅 세션)" as ChatSession #FFEBEE {
    id
    userId
    title
    createdAt
    lastMessageAt
}

class "Message\n(채팅 메시지)" as Message #FFEBEE {
    id
    createdAt
    content
    authorType
}

' 5. RecommendedPlace Collection - 청록색 계열
class "RecommendedPlace\n(추천 장소)" as RecPlace #E0F2F1 {
    id
    googlePlaceId
    createdAt
    updatedAt
    name
    category
    detailInfo
    address
    location (GeoJSON)
    mbtiTag
    sasangTag
    recommendedPlaceImgUrl
    googleMapUrl
}

' --- 관계 정의 ---

' User의 포함 관계 (Embedded Arrays)
User *-- "0..N" TourCourse : tourCourses
User *-- "0..N" RestaurantCourse : restaurantCourses
User *-- "0..N" FavoritePlace : favoritePlaces
User *-- "5" RecentSearch : recentPlaceSearches
User *-- "5" RecentDirection : recentDirections

' ChatSession의 포함 관계
ChatSession *-- "0..N" Message : messages

' 참조 관계 (References - ID로 연결)
AuthToken --> User : userId
ChatSession --> User : userId
User ..> Persona : selectedPersonaId (참조)

@enduml
```