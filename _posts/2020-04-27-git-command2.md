---
title:  "git 사용법 (2)"
excerpt: "git에 대한 심화적인 사용법을 정리해 보았다.."
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

git에 대한 심화적인 사용법을 정리해 보았다.

브랜치에 대한 것과 협업에 대해 자세히 다룰 것이다.

## 1. branch와 merge

### branch란?
git에는 branch라는 기능이 있다. 현재 버전을 그대로 두고 새로운 버전을 시험해보고, 이것이 마음에 들면 원래 버전과 합칠 때 사용한다. 

### 관련 명령어
- `git branch <branch명>`: branch명을 생략할 때는 현재 branch와 branch 목록을 띄운다. 그리고 branch명이 있으면 그 이름으로 새 branch를 생성한다.
- `git checkout <brarnch명>`: branch명에 해당하는 이름의 branch로 이동한다.
- `git merge <branch명>`: 현재 branch에서 branch명에 해당하는 이름의 branch를 병합한다.

### 실습
![]({{ site.url }}{{ site.baseurl }}/assets/images/git-command2/git_branch1.png)

이런 로그가 있는 git 저장소가 있다. 여기서 "ver_B"라는 branch를 새로 만들 것이다.

![]({{ site.url }}{{ site.baseurl }}/assets/images/git-command2/git_branch2.png)

이렇게 하면 현재 branch는 ver_B에 있게 된다. 여기서 my.txt 파일을 수정 후 커밋해 보겠다.

![]({{ site.url }}{{ site.baseurl }}/assets/images/git-command2/git_branch3.png)

이후, master branch로 돌아가서 ver_B와 병합해 보도록 하겠다.

![]({{ site.url }}{{ site.baseurl }}/assets/images/git-command2/git_branch4.png)

이렇게 성공적으로 병합되었음을 알 수 있다.

여기서 branch를 왔다갔다 하며 파일을 수정해볼 수도 있다. 그리고 merge할 때 같은 부분을 서로 다르게 변경하지만 않는다면, 알아서 두 버전을 하나로 합쳐 준다. 하지만 변경된 부분이 같은데 그 내용이 같지 않다면, merge할 때 충돌이 일어난다.

이번에는 master와 ver_B branch에서 서로 다른 내용을 변경해 보자.

아래는 master에서의 변경이다.

![]({{ site.url }}{{ site.baseurl }}/assets/images/git-command2/git_branch5.png)

아래는 ver_B에서의 변경이다.

![]({{ site.url }}{{ site.baseurl }}/assets/images/git-command2/git_branch6.png)

이제 master로 돌아가서 ver_B와 병합을 한다.

![]({{ site.url }}{{ site.baseurl }}/assets/images/git-command2/git_branch7.png)

병합을 했더니, master에서 추가했던 old.txt와 ver_B에서 추가했던 new.txt가 모두 존재함을 알 수 있다.

### 충돌!?

![]({{ site.url }}{{ site.baseurl }}/assets/images/git-command2/merge_conflict.png)

merge할 때 충돌이 일어나면 이런 식으로 충돌난 부분을 표시해 준다. 이를 해결하고 commit하면 된다.

## 2. 되돌리기

- `git revert`: 특정 시점의 commit를 없었던 것으로 한다. 예를 들면 ver1, ver2, ver3 순서로 commit이 되어 있다고 할 때 `git revert HEAD`를 사용하면 commit log가 ver1, ver2, ver3, ver2의 순서로 된다.
- `git reset <commit hash code>`: 해시코드에 있는 commit 버전으로 돌아간다. revert와 달리 commit했던 사실 자체가 날아간다. ver1, ver2, ver3에서 ver1, ver2로 변경되는 것이다.
- `git rebase --i --root`: git commit 버전을 잠시 이전으로 돌린다. 주로 이전의 commit 메세지를 변경하고자 할 때 사용한다.

## 3. 협업

> "회사 기술블로그 repository를 포크떠서 후기 작성하시고 저한테 PR 보내주세요"

https://wayhome25.github.io/git/2017/07/08/git-first-pull-request-story/ 에서 언급된 문구이다.

여기서 포크는 무엇이고 PR은 또 무엇일까? 이 두 용어는 git(github)에서 협업할 때 많이 사용되는 용어들이다.

- 포크: 어떤 remote repository에서 내 remote repository로 가져오는 것이다. 포크할 시 자신의 remote repository에 포크된 것이 생성된다.
- PR(Pull request): 풀리퀘스트는 어떤 remote repository의 사용자에게 내가 포크해 와서 변경한 사항을 적용시키고자 요청하는 것이다. 그 사용자는 내 변경사항을 확인하고 적용시키거나, 그렇지 않을 수 있다.

사실 더 자세한 내용은 위 사이트에 있으므로... 들어가서 확인해 보는 것도 좋을 것이다. 추후 여기에도 정보를 더 추가해 볼 예정이긴 하다.