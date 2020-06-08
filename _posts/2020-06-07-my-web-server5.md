---
title:  "[내 웹서버 만들기 (5)] 라즈베리파이에 CMS 구축해 보기 (2) - DB 시스템 설치 / 사용법"
excerpt: "라즈베리파이에 CMS를 구축해 보자.."
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
---

[내 웹서버 만들기 시리즈 모음]({{ site.url }}{{ site.baseurl }}/my_web_server/)

라즈베리파이에 CMS를 구축해 보자. 지난 포스트와는 달리 db를 구축하고 db를 cms 서비스에 연결해볼 것이다. 

여기서는 MariaDB라는 db 관리 시스템을 설치하고 테스트해볼 것이다.

## 1. MariaDB란?

> MariaDB는 오픈 소스의 관계형 데이터베이스 관리 시스템이다. MySQL과 동일한 소스 코드를 기반으로 하며, GPL v2 라이선스를 따른다. 오라클 소유의 현재 불확실한 MySQL의 라이선스 상태에 반발하여 만들어졌으며, 배포자는 몬티 프로그램 AB와 저작권을 공유해야 한다.
>
>[[MariaDB - 위키백과, 우리 모두의 백과사전]](https://ko.wikipedia.org/wiki/MariaDB)


## 2. MariaDB 설치
CMS의 db를 담당할 MariaDB를 설치해 보자.

super user로 진행해야 한다.

### 2.1. 설치
```
# apt-get -y install mariadb-server
```

### 2.2. 기본 설정파일 수정
아래의 파일에서 내용을 수정해야 한다.
```
# vi /etc/mysql/mariadb.conf.d/50-server.cnf
```

![]({{ site.url }}{{ site.baseurl }}/assets/images/my-web-server5/db1.png)

```
# vi /etc/mysql/mariadb.conf.d/50-client.cnf
```

![]({{ site.url }}{{ site.baseurl }}/assets/images/my-web-server5/db2.png)

그 후 서버를 재시작한다.
```
# service nginx restart
```

### 2.3. root로 MariaDB에 접속
root로 MariaDB에 접속한다. 설치할 때 설정했던 비밀번호를 입력해 준다.
```
# mysql -u root -p
```

![]({{ site.url }}{{ site.baseurl }}/assets/images/my-web-server5/db3.png)

## 3. MariaDB 다뤄보기

### 3.1. 새 db 만들기
이제 새로운 db를 만들어 보자.
userdb라는 이름의 db를 만들고자 한다.

```
> create database userdb;
```

* **반드시 끝에 ;를 붙여야 함!**

### 3.2. db 권한이 부여된 사용자 만들기
userdb에 접근가능한 새로운 사용자를 만들어 보자.

```
> grant all privileges on userdb.* to 'db_chanhk'@'localhost' identified by '1234';
> flush privileges;
```

1234는 db_chanhk라는 사용자의 비밀번호가 되고, 이 사용자는 userdb에 접근하고 수정할 수 있는 권한이 부여된다.

### 3.3. 새 사용자로 접속, 테스트

이후 quit / exit / Ctrl-C 등을 이용하여 빠져 나간 후, root 권한을 가진 사용자가 아닌 다른 사용자로 라즈베리파이에 재접속한다.

```
$ mysql -u db_chanhk -p
```

그리고 userdb에 접근이 정상적으로 되는 지 테스트해본다.

```
> use userdb;
Database changed
```
이렇게 뜨면 정상적으로 접근이 되는 것이다.

그렇다면, 권한을 부여받지 않은 db에 접근하면?

```
> use mysql;
ERROR 1044 (42000): Access denied for user 'db_chanhk'@'localhost' to database 'mysql'
```

이렇게 접근이 불가능함을 알 수 있다.

## 4. 간단한 테스트
db를 이용하여 간단한 채팅 웹페이지를 올려 보자.

여기서 사용할 채팅은 [여기](https://github.com/chanhk-im/Chat-Room)서 가져올 것이다.

### 4.1. 웹서비스 세팅
super user로 진행한다.

```
# vi /etc/nginx/sites-available/chanhk.com
```

아래와 같이 수정한다.

![]({{ site.url }}{{ site.baseurl }}/assets/images/my-web-server5/db4.png)

```
# service nginx restart
```

### 4.2. 채팅 웹페이지 clone
다른 사용자(여기서는 chanhk)로 접속한다.

~/html/ 디렉토리에서 아까 언급했던 채팅 웹페이지를 클론해 온다.
```
chanhk@chanhkpi:~/html $ git clone https://github.com/chanhk-im/Chat-Room.git
```

### 4.3. DB 정보 반영
아까 설정했던 db 정보를 반영한다.
```
$ vi Chat-Room/connection.php
```
![]({{ site.url }}{{ site.baseurl }}/assets/images/my-web-server5/db5.png)

### 4.4. DB에 테이블 생성
```
$ mysql -u db_chanhk -p userdb < Chat-Room/database/chat.sql
```

이후 접속 테스트를 해 보면 된다.

![]({{ site.url }}{{ site.baseurl }}/assets/images/my-web-server5/db6.png)

다음 편에 계속...