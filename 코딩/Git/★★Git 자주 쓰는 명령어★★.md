1. git commit --amend: 깃 커밋 메시지 수정
2. git branch: 브랜치 확인
3. git checkout <브랜치명>
4. git add -u: tracked file만 스테이지에 올림
5. git add -A: untrack,tracked,deleted 파일을 스테이지에 올림
6. git log --oneline --graph
7. git reset --soft <커밋 해쉬> 또는 git reset --soft HEAD~숫자: HEAD만 지정된 커밋으로 이동(스테이지와 워킹 디렉토리는 변경하지 않음)
8. git reset <커밋 해쉬>: HEAD와 스테이지만 지정된 커밋으로 이동(워킹 디렉토리 변경하지 않음)
9. git reset --hard <커밋 해쉬> 또는 git reset --hard HEAD~숫자: HEAD,스테이지,워킹 디렉토리 모두 해당 커밋으로 변경
10. git push origin 브랜치이름: 원격저장소에 이미 만들어져있거나 존재하지 않는 원격 브랜치에 로컬 브랜치에 있는 커밋 내용을 push함
    ex)로컬의 test와 원격의 origin/test가 있을때 test브랜치에서 git push origin test하면 로컬 test에 있는 커밋 내역이 원격 test브랜치에 push됨
11. git show <커밋_해시>:<파일_경로>
12. git reset HEAD <파일명>: <파일명>만 스테이지에서 제거
13. git rm -r --cached <파일명>
14. git commit --amend -m "커밋 메시지"
15. git merge <branch명>: ~={red}(반드시 병합할 브랜치에 있어야함)=~현재 브랜치에 <branch명> 브랜치를 병합한다
16. git branch -m <기존이름> <새로운이름>
17. git remote -v:원격 리파지토리 연결 확인
18. git pull origin main: git pull from origin main branch
19. git branch -d 브랜치명: safety delete
20. git push origin --delete 브랜치명
21. git fetch --all --prune: pull 하기전 정보 갱신(Pull은 fecth + merge)
22. git merge origin/브랜치명: fetch로 받아온 브랜치의 변경 사항을 fast-forward로 merge
23. git push origin 브랜치명 --force-with-lease
	1. **`--force-with-lease`:** 누군가 그 사이에 내 브랜치(`feature/route`)에 다른 코드를 올리지 않았는지 확인하고, 안전할 때만 덮어씌웁니다.

### Git stash 명령어
- git stash save
- git stash list
- git stash apply <stash 이름>
- git stash drop
- git stash show

### HEAD~과 HEAD^의 차이
- HEAD~: n단계 이전의 커밋
- HEAD^: 병합 커밋의 n번째 부모
	- `HEAD^`: 병합 커밋의 첫 번째 부모
	- `HEAD^2`: 병합 커밋의 두 번째 부모 (병합 대상 브랜치)