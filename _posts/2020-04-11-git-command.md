---
title:  "git 사용법"
excerpt: "버전관리 시스템인 git의 사용법을 정리해 보았다.."

categories:
  - study
tags:
  - linux
  - git
  - github
---

버전관리 시스템인 git의 사용법을 정리해 보았다.

자신의 프로젝트에 git을 사용하게 된다면, 버전관리에 있어서 매우 편할 것이다. 일일이 파일들을 복사해서 관리할 필요 없이 명령어 몇줄이면 버전관리가 끝나니 말이다.

또한 github라는 웹사이트가 있는데 이를 활용하면 서버에 내 프로젝트를 올려서 다른사람이 내 코드를 볼 수도 있고, 일련의 설정을 거치면 다른사람이 내 코드 수정도 할 수 있다. 즉, 협업을 하기에 유용하다는 것이다.

## 1. git 시작

#### git 설치
Windows 환경에서는 [여기](https://git-scm.com/)에 들어가서 설치하면 되고,
Mac 환경에서는 터미널에서 `brew install git`와 `brew upgrade git` 명령어를 실행하면 설치된다.

git을 처음 사용하는 경우,
```
git config --global user.name "<자신의 username>"
git config --global user.email "<자신의 email>"
```
명령어를 사용하여 git global 값을 초기화해야 한다.

#### git 시작
git을 시작하는 데는 크게 두 가지 명령어가 있다.
1. `git init`: 현재 디렉토리에서 새로운 깃 저장소를 만든다. 

2. `git clone`: 현재 디렉토리에서 다른 곳의 저장소를 복제해 가져온다. 주로 github에 있는 원격 저장소를 로컬로 가져오는 데 쓰인다.

![](https://chanhk-im.github.io/assets/images/git-command/git_init.png)
여기에서는 기존에 진행하고 있던 CRUD 프로젝트에 git이 관리하게 해 보았다.

## 2. stage
git에서는 파일이 변경되면 자동으로 관리해 주는 것이 아니라, 사용자가 명령어를 통해 stage라는 곳에 올려줘야 한다.

1. `git status`: git이 관리하고 있는 파일들의 상태(변경되었는지, stage에 올라와 있는지)를 표시해 준다.
   
2. `git add <파일이름>`: 파일을 stage에 올린다. -A 옵션을 사용하면 모든 변경된 파일이 stage에 올라간다.

다음은 필자의 프로젝트에서 변경된 파일을 stage에 올려보는 과정이다.

![](https://chanhk-im.github.io/assets/images/git-command/git_status.png)
`git status` 명령어를 사용하여 파일들의 상태를 확인하고,

![](https://chanhk-im.github.io/assets/images/git-command/git_add.png)
`git add` 명령어를 통해 stage상에 파일들을 올린다(위의 경우에는 Main.c 파일이 stage상에 올라감). 그 이후 다시 `git status`명령어를 사용한 모습이다.

## 3. commit
stage 상에 올라온 파일을 이제는 git 저장소에 기록해야 한다. 이를 위해 commit이라는 과정을 거치게 되는데, commit을 하게 되면 새로운 버전이 저장되게 된다. 이를 위한 명령어는 `git ccommit`이다. 

commit을 위해서 commit message를 남겨야 하는데, `git commit` 명령어 뒤에 -m 옵션을 붙인 후 메세지를 남기면 된다(메세지는 큰따옴표로 감싸야 한다).
```
git commit -m "first commit"
```

![](https://chanhk-im.github.io/assets/images/git-command/git_commit.png)
실제로 프로젝트에서 commit를 한 화면이다. 이를 위해 미리 Main.c 외에 3개의 파일을 더 stage에 올려놓은 상태였다. 4개에 파일이 commit되었다는 것을 알 수 있다.

## 4. github
이렇게 git이 관리하는 디렉토리는 github에 올려볼 수도 있다. 

#### github 원격 저장소 생성
우선 [github](https://github.com/)에 들어가 가입한 후, 왼쪽에 있는 New 버튼을 누르거나, 중간의 Start a project 버튼을 누르면 새 원격 저장소를 만들 수 있다.
![](https://chanhk-im.github.io/assets/images/git-command/github_homepage.jpeg)

이후 Repository name에는 원하는 이름을 적어주면 된다.
![](https://chanhk-im.github.io/assets/images/git-command/github_create.png)

#### 로컬 저장소와 연결하기
이제 github 원격 저장소와 아까 commit했던 로컬 저장소를 연결시킬 차례이다.
`git remote add origin <방금 생성한 로컬 저장소의 웹사이트 주소>.git` 명령어를 사용하면 바로 연결된다.
![](https://chanhk-im.github.io/assets/images/git-command/git_remote.png)

#### push 하기
push는 로컬에서 commit한 것을 원격 저장소에 올리는 것이다.

`git push -u origin master` 명령어를 사용하면 원격에 올라간다. github username과 password를 요구할 수도 있다.

![](https://chanhk-im.github.io/assets/images/git-command/git_push.png)

push 이후, 자신의 원격 저장소에 들어가 보면 다음과 같이 업데이트되었음을 확인해 볼 수 있다.
![](https://chanhk-im.github.io/assets/images/git-command/github_afterpush.png)

#### 변경사항 확인
이후에 Main.c에 변경사항이 생겨 이를 다시 commit 후 push하는 과정을 거쳤다.   
그런데, github에서는 변경사항을 확인할 수 있다. 

여기서 "# commits"를 눌러보자(#는 어떤 숫자).   
그러면 commit의 목록을 볼 수 있는데, 원하는 부분을 보면 어떻게 파일이 변경되었는지를 보여주는 창이 나온다.

![](https://chanhk-im.github.io/assets/images/git-command/github_change.png)

## 그 외 git 명령어
 - `git log`: git 저장소의 로그를 출력한다. 커밋했던 정보를 볼 때 사용한다. --oneline 옵션을 사용 시 좀 더 간략하게 볼 수 있다.
![](https://chanhk-im.github.io/assets/images/git-command/git_log.png)
 - `git diff`: 변경사항을 비교하는 명령어이다. branch명이나 commit hash를 비교할 수 있다.
![](https://chanhk-im.github.io/assets/images/git-command/git_diff.png)
 - `git checkout <file>`: 수정사항을 원래대로(원래의 커밋 상태로) 되돌린다.
![](https://chanhk-im.github.io/assets/images/git-command/git_checkout1.png)
파일을 다음과 같이 수정하고, 위의 명령을 사용하면
![](https://chanhk-im.github.io/assets/images/git-command/git_checkout2.png)
![](https://chanhk-im.github.io/assets/images/git-command/git_checkout3.png)
수정사항이 변경 전으로 되돌아와있는 것을 알 수 있다.
 - `git reset <file>`: stage에 있는 file을 unstaged상태로 돌려 놓는다.
![](https://chanhk-im.github.io/assets/images/git-command/git_reset.png)