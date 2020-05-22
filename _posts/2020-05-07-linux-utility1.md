---
title:  "[리눅스 다루기 (2)] 리눅스 ip, network, domain name 관련 utility 정리"
excerpt: "리눅스 환경에서 ip, network, domain에 관한 유틸리티를 다뤄 보자.."

categories:
  - study
tags:
  - linux
  - network
---

리눅스 환경에서 ip, network, domain에 관한 유틸리티를 다뤄 보자.

여기서 다루고자 하는 유틸리티는 ifconfig, ip, netstat, host, hostname, ethtool, traceroute, nslookup, ping이 있다.

## 1. ifconfig

리눅스에서 네트워크 시스템을 관리하는 유틸리티이다.

#### 용도
`ifconfig`는 일반적으로 네트워크 인터페이스의 IP 주소와 넷마스크의 설정 및 인터페이스의 활성화/비활성화 등을 위해 사용된다. 이 유틸리티를 사용하면 네트워크 인터페이스 변수를 표시하고 분석할 수 있다.

#### 예제
![]({{ site.url }}{{ site.baseurl }}/assets/images/linux-utility1/ifconfig1.png)
이렇게 네트워크 인터페이스의 설정을 확인할 수 있다.

또한 `ifconfig <인터페이스명>`으로 해당 인터페이스의 네트워크 정보만 확인할 수도 있다.
![]({{ site.url }}{{ site.baseurl }}/assets/images/linux-utility1/ifconfig2.png)

#### 옵션
- `up/down`: 인터페이스를 활성화/비활성화 시킨다.
- `netmask [주소]`: 넷마스크 주소를 설정한다.
- `broadcast [주소]`: 브로드캐스트 주소를 설정한다.

## 2. ip

ip는 리눅스에서 ip 관련 정보 조회 및 설정에 관한 명령어이다.

#### 옵션

| 옵션      | 설명                          |
| ------- | --------------------------- |
| `addr`  | ip주소와 그에 대한 특성에 관한 정보를 출력   |
| `link`  | 모든 네트워크 인터페이스의 상태를 관리하고 출력함 |
| `route` | 라우팅 테이블을 변경하거나 출력           |
| `maddr` | 멀티캐스트 ip 주소들을 관리하고 출력함      |

#### 예제
- `ip addr`
  ![]({{ site.url }}{{ site.baseurl }}/assets/images/linux-utility1/ip-addr.png)
- `ip link`
  ![]({{ site.url }}{{ site.baseurl }}/assets/images/linux-utility1/ip-link.png)
- `ip route`
  ![]({{ site.url }}{{ site.baseurl }}/assets/images/linux-utility1/ip-route.png)
- `ip maddr`
  ![]({{ site.url }}{{ site.baseurl }}/assets/images/linux-utility1/ip-maddr.png)

## 3. netstat

네트워크 연결 상태, 라우팅 테이블, 인터페이스의 상태 등을 보여주는 명령어이다.

![]({{ site.url }}{{ site.baseurl }}/assets/images/linux-utility1/ip-maddr.png)

-a 옵션 사용 시, 모든 네트워크의 연결 상태를 보여 준다.

## 4. host

도메인명에 해당하는 ip 주소를 알고 싶거나, ip 주소에 해당하는 도메인명을 알고 싶을 때 사용하는 명령어이다.
`host [옵션] [도메인 혹은 ip주소] [DNS서버]`로 사용한다.

#### 옵션

| 옵션        | 설명                 |
| --------- | ------------------ |
| `-a`      | -t ANY와 같은 기능      |
| `-d`      | 디버깅 모드             |
| `-l zone` | zone 아래 모든 시스템을 출력 |
| `-r`      | 반복 처리 하지 않음        |
| `-t type` | type를 지정하여 정보를 얻음. (A: 호스트 ip 주소, NS: 검색한 호스트의 네임 서버 호스트명, PTR: 도메인 네임 포인터, ANY: 타입의 모든 정보) |
| `-w`      | zone 아래 모든 시스템을 출력 |
| `-v`      | 반복 처리 하지 않음        |

#### 예제

`host` 명령어를 이용하여 naver.com의 ip주소를 조회해 보았다.

![]({{ site.url }}{{ site.baseurl }}/assets/images/linux-utility1/host.png)

## 5. hostname

리눅스에서 시스템의 이름을 확인하거나 변경할 때 사용하는 명령어이다.

#### 사용법
- `hostname`: 시스템 이름을 확인한다.
- `hostname [변경할 이름]`: 시스템 이름을 [변경할 이름]으로 변경한다.
  
#### 예제

![]({{ site.url }}{{ site.baseurl }}/assets/images/linux-utility1/hostname.png)

## 6. ethtool

네트워크 디바이스 드라이버와 하드웨어 세팅을 보거나 설정하는 명령어이다.

`ethtool [옵션] [ethX]`로 사용한다.

## 7. traceroute

명령어를 사용하는 리눅스 시스템에서 목적지 서버로 가는 네트워크 경로를 확인하는 명령어이다.

`traceroute [도메인 명 / ip 주소]`로 사용한다.

#### 예제

google.co.kr로 접속하는 경로를 확인해 보았다.

![]({{ site.url }}{{ site.baseurl }}/assets/images/linux-utility1/traceroute.png)

30초가 지나면 자동으로 종료된다. ICMP를 차단하는 서버에는 목적지에 대한 정보를 찾지 못한다.

## 8. nslookup

네임 서버에 질의하는 명령어이다.
ip주소로 도메인 주소를 알고자 하거나, 도메인 주소로 ip주소를 알고자 할 때 사용한다.

#### 사용법
nslookup 뒤에 조회하려는 도메인 주소를 지정한다. ip주소를 지정하여 역으로 조회할 수도 있다.

![]({{ site.url }}{{ site.baseurl }}/assets/images/linux-utility1/nslookup1.png)

MX(Mail Record)를 확인할 수도 있다.

![]({{ site.url }}{{ site.baseurl }}/assets/images/linux-utility1/nslookup2.png)

## 9. ping

외부 호스트 서버가 네트워크상으로 접근이 가능한 지 확인해보는 명령어이다.

#### 사용방법

`ping -c [요청수] -i [초단위 전송간격] [도메인명 혹은 IP주소]`로 사용한다.

[요청수]는 ping을 보낼 횟수를 의미하고, [초단위 전송간격]은 ping을 보내는 간격을 의미한다.

#### 예제

8.8.8.8로 ping을 3회 보낸 모습이다.

![]({{ site.url }}{{ site.baseurl }}/assets/images/linux-utility1/ping.png)

## 출처 및 참고자료
- https://ko.wikipedia.org/wiki/Ifconfig
- http://blog.naver.com/PostView.nhn?blogId=jsky10503&logNo=220744122731
- http://blog.naver.com/PostView.nhn?blogId=fortbank&logNo=220880020014&redirect=Dlog&widgetTypeCall=true&directAccess=false
- https://websecurity.tistory.com/103
- https://shaeod.tistory.com/950
- https://m.blog.naver.com/PostView.nhn?blogId=diceworld&logNo=220296886442&proxyReferer=https:%2F%2Fwww.google.com%2F
- https://araikuma.tistory.com/131
- https://www.lesstif.com/system-admin/nslookup-20775988.html
- https://m.blog.naver.com/diceworld/220296844545