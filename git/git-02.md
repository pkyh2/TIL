# git(lecture_02)

## NEVER

1. `~` 에서 `$ git init` 진행
2. 리포 안에 리포 만들기
3. `$ git init` 입력 전 확인할 점
   1. `~` 인지
   2. `(master)` 떠 있는지

## Project commit

### 프로젝트 초기화

```sh
# pjt 폴더 생성
$ mkdir new_project

# 폴더로 이동
$ cd new_project

# README 파일 & .gitignore 생성
$ touch README.md .gitignore
# gitignore.io 에 접속하여 필요한 내용 복-붙

# 폴더를 리포로 초기화
$ git init

# README & .gitignore 파일 add(tracking)
$ git add .

# commit
$ git commit -m 'first commit'

# 원격 저장소 생성 @ github.com
# 생성한 원격 저장소 등록
$ git remote add origin <URL>

# 등록된 저장소 확인
$ git remote -v

# 지금까지의 commit push
$ git push origin master
```

## git pull

```sh
#새로운 컴퓨터에서 기존 원격 저장소 복제하기
$ git clone <URL>

#새로운 컴퓨터에서 파일 수정 후
$ git add .
$ git commit -m 'message'
$ git push origin master

#git 로그 확인해서 최신 commit인지 확인
$ git log --oneline

#집 컴퓨터에서 새로운 컴퓨터에서 push한 내용을 받아오려면
$ git pull origin master
```

**항상 pull먼저 하고 자리에서 일어날때 push!!**



## git conflict

`git log --oneline --graph` 를 통해 최신 로그 상태를 확인 후 `git pull` 과 `git push` 를 적절히 해준다.

2개 이상의 컴퓨터에서 같은 원격 저장소 사용시

1. `$ git add .`

2. `$ git commit -m '메세지'`

3. `$ git push origin master`

   1. 잘 된다 => 8번으로

   2. 안된다 => 4번으로

      ```
      hint: Updates were "rejected" because the tip of your 
      current branch is behind
      hint: its remote counterpart. Integrate the remote changes (e.g.
      hint: '"git pull" ...') before pushing again.
      hint: See the 'Note about fast-forwards' in 'git push --help' for details.
      ```

4. `$ git pull origin master`

   1. 자동 병합(auto merge)이 일어난다. =>8번으로

      - `$ git status` 에 아무런 알림이 없다.

   2. 자동 병합에 실패 => 5번으로

      ```
      ...
      Automatic merge failed; fix conflicts and then commit the result.
      ```

5. `vscode` 에서 직접 빨간불 들어온 파일을 수정한다.

6. `$ git add .`

7. `$ git commit -m '충돌 해결'`

8. `$ git push origin master`



## git branch

#### 주요 명령어

- `(master)` : branch name

- `git branch` : 현재 branch들의 목록

- `git branch <name>` : branch 생성

- `git branch -d <name>` : branch 삭제

- `git switch <name>` : branch 변경

- `git switch -c <name>` : branch 생성 및 변경

- (master) - 완성된 프로그램

- 나머지 branch들은 따로 개발되다가 master에 merge 되기 위해 

- `git merge <name>` : (master)가 <name> 을 merge하겠다.

  - conflict 발생하면 vscode에서는 충돌 변경 설정

  - 후 git add. -> git commit -m ' '

#### github에서 branch 사용 (협업)

1. settings -> manage access (add people)

2. git repo 생성

3. 사람 마다 clone 해주기

4. 각자 repo에서 브랜치 생성

5. 작업 => add => commit => 반복
6. `$ git push origin <branch name>`
7. 리모트에서 PR(pull request) 생성
8. 팀원들끼리 코드 리뷰 및 최종 merge 결정
   1. Auto merge 가능(초록색) => 진행
   2. Conflict 발생(빨간색) => github에서 수동으로 수정 후 merge 진행
9. 로컬 master 브랜치에서 `$ git pull origin master`
10. 작업 반복 

#### commit 삭제 or 복구 방법(추천x)

- reset
- amend
- rebase
