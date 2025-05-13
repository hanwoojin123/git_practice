# Git 정리

## 작업 영역 및 기본 명령어

- **작업 영역 지정**: `git init`
- **변경사항 감지**: `git status`
- **HEAD 내용 확인**: `git log`
- **인덱스에 저장**: `git add .`
- **HEAD(저장소)에 저장**: `git commit " "`

## Reset

- **reset 방법**:
  ```bash
  git reset --soft <해시값 앞 4~5자리>
  git reset --mixed <해시값 앞 4~5자리>
  git reset --hard <해시값 앞 4~5자리>
  ```

- **reflog 방법**:
  ```bash
  git reflog
  git reset --hard <reflog 해시값>
  ```

## Commit 수정

- **커밋 메시지만 변경**:
  ```bash
  git commit --amend -m "새 메시지"
  ```

- **커밋 내용 변경 가능**:
  ```bash
  git commit --amend
  ```

## 브랜치

- **branch 생성**: `git branch <name>`
- **branch 변경**: `git checkout <name>`
- **branch 생성 + 변경**: `git checkout -b <name>`
- **branch에 내용 추가**: `touch <filename>`

## Merge

- **Fast-forward merge**:
  ```bash
  git checkout master
  git merge topic
  ```

- **Three-way merge**:
  ```bash
  git merge topic
  # ESC -> :wq 저장 후 종료 / :q 종료
  ```

- **Merge 충돌 해결**: 충돌 파일 수정 후 `git add` + `git commit`

## Rebase

- **특정 파일 제거**:
  ```bash
  git rebase -i HEAD~<갯수>
  # pick → d 변경
  ```

- **이름 변경**:
  ```bash
  git rebase -i HEAD~<갯수>
  # pick → r 변경 → 이름 수정
  ```

- **히스토리 병합**:
  ```bash
  git rebase -i HEAD~<갯수>
  # pick → s 변경 → 메시지 통합
  ```

- **서로 다른 브랜치 히스토리 병합**:
  ```bash
  git rebase origin/master
  git cherry-pick <커밋 ID>
  ```

## 원격 저장소

- **원격 저장소 추가**: `git remote add origin <url>`
- **추가 확인**: `git remote -v` / `git ls-remote`
- **원격 푸시**: `git push origin master`
- **원격 저장소 삭제**: `git remote rm origin`
- **원격 내용 다운로드**: `git fetch origin master`
- **다운로드 + 병합**: `git pull origin master`
- **init + remote + pull**: `git clone <url>`

## 원격 저장소 브랜치 관리

- **미완 작업 다른 브랜치로 푸시**:
  ```bash
  git branch <new_branch>
  # 내용 추가 후 push
  ```

- **연결되지 않은 원격 브랜치 가져오기**:
  ```bash
  git fetch origin
  git checkout -b <local_branch> origin/<remote_branch>
  # 또는
  git checkout -b <local_branch>
  git pull origin <remote_branch>
  ```

- **병합 (Squash Merge)**:
  ```bash
  git checkout master
  git merge --squash --topic
  git commit ""
  ```

- **Fast-forward Merge 로그 남기기**: `git merge --no-ff <branch>`
- **원격 브랜치 삭제**: `git push --delete origin <branch>`
- **강제 푸시**: `git push -f origin <branch>`

## Git 명령 요약

- **git clone**: 원격 저장소를 로컬로 복사
- **git remote**: 원격 저장소 연결
- **git fetch**: 원격 저장소 상태 동기화 (자동 병합 없음)
- **git fetch origin <remote_branch>:<local_branch>**: 로컬 브랜치 강제 덮어쓰기 가능 (위험)
- **git pull --rebase origin main**: rebase로 병합
- **git log --graph --oneline --all**: 브랜치 공통 조상 확인
- **git merge branch1 branch2 --strategy=ours**: 현재 브랜치 우선 병합
- **git push origin --delete <branch>**: 원격 브랜치 삭제
- **git branch -d <branch>**: 로컬 브랜치 삭제

## Push / Pull 요약

1. **로컬 & 원격 브랜치 명 동일**:
   ```bash
   git push origin <branch>
   git pull origin <branch>
   ```
2. **추적 브랜치 설정된 경우**:
   ```bash
   git push
   git pull
   ```
3. **브랜치명이 다르고 추적 브랜치 없음**:
   ```bash
   git push origin <local>:<remote>
   git pull origin <remote>
   ```

## 커밋 되돌리기

- **reset (히스토리 변경)**:
  ```bash
  git reset --hard HEAD~N
  git reset --mixed HEAD~N
  git reset --soft HEAD~N
  ```

- **reflog 기준**:
  ```bash
  git reset --hard HEAD@{N}
  ```

- **revert (푸시 후에도 가능)**:
  ```bash
  git revert <커밋 해시>
  git revert A^..C
  git revert -n A^..C
  ```

- **checkout 커밋 해시**: 특정 시점으로 HEAD만 이동

## 병합 종류

- **Fast-forward Merge**: 현재 브랜치에만 커밋이 있을 때
- **Three-way Merge**: 양쪽 브랜치가 변경된 경우 (충돌 가능)
- **Octopus Merge**: 3개 이상 브랜치 병합 (충돌 발생 쉬움)
- **Rebase**: 히스토리를 깔끔하게 정리 (주의 필요)
  ```bash
  git rebase --continue
  git rebase --abort
  ```

- **git rebase -i HEAD~N**: 커밋 편집
  - `pick`: 유지
  - `reword`: 메시지 수정
  - `edit`: 내용 수정
  - `squash`: 앞 커밋과 합치기 (메시지 포함)
  - `fixup`: 메시지 생략하고 합치기
  - `drop`: 삭제

## Squash Merge

- 여러 커밋을 하나로 합친 후 병합:
  ```bash
  git merge --squash <branch>
  ```

## Cherry-pick

- 다른 브랜치의 커밋 복사:
  ```bash
  git cherry-pick <commit1> <commit2> ...
  ```

## 추적 브랜치 설정

1. `git checkout -b <local> origin/<remote>`
2. `git branch --set-upstream-to=origin/<remote> <local>`
3. `git branch -u origin/<remote> <local>`
4. `git push -u origin <local>:<remote>`