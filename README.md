# Git 요약 정리

## 기본 명령어

- 작업 영역 지정: `git init`
- 변경사항 감지: `git status`
- HEAD 내용 확인: `git log`
- 인덱스에 저장: `git add .`
- HEAD(저장소)에 저장: `git commit " "`

## Reset

- `git reset --soft <해시>`: 커밋만 되돌림
- `git reset --mixed <해시>`: 커밋과 스테이징 되돌림
- `git reset --hard <해시>`: 커밋, 스테이징, 작업 파일 모두 되돌림
- `git reflog` 후 `git reset --hard <reflog 해시>`: 과거 작업 상태로 이동

## Commit 수정

- 메시지만 변경: `git commit --amend -m "새 메시지"`
- 내용도 변경: `git commit --amend`

## 브랜치

- 생성: `git branch <name>`
- 변경: `git checkout <name>`
- 생성 + 변경: `git checkout -b <name>`
- 내용 추가: `touch xxx.확장자`

## Merge

- Fast-forward: `git checkout master` → `git merge topic`
- Three-way: 충돌 발생 시 수동 해결 후 `git add`, `git commit`
- Merge 로그 남기기: `git merge --no-ff 브랜치명`
- Octopus Merge: `git merge branch1 branch2` (3개 이상 병합)
- 충돌 해결: 충돌된 파일 수정 → `git add`, `git commit`

## Rebase

- 기본: `git rebase -i HEAD~N`
  - `pick`: 그대로 유지
  - `reword`: 메시지 수정
  - `edit`: 커밋 수정
  - `squash`: 이전 커밋과 병합
  - `drop`: 제거
- continue: `git rebase --continue`
- abort: `git rebase --abort`
- 다른 브랜치 히스토리 합치기: `git rebase origin/master`, 이후 `git cherry-pick <커밋ID>`로 보존 커밋 복원

## 원격 저장소

- 추가: `git remote add origin <주소>`
- 확인: `git remote -v`, `git ls-remote`
- 연결 취소: `git remote rm origin`
- 다운로드: `git fetch origin master`
- 병합 포함 다운로드: `git pull origin master`
- 초기화 + 연결 + pull: `git clone <주소>`

## Push

1. 브랜치명이 같은 경우: `git push origin <브랜치명>`
2. 추적 브랜치 설정 시: `git push`, `git pull`만으로도 가능
3. 브랜치명이 다를 경우: `git push origin 로컬:원격`

## 추적 브랜치 설정

1. `git checkout -b 로컬브랜치 origin/원격브랜치`
2. `git branch --set-upstream-to=origin/원격브랜치 로컬브랜치`
3. `git branch -u origin/원격브랜치 로컬브랜치`
4. `git push -u origin 로컬브랜치:원격브랜치`

## 브랜치 병합

- Fast-forward: 변경 사항이 한 쪽 브랜치에만 있는 경우
- Three-way: 양쪽에 변경 사항이 있는 경우
- Squash: 커밋 여러 개를 하나로 합쳐 병합 → `git merge --squash <브랜치>`
- Rebase: 브랜치 위에 커밋을 재배치해 히스토리를 직선으로 정리
  - `git rebase <브랜치>`
  - `git rebase --continue`
  - `git rebase --abort`

## 커밋 되돌리기

- `git reset --hard HEAD~N`: 커밋, 스테이징, 작업 파일 삭제
- `git reset --mixed HEAD~N`: 커밋, 스테이징 삭제, 작업 파일 유지
- `git reset --soft HEAD~N`: 커밋만 삭제
- `git revert <해시>`: 푸시된 커밋을 되돌리는 새 커밋 생성

## cherry-pick

- 커밋 복사 및 적용: `git cherry-pick <해시1> <해시2>`

## 브랜치 삭제

- 로컬 브랜치 삭제: `git branch -d 브랜치명`
- 원격 브랜치 삭제: `git push origin --delete 브랜치명`

## 기타

- 브랜치 시각화: `git log --graph --oneline --all`
- ours 전략 머지: `git merge 브랜치명 --strategy=ours`
- 강제 push: `git push -f origin 브랜치명`
- 원격 브랜치 받아오기: 
  - `git fetch origin` 후 `git checkout -b 브랜치명 origin/브랜치명`
  - 혹은 `git checkout -b 브랜치명` → `git pull origin 브랜치명`
