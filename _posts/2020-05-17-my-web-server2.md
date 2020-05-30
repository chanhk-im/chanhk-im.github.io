---
title:  "[내 웹서버 만들기 (2)] 라즈베리파이에 웹 서비스 세팅하기"
excerpt: "지난 글에 이어 내 웹서버 만들기 시리즈의 두 번째인 라즈베리파이에 웹 서비스 세팅하기이다.."
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

지난 글에 이어 내 웹서버 만들기 시리즈의 두 번째인 라즈베리파이에 웹 서비스 세팅하기이다.

웹 서버 만들기 시리즈에서는 라즈베리파이에 웹 서버를 구축해볼 것이다.

여기서 사용한 라즈베리파이 모델은 Raspberry Pi 4B 모델이다.

## 1. sd카드에 라즈비안 os 설치
라즈베리파이에 들어가는 os는 주로 라즈비안을 사용한다. 그럼 라즈비안을 설치해 보자.

### 라즈비안 이미지 다운
우선 sd카드를 원래 사용하던 개인 컴퓨터에 연결한다.
그리고 [여기](https://www.raspberrypi.org/downloads/raspbian/)에 들어가서,

![]({{ site.url }}{{ site.baseurl }}/assets/images/my-web-server2/install-raspbian1.png)
위의 이미지에서 보이는 세 개의 항목 중 데스크탑과 몇몇 소프트웨어가 기본적으로 설치되는 
"Raspbian Buster with desktop and recommended software" 항목을 선택해 라즈비안 이미지를 다운받는다.

### 라즈베리파이 이미저 설치
[여기](https://www.raspberrypi.org/downloads/)로 들어가서 "Raspberry Pi Imager"를 자신의 os에 맞추어 다운받고 설치한다.

아래 이미지는 Raspberry Pi Imager 실행화면이다.
![]({{ site.url }}{{ site.baseurl }}/assets/images/my-web-server2/install-raspbian2.png)

"CHOOSE OS"를 선택하면 os를 선택하는 창이 나오는데, 여기서 "Use Custom"을 선택하고, 전에 받아 둔 라즈비안 이미지를 선택한다.(단, 라즈비안 이미지의 경로 중 한글이름이 있으면 안됨)

그리고 "CHOOSE SD CARD"를 눌러서 sd카드를 선택하고 "WRITE"를 누른다. 그러면 sd card에 라즈비안을 설치하게 된다.

### 라즈베리파이 ssh 및 Wi-Fi 설정
라즈베리파이는 기본적으로 ssh와 Wi-Fi 설정을 해 주어야 모니터 없이 첫 부팅을 할 수 있다.

sd card의 root(가장 상위) 경로에 ssh라는 빈 파일을 만들어 준다. 윈도우라면 새 텍스트 파일을 만들어서 이름을 확장자 없이 ssh로 변경해 준다.

Mac의 경우, 터미널을 실행해서 mount 명령어를 이용하여 sd card의 경로를 확인 후 이동한다.
![]({{ site.url }}{{ site.baseurl }}/assets/images/my-web-server2/setting-sd-card1.png)

이후, touch 명령어 등을 실행하여 ssh 파일을 만들어 준다.

Wi-Fi 설정은 wpa_supplicant.conf라는 파일을 만들어서 아래와 같은 내용을 써 넣는다.
```
ctrl_interface=DIR=/var/run/wpa_supplicantGROUP=netdev
update_config=1
country=US
network={
    ssid="Wifi이름(SSID)"
    psk="Wifi비밀번호"
    key_mgmt=WPA-PSK
}
```

## 2. 라즈베리파이 첫 부팅

라즈비안이 설치된 sd card를 라즈베리파이에 삽입한다(라즈베리파이 아래쪽에 sd card를 넣는 곳이 있음!). 그리고 전원을 연결해 주면 라즈베리파이가 부팅한다.

### ssh로 라즈베리파이에 접속하기

기존에 사용하던 컴퓨터를 라즈베리파이와 연결되어 있는 곳과 같은 공유기에 연결한 후, 아래와 같이 `ping raspberrypi.local`을 실행하면 라즈베리파이의 ip주소를 찾을 수 있다.

```
$ ping raspberrypi.local
PING raspberrypi.local (172.30.1.61): 56 data bytes
64 bytes from 172.30.1.61: icmp_seq=0 ttl=64 time=2.893 ms
64 bytes from 172.30.1.61: icmp_seq=1 ttl=64 time=14.578 ms
64 bytes from 172.30.1.61: icmp_seq=2 ttl=64 time=2.661 ms
64 bytes from 172.30.1.61: icmp_seq=3 ttl=64 time=1.520 ms

--- raspberrypi.local ping statistics ---
4 packets transmitted, 4 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 1.520/5.413/14.578/5.317 ms
```

여기서 찾아진 ip는 172.30.1.61이 된다.

이제 putty나 shell 등으로 위 ip의 ssh에 접속할 수 있다. 여기서 username은 pi, password는 raspberry로 기본 설정되어 있다.
```
$ ssh pi@172.30.1.61
pi@172.30.1.61's password:
Linux raspberrypi 4.19.97-v7l+ #1294 SMP Thu Jan 30 13:21:14 GMT 2020 armv7l

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
``` 

이렇게 성공적으로 집속할 수 있다!!


## 3. 웹 서버 세팅하기
우리는 웹 서버로 nginx를 사용해볼 것이다. 최근 Apache라는 웹 서버 소프트웨어를 무섭게 따라잡고 있는 웹 서버 소프트웨어이다.

### apt 업데이트 및 nginx 설치

nginx를 설치하기에 앞서, linux에서 패키지를 관리하는 툴인 apt를 업데이트한 후, nginx를 설치한다.
```
$ sudo su - 
# apt-get update 
# apt-get install nginx
```
apt를 다루기 위해서는 root권한으로 전환해야 하므로 root계정으로 진행한다.

이후 nginx를 시작한다.
```
$ service nginx start
```

### 웹 페이지 작성

vim은 설치가 되어있지 않으므로, vi를 사용하여 index.html을 작성한다.
```
$ vi /var/www/html/index.html
```

그리고 아래의 내용을 작성해 본다.
```
<h1>Welcome to My Raspberry Pi Server!!!</h1>
```

그 후 저장하고 종료한 다음, 로컬 컴퓨터(원래 쓰던 컴퓨터)에서 라즈베리파이의 ip주소를 웹 브라우저 주소창에 적어서 접속하고 결과를 확인한다.

![]({{ site.url }}{{ site.baseurl }}/assets/images/my-web-server2/my-page.png)

## 4. 마치며...

처음으로 호스팅 받아서 웹서버를 세팅한 것이 아닌 직접 웹 서버를 세팅해 보았다. 지금은 내 공유기 내에서만 내가 만든 서버의 웹 페이지를 열어볼 수 있지만, 몇몇의 설정을 거치면 세계 어디서나 내 웹 페이지를 열어볼 수 있게 할 수 있을 것이다!