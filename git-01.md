# git(lecture_01)

## CLI(Command Line Interface)

### 개요

터미널 환경에서 명령어를 통해 유저와 컴퓨터가 상호작용 할 수 있는 방식

## 실습

현재 윈도우 환경에서 작업중이어서 Unix기반의 git bash를 설치해서 CLI환경을 실습한다.

### 설치

1. git bash 설치
   1. <https://git-scm.com/downloads> 링크로 접속하여 다운로드 파일 설치(운영체제에 맞는 환경을 선택)
   2. 설치파일 실행 후 따로 변경사항 없이 **NEXT**클릭 후 설치
   3. 설치 완료 후 실행
2. vscode 설치
   1. <https://code.visualstudio.com/Download> 링크로 접속하여 다운로드 파일 설치(운영체제에 맞는 환경을 선택)
3. Typora 설치
   1. <https://typora.io/windows/dev_release.html> 링크로 접속하여 다운로드 파일 설치(운영체제에 맞는 환경을 선택)
   2. 설치완료 후 프로그램 실행하여 **환경설정** 변경
      1. 파일 > 환경설정 > 이미지 탭 > 항목 선택중 '블라블라 경로로 이미지 복사' 선택
      2. 이미지 탭에서
         - [x] '가능하다면 상대적 위치 사용' 체크

### 기본 명령어(git bash)

1. `~` : home directory 표시
2. `/` : rooot driectory 표기
3. `cd` : home directory로 이동
4. `cd 'fold name' or cd ..` : 폴더로 접근 또는 상위 폴더로 이동
5. `mkdir 'ford name'` : 폴더 생성
6. `mv 'file name' '../file name'` : 파일을 상위 폴더로 이동
7. `touch 'file name.extension'` : 파일 생성
8. `rm 'file name' or rm -rf 'fold name'` : 파일 or 폴더 삭제
9. `code .` : 현재 경로('.')에서 vscode 실행

### 기본 명령어(Typora)

````markdown
# 가장 큰 제목

## 2번째 제목

### 3번째 제목

이것은 문단(내용)입니다.

## 목록

### 순서 없는 목록

- 족발
  - 뽕나무 쟁이
- 순대국
  - 농민 백암 순대
- 모츠나베
  - 후쿠오카 모츠나베

### 순서 있는 목록

1. 기본 설치
2. CLI 학습
3. md 학습

## 텍스트 스타일링

*italic*

**Bold**

`inline code`

폴더를 이동할 때는, `cd` 명령어를 사용합니다.

## 코드 블록

```python
def my_func():
	print('Hello')
```

## 표

| 이름 | 메일  | 주소 | 전화번호    |
| ---- | ----- | ---- | ----------- |
| 용현 | pkyh1 | 안산 | 01012341234 |
| 김   | pkyh2 | 수원 | 01012344321 |
````

### vscode gitbash 연동

1. 터미널 창 실행 후 터미널 이름 옆에 ^반대 표시 선택 후 기본 프로필 선택(git bash)

### git 사용법

#### git 명령어

- `git init` : Git repository 생성 (폴더 업그레이드)
  - git repo -> .git 폴더 생성하면 하위 폴더는 관리 대상이 된다.
  - `rm -rf .git` : 폴더 다운그레이드
- `git status` : 현재 repo 상태
  - commits : verstioning
  - Untracked files : 추적되지 않은 파일
- `$ git add <file name>` : 추가
- `$ git commit -m <first commit>` :  커밋하는데 메시지를 'first commit'라고 남긴다
- `$ git config --global user.name` : 유저 네임
- `$ git config -- global user.email` : 유저 이메일
- `$ cat ~/.gitconfig` : 입력한 네임과 이메일이 잘 입력 되었는지 확인
- `$ git log` : commit 로그 확인

#### git 개념

- 변경사항이 git add 후 스테이징된 변경사항으로 이동
- `$ git restore --staged <file>` : staging 된 파일이 modify로 이동
- 처음 생긴 파일은 Untracked file이다. -> 그래서 add를 통해 tracking 해준다.

#### git 사용 주의사항

- `~` 에서 `$ git init` 진행 하면 안됨

- repo 안에 repo 만들기 하면 안됨

- `$ git init`하기 전에

  - 디렉토리 확인
  - (master) 확인

- `$ git commit` 할 때는 `-m` 옵션주고 무조건 메시지 입력해야함

  

### github 사용법

- github - TIL 폴더 생성
- 내 컴퓨터에서 TIL 폴더에서 github TIL 폴더에 동기화
- commit 내용을 github TIL 폴더에 전송
- github에 원격 저장소 생성하기
- 원격 저장소(remote repo) 등록하기
  - `git remote add origin <repo URL>` : 원격 저장소의 레포지토리 등록
  - `git remote -v` : 원격 저장소 확인하기
  - `git push -u origin master` : 원격 저장소로 전송