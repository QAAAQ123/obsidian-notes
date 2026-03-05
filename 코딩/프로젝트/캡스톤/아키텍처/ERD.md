## 테이블

### 사용자

1. user
    1. user_id(PK)
    2. email
    3. provider
    4. provider_id
    5. lang
    6. nickname
    7. birthdate
    8. mbti
    9. profile_img_url
    10. is_profile_img_enabled
    11. companion_type
    12. sasang_type
    13. deleted_at(선택-soft delete)
    14. created_at
    15. **persona_id(FK)** -> user:persona = N:1
2. persona
    1. persona_id(PK)
    2. persona_name
    3. persona_tone_style
    4. persona_overlay_img_url
    5. persona_map_img_url
    6. persona_select_img_url
    7. persona_chat_img_url

### 장소

1. wishlist
    1. wish_id(PK)
    2. created_at
    3. location: DB에 없는 장소도 찜해야 하기 때문에
    4. address
    5. rating
    6. google_map_url
    7. **user_id(FK)** -> user:wishlist = 1:N
    8. **place_id(FK)** -> recommend_place:wishlist = N:1
2. recommend_place
    1. place_id(PK)
    2. place_name
    3. place_category
    4. place_detail_info
    5. place_address
    6. place_coordinates
    7. mbti_tag
    8. sasang_tag
    9. place_img_url

### 경로

1. route
    1. route_id(PK)
    2. created_at
    3. route_type
    4. **user_id(FK)** -> user:route = 1:N
2. route_detail
    1. route_detail_id(PK)
    2. **route_id(FK)** -> route:route_detail = N:1
    3. **place_id(FK)** -> recommend_place와 1:1 관계
    4. step_order

## 길찾기

1. directions_search_log
    1. search_log_id(PK)
    2. search_text
    3. searched_at
    4. **user_id(FK)** -> user:directions_search_log = 1:N
2. directions_history
    1. directions_history_id(PK)
    2. origin
    3. destination
    4. created_at
    5. **user_id(FK)** -> user:directions_history = 1:N

## 채팅

1. chat_session
    1. chat_session_id(PK)
    2. title
    3. last_message_at
    4. created_at
    5. **user_id(FK)** -> user:chat_session = 1:N
    6. **persona_id(FK)** -> persona와 1:1관계
2. chat_message
    1. message_id(PK)
    2. content
    3. author_type
    4. created_at
    5. **session_id(FK)** -> chat_session: chat_message = 1:N