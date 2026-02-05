#### 설치
1. Python 다운
2. pip로 fastapi 다운
3. pip로 uvicorn 다운
4. `pip freeze > requirements.txt`로 라이브러리 및 버전 저장

## uvicorn 실행
- `uvicorn main:app`으로 실행
- `--reload`를 넣어주면 코드 변경될 때마다 재실행-> 코드 변경할 때마다 실행할 필요 없음
- `uvicorn main:app --reload`를 run.bat에 넣고 run.bat만 입력하면 코드 자동 실행
- main은 py파일의 이름 main.py로 만들었으니 main
- app은 FastAPI()의 객체 변수 이름
- 종료는 `ctrl + c`

## API 문서 
- /docs에 자동으로 API문서를 작성해준다(Swagger 이용)
- /redoc으로 API에 관한 정보를 더 직관적이고 상세하게 전달한다
	- 샘플 데이터를 넣을 수 있다
	- 작성한 모델 아래에 Config 클래스를 만들면 샘플 데이터를 넣을 수 있다
	- ```python
	  #2.xxx 버전 이상에서 샘플만들기 위해서는  Field 임포트 해야함
	  class Todo(BaseModel):
		  id: int = Field(example=1)
		  item: str = Field(example="Example Schema")
		  
		  #python 1.xx 버전에서 사용하던 코드(Field 임포트 불필요)
		  class Config:
			schema_extra = {
				"example" : {
					"id": 1,
					"item": "Example Schema"
					}
			}
	  ```