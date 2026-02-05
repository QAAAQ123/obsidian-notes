`git pull`은 원격의 변경 사항을 가져와(`fetch`) 즉시 내 코드에 합치는(`merge`) 명령어입니다. 이를 분리해서 실행하면 어떤 코드가 들어오는지 미리 눈으로 확인하고 합칠 수 있어 훨씬 안전합니다.

안전한 업데이트를 위한 3단계 프로세스를 소개합니다.

### 1단계: 원격 변경 사항 가져오기 (Fetch)

먼저 원격 저장소의 최신 내역을 로컬의 "원격 추적 브랜치(remote-tracking branch)"로 가져옵니다. 이 단계에서는 내 작업 파일이 전혀 변경되지 않습니다.

Bash

```
# 원격 저장소(origin)의 최신 이력을 가져옴 (파일 변경 없음)
git fetch origin
```

### 2단계: 변경 내용 미리보기 (검증)

이제 가져온 내용이 내 작업에 어떤 영향을 줄지 확인합니다. 주로 `main` 브랜치를 기준으로 설명해 드립니다.

A. 들어오는 커밋 목록 확인

어떤 커밋들이 새로 들어왔는지 제목만 빠르게 훑어봅니다.

Bash

```
# 내 로컬(HEAD)에는 없지만 원격(origin/main)에는 있는 커밋 목록 조회
git log HEAD..origin/main --oneline
```

B. 실제 코드 변경점 확인 (가장 중요)

실제로 어떤 코드가 수정되었는지 상세히 비교합니다. 충돌 가능성을 미리 파악할 수 있습니다.

Bash

```
# 내 현재 상태와 원격 브랜치 간의 코드 차이 비교
git diff HEAD origin/main
```

> **Tip:** 파일명만 보고 싶다면 `git diff --name-only HEAD origin/main`을 사용하세요.

### 3단계: 안전하게 합치기 (Merge/Rebase)

변경 내용을 확인했고 문제가 없다고 판단되면, 이제 실제로 합칩니다.

Bash

```
# 방법 1: 일반 병합 (Merge Commit 생성됨)
git merge origin/main

# 방법 2: 리베이스 병합 (추천: 히스토리 깔끔하게 유지)
git rebase origin/main
```

---

### 요약: 안전한 `git pull` 대체 워크플로우

한 번에 `git pull`을 하는 대신 아래 순서로 습관을 들이시면 실수를 크게 줄일 수 있습니다.

1. `git fetch origin` (가져오기)
    
2. `git log HEAD..origin/main --oneline` (뭐가 바뀌었나?)
    
3. `git merge origin/main` (합치기)
    

다음 단계:

매번 이 과정을 타이핑하기 번거로우시다면, git up 같은 단축 명령어(Alias)로 이 과정을 한 번에 등록해 드려도 될까요?