---
title:  "[내 웹서버 만들기 (3)] 라즈베리파이에 두 개의 가상 호스트 서비스 세팅하기"
excerpt: "이번에는 라즈베리파이(또는 리눅스 머신)에 가상 호스트 서비스를 세팅해 보자.."
toc: true
toc_sticky: true
toc_label: "페이지 주요 목차"

categories:
  - study
tags:
  - web
  - raspberry-pi
  - linux
---

[내 웹서버 만들기 시리즈 모음]({{ site.url }}{{ site.baseurl }}/my_web_server/)

이번에는 라즈베리파이(또는 리눅스 머신)에 가상 호스트 서비스를 세팅해 보자.

## 1. 가상 호스트 서비스?
가상 호스트 서비스는 여러 도메인이나 ip 등으로 서버에 접속했을 때 각각 다른 웹사이트를 띄우는 것이다.

하나의 웹서버라도 www.chanhk.com과 www.leelee.com으로 접속했을 때 다른 결과가 나오게 되는 것이다.

## 2. hostname 변경
가상 호스트 서비스를 세팅하기에 앞서, 내 라즈베리파이의 hostname을 바꿔 보자.

우선 root권한을 가져야 한다.
```
$ sudo su -
# raspi-config
```

![]({{ site.url }}{{ site.baseurl }}/assets/images/my-web-server3/raspi-config1.png)
그러면 이런 화면이 나온다.

여기서 2. Network Options -> N1 Hostname에 들어간다. 그러면 hostname을 설정하는 창이 나온다. 
![]({{ site.url }}{{ site.baseurl }}/assets/images/my-web-server3/raspi-config2.png)

그리고 설정화면을 빠져나오면, 자동으로 리부트되며 설정이 적용된다.

## 3. 라즈베리파이 웹서버 세팅
웹서버 사용자에게서 도메인 정보와 사용자 정보를 가져온다. 여기서는 사용자 이름이 leelee이고, 도메인을 leelee.com으로 실습해 보도록 하겠다.

### 3.1. user 추가
여기서도 root권한으로 진행해야 한다. 우선 leelee라는 user를 추가한다. 그리고 패스워드를 설정해 주면 된다.

```
$ sudo su -
# useradd -m leelee
# passwd leelee
```

### 3.2. 웹사이트 설정파일
그 다음, 사용자의 웹 사이트 설정파일을 만든다.

```
# vi /etc/nginx/sites-available/leelee.com
```

그 안에는 아래 내용을 써넣는다.
```
server {
    listen 80;
    listen [::]:80;

    server_name leelee.com

    root /home/leelee/html;
    index index.html;
}
```

### 3.3. 심볼릭 링크
/etc/nginx/sites-enabled 디렉토리에 방금 만든 설정파일의 심볼릭 링크를 생성해 주어야 한다.
```
# ln -s /etc/nginx/sites-available/leelee.com /etc/nginx/sites-enable
```

### 3.4. 설정 완료
이제 웹 서비스 세팅 테스트를 해 본다.
```
# nginx -t
```

![]({{ site.url }}{{ site.baseurl }}/assets/images/my-web-server3/test.png)
위와 같이 나오면 성공한 것이다! 그러면
```
# service nginx restart
```
를 통해 웹서비스를 제시작하면 완료다.

## 4. 웹 페이지 퍼블리싱
이제 사용자의 입장으로 넘어가 보자.

사용자는 유저명과 패스워드로 서버에 들어갈 수 있다.

ssh를 통해 접속해 보자. 그리고 `passwd` 명령어를 통해 패스워드를 바꿀 수도 있다.

그리고 `mkdir html` 명령어를 사용하여 html 디렉토리를 만들고, 그 안에 index.html 파일을 만들어서 웹페이지를 작성해 보자.
```
$ mkdir html
$ cd html
$ vi index.html
```

마지막으로, 지금은 가짜 도메인으로 실습할 것이므로 자신의 PC에서 hosts 파일을 수정함으로써 직접 ip 주소를 등록해야 한다.

아래는 각 os별 hosts파일의 위치이다.
- Mac OS: `/etc/hosts`
- Windows 10: `/windows/system32/drivers/etc/hosts`

hosts파일을 열고, 아래 내용을 중간이나 맨 아래에 써 넣는다.

```
[라즈베리파이 ip주소] [도메인명]
```

예를 들면, 
```
172.30.1.61 leelee.com
```
이런 식으로 작성한다.

## 5. 웹서비스 접속
이제 브라우저(크롬 등)를 켜서 주소창에 도메인 주소를 입력해 보자.

![]({{ site.url }}{{ site.baseurl }}/assets/images/my-web-server3/leelee-website.png)
이렇게 나오면 성공이다!

이제 3번과 4번을 다른 이름과 다른 도메인으로 반복하면 여러 웹 페이지를 만들 수 있다.
![]({{ site.url }}{{ site.baseurl }}/assets/images/my-web-server3/chanhk-website.png)

## 6. github에서 웹페이지 가져오기
github에 올라와 있는 html 템플릿 중 2개를 선정하여 가상 호스팅에 올려 보자.

맘에 드는 템플릿을 찾아서(여기서는 잘 모르겠으면 리포지토리의 루트 디렉토리에 index.html이 있는 저장소를 찾아 보자) 포크한다. 그리고 라즈베리파이 html 디렉토리에 클론한다.

```
$ git clone https://github.com/chanhk-im/universe.git
```

그 다음 웹사이트에 도메인 주소에 /저장소명을 붙여서 검색해보면, 포크해온 웹페이지가 나올 것이다.
현재 상황에서는 주소창에 `leelee.com/universe`를 넣는 것이다.

![]({{ site.url }}{{ site.baseurl }}/assets/images/my-web-server3/leelee-website2.png)

이번에는 웹페이지의 내용을 살짝 변경하고, 브라우저에서 확인해 보자.

universe라는 내용이 마음에 안들어서 amazing으로 바꾸어 보겠다(amazing이 더 마음에 안 들지만...).

index.html 파일을 열어 적절히 수정한 후 브라우저에서 다시 확인해 보면,

![]({{ site.url }}{{ site.baseurl }}/assets/images/my-web-server3/leelee-website3.png)

성공적으로 적용되었음을 알 수 있다.

이제 변경 내용을 github에 push하고 chanhk.com에도 6번을 반복해서 적용한다.

![]({{ site.url }}{{ site.baseurl }}/assets/images/my-web-server3/chanhk-website2.png)