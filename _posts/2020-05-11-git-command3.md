---
title:  "git 사용법 (3)"
excerpt: "위의 사진처럼 협업하기 위해서는 어떤 과정을 거쳐야 할까?.."
toc: true
toc_sticky: true
toc_label: "페이지 주요 목차"

categories:
  - study
tags:
  - linux
  - git
  - github
---

![]({{ site.url }}{{ site.baseurl }}/assets/images/git-command3/pull-request.png)

위의 사진처럼 협업하기 위해서는 어떤 과정을 거쳐야 할까? [git 사용법 2]({{ site.url }}{{ site.baseurl }}/%EB%A6%AC%EB%88%85%EC%8A%A4/git-command2/)에서 다룬 pull request를 세부적으로 공부해 보았다.

## 1. fork
우선 팀원의 remote repository를 포크해 온다.

그리고 포크해온 내 remote repository를 로컬 저장소에 클론한다.

```
$ git clone [내 remote repository URL]
```

## 2. remote 설정
로컬 저장소와 현재 연결되어 있는 remote repository는 원본 remote repository이다. 이제 로컬 저장소와 내 remote repository를 연결시켜야 한다.

```
# 원본 프로젝트 저장소를 원격 저장소로 추가
$ git remote add [별칭] [원본 remote repository URL]

# 원격 저장소 설정 현황 확인방법
$ git remote -v
```

이렇게 하면 origin이라는 별칭으로 내 remote repository와, 새로 정한 별칭으로 원본 remote repository와 연결된다.

## 3. branch 생성
여기서 branch를 만드는 이유는, 원본을 살려 두고 새 branch에서 pull request를 보내기 위한 작업을 하기 위함이다. 원본에서 작업을 하면 나중에 원본에서 변경사항이 생겼을 때 pull할 시 충돌이 발생할 수 있다.

```
# develop 이라는 이름의 branch를 생성한다.
$ git checkout -b develop
Switched to a new branch 'develop'

# 이제 2개의 branch가 존재한다.
$ git branch
* develop
  master
```

## 4. 작업 후 add, commit, push
작업을 완료하고 commit한 이후, 우선 내 remote repository(origin)로 push해야 한다.

```
$ git push -u origin master
```

## 5. pull request
push를 하고 나면, 내 remote repository에 Compare & pull request 버튼이 활성화 되어 있다.
이 버튼을 클릭하여 pull request를 보낼 수 있다.

## 6. merge 이후 동기화
PR이 승낙되고 원본에 merge되면, 내 로컬 저장소에 이를 반영해야 한다. 

```
# 우선 master branch로 돌아가야 한다.
$ git checkout master

# 그 다음 원본에서 pull해온다.
$ git pull [원본 remote 별칭]

# 그리고 작업했던 branch를 제거한다.
$ git branch -d develop
```

이제 새로운 작업을 할 때마다 `git pull [원본 remote 별칭]`을 해준 후 3~6을 반복해 주면 된다.


## 출처
[https://wayhome25.github.io/git/2017/07/08/git-first-pull-request-story/](https://wayhome25.github.io/git/2017/07/08/git-first-pull-request-story/)