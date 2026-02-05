## git add 동작 원리
1. 파일 작업 (Working Tree)
    test.txt 파일을 생성하거나 수정하는 등 로컬 작업 디렉토리에서 작업이 이루어집니다.
2. git add 실행 (Staging)
    git add test.txt 명령어를 실행하면, 파일의 내용이 blob 오브젝트로 .git/objects에 저장되고 해당 파일의 체크섬이 계산됩니다.
    - **파일 내용이 동일하면 항상 같은 체크섬이 생성됩니다.**
3. 인덱스 업데이트 (Staging)
    Git은 .git/index 파일에 test.txt의 경로와 함께 위에서 생성된 blob 오브젝트의 체크섬을 기록합니다.
    - 이 과정이 완료되면, `test.txt`는 **tracked** 상태가 되며 **스테이징 영역(Staging Area)**에 포함됩니다.

add 과정 정리: 파일 생성 -> blob 타입으로 .git/object에 파일 체크섬 생성 -> .git/index에 blob타입의 체크섬을 넣음

## git commit 동작 원리
1. 트리 생성
	- /index에 기록된 파일 목록과 blob object를 기반으로 tree 객체를 생성합니다.
	- tree 객체는 커밋 시점의 디렉토리와 파일 구조의 스냅샷 입니다.
2. commit 객체 생성
	- tree 객체의 hash,**~={red}부모 커밋의 hash=~**,커밋 메시지,작성자 정보를 포함하는 commit 객체를 생성합니다.
3. HEAD 업데이트
	- HEAD가 현재 브랜치의 최신 커밋 객체를 가리키도록 업데이트 합니다
	- **~={red}HEAD가 어떤 브랜치를 가리키고 있는지는 .git/HEAD에 있습니다. 예시) cat .git/HEAD -> refs/heads/main=~**
	- **~={red}각 브랜치는 `.git/refs/head/브랜치이름` 에 최신 커밋 객체를 참조하는 형태로 존재합니다.=~**
commit 과정 정리: .git/index 기반으로 트리 생성 -> .git/object에 트리 저장 -> commit 객체가 .git/object에서 생성됨 -> commit 객체는 tree를 가리킴 -> 생성된 최신 커밋 객체를 .git/refs/heads/브랜치 이름에 넣어줌 -> .git/HEAD가 refs/heads/브랜치 이름을 가리키면 커밋 완료됨

커밋이 완료된 직후 아무 변경도 없는 상태(clean 상태): working tree,stage,commit이 가리키고 있는 HEAD가 모두 같다.


## branch의 동작 원리
1. branch의 정체: branch는 `.git/refs/heads/`에 있는 파일이다.branch를 하나 만들면 `.git/refs/heads/브랜치이름`파일이 하나 생성되고 이것을 `.git/HEAD`가 참조하는것 뿐이다
2. git ckeckout 과정: .git/HEAD에 참조 파일을 옮겨갈 브랜치 파일로 바꾸고 이 브랜치가 참조하는 (`.git/refs/head/브랜치이름`의 커밋 체크섬의) 커밋 내용으로 stage와 working tree를 바꾼다



### .git/object에 저장하는 것
- **Blob (파일)**: 파일 내용 그 자체를 나타냅니다.
- **Tree (디렉토리)**: 디렉토리와 파일들의 계층 구조를 나타냅니다. 이 객체는 하위 디렉토리(다른 Tree 객체)와 파일(Blob 객체)의 해시를 포함합니다.
- **Tag (태그)**: 특정 커밋에 의미 있는 이름을 부여하는 오브젝트입니다. 주로 릴리스 버전 관리에 사용됩니다
- **Commit (커밋)**: 특정 시점의 전체 프로젝트 스냅샷을 나타냅니다.
    - **Tree** 해시를 포함하여 해당 커밋의 파일 상태를 가리킵니다.
    - **Parent Commit** 해시를 포함하여 이전 커밋을 가리킵니다.


#### 간단한 정리
1. git add 명령어를 수행하면 working tree의 내용을 스테이지에 추가한다.
2. git commit 명령을 수행하면 스테이지의 내용으로 새로운 커밋을 만든다.
3. 커밋 이후 clean한 상태는 working tree,stage,HEAD commit이 모두 동일한 내용을 담고 있다는 뜻이다.
4. 커밋 객체는 트리 객체와 blob 객체들의 조합으로 이루어져있다
5. 커밋 객체는 부모 커밋에 대한 참조를 가지고 있다.
6. 브랜치를 생성하면 단순히 `.git/refs/heads/`디렉토리에 브랜치 파일을 하나 추가한다
7. 브랜치를 checkout하면 HEAD를 해당 브랜치로 변경하고 브랜치가 참조하는 커밋의 내용으로 스테이지와 워킹트리의 내용을 변경한다. 