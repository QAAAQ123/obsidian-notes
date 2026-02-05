## when()을 사용하는 이유

`given` 단계에서 `when()`과 실제 값을 사용하는 것은 **테스트가 시작되기 전, Mock 객체가 실제와 같이 특정 상태를 가지고 있도록 설정하는 Mockito의 표준적인 방법**입니다.
예시) PUT 요청을 테스트 하려면 반드시 리퍼지토리 안에 수정 전 데이터가 있어야 한다.이럴때 when()을 사용해서 테스트 코드에 리퍼지토리에 이 데이터가 들어가 있다고 알려주는것(설정하는 것)이다.
```java
//given-리퍼지토리 기본 데이터 설정  
// 1. 기존 데이터 (DB에 이미 저장되어 있다고 가정)  
Long postId = 1L;  
Post existingPost = new Post(1L, "원래 제목", "원래 내용", LocalDateTime.now());  
Media existingMedia1 = new Media(1L, "image/jpeg", sampleJpgBytes, existingPost);  
Media existingMedia2 = new Media(2L, "image/png", samplePngBytes, existingPost);  
List<Media> existingMediaList = Arrays.asList(existingMedia1, existingMedia2);  
  
//2. findById가 호출되면 기존 post,meida 반환하도록 설정  
when(mockPostRepository.findByIdOrElseThrow(postId)).thenReturn(existingPost);  
when(mockMediaRepository.findAllByPostPostId(postId)).thenReturn(existingMediaList);  
  
//when-수정을 위한 데이터  
PostDTO inputPostDTO = new PostDTO(null, "수정된 제목", "수정된 내용", null);  
MediaDTO inputMediaDTO2 = new MediaDTO(2L, "image/jpeg", sampleJpgBytes);  
MediaDTO inputMediaDTO3 = new MediaDTO(null, "image/gif",samplePngBytes);  
List<MediaDTO> inputMediaDTOList = Arrays.asList(inputMediaDTO2, inputMediaDTO3);  
PostAndMediaDTO inputPostAndDTOList = new PostAndMediaDTO(inputPostDTO,inputMediaDTOList);  
// 실제 서비스 메서드 호출  
PostAndMediaDTO updatedResult = mockPostService.updatePost(postId, inputPostAndDTOList);
```
