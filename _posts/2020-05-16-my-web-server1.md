---
title:  "[내 웹서버 만들기 (1)] 간단한 웹서버 만들어 보기"
excerpt: "웹서버를 만들어 보기 이전에 맛보기로 호스팅 사이트를 이용해 간단한 웹서버를 만들어 보자.."

categories:
  - study
tags:
  - web
---

웹서버를 만들어 보기 이전에 맛보기로 호스팅 사이트를 이용해 간단한 웹서버를 만들어 보자.

우선, 웹서버는 아래 그림과 같은 구조로 작동한다.
![](https://chanhk-im.github.io/assets/images/my-web-server1/web-server1.png)

서버로 사용할 컴퓨터에 서버 환경을 만들어준다면, 웹서버를 만들어볼 수 있을 것이다.
서버 환경을 세팅하는 것은 다음으로 미루어 두고, 지금은 호스팅 사이트에서 웹서버를 만들어 볼 것이다.

## 1. 회원 가입 및 호스팅
호스팅은 서버 컴퓨터의 일부를 사용자에게 이용할 수 있게 하는 서비스이다.
여기서는 닷홈이라는 곳에서 호스팅을 받을 것이다.

우선 [여기](https://www.dothome.co.kr/)에 가서 회원가입을 하자.
그리고 "웹호스팅 > 무료 호스팅"에 들어간다. 그 다음 무료 호스팅을 신청한다(CMS 설치안함을 체크).
정보를 몇개 적어 주면 손쉽게 신청할 수 있을 것이다.

## 2. filezilla
FTP는 파일 전송 프로토콜의 준말로 호스팅을 이용할 때 파일을 다운/업로드 할 수 있는 프로그램이다.
그 중 filezilla라는 프리웨어로 이용 가능한 프로그램을 사용할 것이다.

#### filezilla 설치 및 실행
[여기](https://filezilla-project.org/)로 들어가서 자신의 컴퓨터에 맞게 다운받아 실행하자.
실행하면 아래와 같은 화면이 나올 것이다.
![](https://chanhk-im.github.io/assets/images/my-web-server1/filezilla.png)

#### filezilla 사용
![](https://chanhk-im.github.io/assets/images/my-web-server1/filezilla2.png)
이부분을 작성하고 빠른 연결을 클릭한다.
- 호스트: 호스팅받은 사이트 주소를 입력
- 사용자명: 호스팅받을 때 정한 FTP 아이디 입력
- 비밀번호: 호스팅받을 때 정한 FTP 비밀번호 입력
- 포트: FTP 접속 시 이용할 포트. 빈 칸으로 두면 된다.

접속 성공 시 아래와 같은 화면이 나온다.
![](https://chanhk-im.github.io/assets/images/my-web-server1/filezilla3.png)

몇 개의 메세지가 나오고 오른쪽의 리모트 사이트에 무언가가 나타난다.
![](https://chanhk-im.github.io/assets/images/my-web-server1/filezilla4.png)

여기서 메모장 등으로 `index.html` 파일을 만들고 아래의 내용으로 써넣는다.
```html
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <title></title>
  <meta name="author" content="">
  <meta name="description" content="">
  <meta name="viewport" content="width=device-width, initial-scale=1">

  <link href="css/normalize.css" rel="stylesheet">
  <link href="css/style.css" rel="stylesheet">
</head>

<body>

  <p>Hello, world!</p>

  <script src="https://ajax.googleapis.com/ajax/libs/jquery/2.1.4/jquery.min.js"></script>
  <script src="js/script.js"></script>
</body>

</html>
```
그 후 이 파일을 리모트 사이트에 보이는 html 폴더 안에 집어 넣는다(왼쪽의로컬 사이트에서 찾아 넣기).
