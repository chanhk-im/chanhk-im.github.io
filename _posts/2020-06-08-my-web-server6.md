---
title:  "[내 웹서버 만들기 (6)] 라즈베리파이에 CMS 구축해 보기 (3) - backup, wordpress"
excerpt: "CMS 구축해 보기 시리즈 마지막이다.."
toc: true
toc_sticky: true
toc_label: "페이지 주요 목차"

categories:
  - study
tags:
  - web
  - raspberry-pi
  - linux
  - cms
  - db
  - mysql
  - mariadb
  - backup
  - wordpress
---

CMS 구축해 보기 시리즈 마지막이다. 백업과 wordpress라는 CMS 서비스를 설치해볼 것이다.

## 1. backup
서버 내의 데이터를 통째로 날려버렸다고 생각해 보면 끔찍하지 않은가... 백업은 당연히 중요한 것이다. 여기서는 백업 스크립트를 작성하고, 주기적으로 백업하도록 만들어 볼 것이다.

### 1.1. backup 스크립트 작성

super user로 진행한다.

우선, 백업용 디렉토리를 만든다.
```
# mkdir /backup
# chmod 700 /backup
```

그리고, /root 디렉토리에서 스크립트 파일을 만든다.

```
# pwd
/root
# vi backup.sh
# chmod 700 backup.sh
```

![]({{ site.url }}{{ site.baseurl }}/assets/images/my-web-server6/backup1.png)

/home 디렉토리에 있는 데이터를 압축해서 백업하고, userdb database를 백업한다.

### 1.2. backup 스크립트 테스트

```
# ./backup.sh
```
를 치면 아까 작성한 스크립트를 실행할 수 있다.

![]({{ site.url }}{{ site.baseurl }}/assets/images/my-web-server6/backup2.png)

### 1.3. 백업 주기적으로 실행하기

cron이라는 것으로 스크립트를 자동으로 실행하게 만들어 보자.

cron은 유닉스 기반 운영체제에의 시간 기반 잡 스케줄러이다.

```
# crontab -e
```

를 치면 텍스트 에디터를 선택하라는 메시지가 나온다. 원하는 텍스트 에디터를 선택한다.

그러면 선택한 에디터가 나오고, 뭔가 써져 있다.

![]({{ site.url }}{{ site.baseurl }}/assets/images/my-web-server6/backup3.png)

여기서 맨 밑에 숫자를 5개 적고 그 뒤에 명령어를 적는데, 숫자 5개는 각각 분, 시, 날짜, 월, 요일이다. 예를 들면 0 4 * * *은 모든 월, 날짜의 4시 0분에 명령어를 실행한다는 것이다.

```
0 4 * * * /root/backup.sh 1>dev/null 2>dev/null
```
를 맨 아래에 추가한다.

## 2. Wordpress 설치

wordpress는 가장 대표적인 CMS이다. db는 지난번에 세팅한 userdb를 사용한다.

전에 만들어 뒀던 사용자(chanhk)로 접속한다.

### 2.1. 설치파일 다운로드

/home/chanhk/html 디렉토리에서 다운받는다. 그리고 압축을 풀어 준다.

```
$ wget https://ko.wordpress.org/latest-ko_KR.tar.gz
$ tar -xzvf latest-ko_KR.tar.gz
```

### 2.2. 설치 페이지

이전에 설정해 둔 도메인에 /wordpress를 붙여 접속한다. 

여기서는 chanhk.com/wordpress로 접속한다.

db 정보를 적는 페이지가 나오면 데이터베이스 이름, 사용자명, 비밀번호를 알맞게 적어 준다.

그런데 wp-config.php를 만들 수 없다고 나올 수 있다. 이럴 땐 페이지에서 요구하는 대로 복사한 후, /home/chanhk/html/wordpress 디렉토리로 가서 wp-config.php를 vim이나 cat을 이용하여 붙여넣어 준다.

그리고 관리자 id/pw도 입력해 주고, 기본 설정을 마치면 완료다.

### 2.3. wp 관리
wp 관리를 위해 디렉토리의 권한을 변경시켜 줘야 한다. super user로 들어와서,
```
# chgrp -R www-data ~/html/wordpress/wp-content
# chmod -R 775 ~/html/wordpress/wp-content
```
를 실행한다.

그리고 wp-config.php의 맨 밑에 다음을 추가한다.

```
define('FS_METHOD', 'direct);
```