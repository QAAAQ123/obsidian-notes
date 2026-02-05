	### 🔧 `git pull`의 동작 방식

실제로는 두 가지 명령을 **연속적으로 실행**하는 것과 같음


```bash
git fetch
git merge origin/브랜치명
```


1. `git fetch`: 원격 저장소의 최신 변경 사항을 로컬로 가져옴 (하지만 병합은 하지 않음)
    
2. `git merge`: 가져온 변경 사항을 현재 작업 중인 브랜치에 병합