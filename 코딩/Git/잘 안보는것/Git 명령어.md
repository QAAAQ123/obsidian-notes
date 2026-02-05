1. .gitignore 적용 확인:git check-ignore -v <파일명>
2. github 연결: git remote add `원격저장소별명` <github repository 주소>
   여러개의 github 저장소 연결 가능(fork할때 주로 사용)


## git push
- github 처음 push: git push -u origin main
- - **`origin`**: 원격 저장소의 별칭(alias)입니다. Git은 원격 저장소의 URL 대신 `origin`이라는 별명을 기본적으로 사용합니다.
    
- **`main`**: 로컬 브랜치의 이름입니다. 이 명령어는 현재 로컬의 **`main`** 브랜치에 있는 커밋들을 원격 저장소의 **`main`** 브랜치로 올리겠다는 의미입니다.
    
- **`-u` (또는 `--set-upstream`)**: 이 옵션은 로컬의 `main` 브랜치가 원격의 `origin/main` 브랜치를 추적하도록 설정합니다. 이 설정을 통해 다음부터는 `git push`나 `git pull` 명령을 실행할 때 원격 저장소 이름과 브랜치 이름을 생략하고 단순히 `git push` 또는 `git pull`만 입력해도 동작하게 됩니다.

## git log
1. git log: HEAD와 관련된 커밋 자세하게 나옴
2. git log --oneline: 간단하게 
3. git log --oneline --graph --decorate
4. git log --oneline --graph --all -decorate: 모든 브랜치 보는 명령어
5. git log --oneline -n5: 최신 커밋 5개만 
6. 개별 파일 커밋 확인: git log <파일명>
	1. -p: 변경 내용까지 확인
	2. --follow: 이름 변경 추적

## git add
1. 추가,수정,삭제된 모든 파일 스테이지에 올림: git add -A
2. git이 추적 중인 파일 중에 수정,삭제된 파일 스테이지에 올림(untrack 미포함): git add -u
3. **~={red}git rm -r --cached <파일이름>=~**: 파일 스테이징 취소

## git fetch
- git fetch [원격저장소별명] [브랜치이름]
- 현재 저장소에 원격 저장소의 데이터를 받아와 동기화 하는 명령어
- **`git fetch`**는 원격 저장소의 변경 사항(커밋, 파일 등)을 로컬 `.git` 디렉터리에 다운로드하지만, 현재 작업 중인 **커밋 기록**, **스테이징 영역**, **워킹 디렉토리**에는 영향을 주지 않습니다.

---

##### Git Fetch의 정확한 역할

`git fetch`는 원격 저장소의 상태를 반영하는 **원격 추적 브랜치(remote-tracking branch)**를 업데이트하는 역할을 합니다. 예를 들어, `git fetch origin`을 실행하면 `origin/main`과 같은 로컬의 원격 추적 브랜치가 원격 저장소의 `main` 브랜치 최신 상태로 동기화됩니다.

이 과정에서 원격에만 존재하는 커밋 객체와 파일 객체는 로컬 `.git` 디렉터리에 저장되지만, 현재 체크아웃된 로컬 브랜치나 워킹 디렉터리의 파일들은 전혀 변경되지 않습니다.

`git fetch`는 원격 저장소의 변경 사항을 안전하게 확인만 하고 싶을 때 유용합니다. 변경 사항을 로컬 작업 내용과 병합하려면 **`git merge`** 또는 **`git rebase`** 명령어를 별도로 실행해야 합니다.

## git pull
`git fetch`와 merge or rebase를 동시에 수행하는 명령어

