---
title:  "Kali linux와 Metasploitable2 활용 DoS 공격 실습, DDoS 이론"
excerpt: "DoS Attack"

comments: true
categories: network-security
tags: [kali, metasploitable2, vmware, wireshark]
toc: true
toc_sticky: true
 
date: 2022-05-02-22:01
last_modified_at: 2022-05-02
---
### 주의 !!
* * *
해당 게시물은 공부를 하기 위해 작성된 글입니다.

아래 내용을 악의적으로 이용할 시 법적으로 처벌 받을 수 있습니다.

보안은 윤리의식도 매우 중요하답니다. 사용하기 전에 한번 더 생각 해보세요!

#### [3] DoS 공격 (Denial of Service : 서비스 거부 공격 / 서비스 방해 공격)
* * *
- 서버에 부하를 주어 정상적인 동작을 못하게 하는 것
- 대상 pc의 부하를 확인하는 명령어 : linux : top, windows : ctrl + shift + esc

##### ICMP를 이용한 공격 (Ping of Death, ICMP Flood)

- 5개의 오류메시지와 4쌍의 질의응답으로 구성 (13개의 종류)
- Destination unreachable, Time exeeded, echo request를 보내면 echo reply가 옴

```bash
$ kali : sudo hping3 --icmp -d 65000 -i u1000 192.168.5.129
```

- -d : 데이터 사이즈, -i : interval, u1000 : 1000 microsec, -a : 출발지 ip를 변경하는 옵션
- 65000 => 65000 / 1480 : 48조각
- hping3 : hacker's ping

![](../../assets/images/20220517-232821.png){: .center}

##### LAND Attack

- 출발지 IP 주소를 Target의 IP 조소로 지정한 Ping of Death
- -a 옵션으로 Source IP 를 변경 (target IP로)
- but 영원히 모르는건 아니다 why? 어디를 타고 왔는지 역추적 가능.
  
```bash
$ kali : sudo hping3 --icmp -d 65000 -i u1000 192.168.5.129 -a 192.168.5.129
```

![](../../assets/images/20220517-232904.png){: .center}

##### SYN Flooding (TCP)

- SYN 를 많이 보내서 웹서버 등이 정상 응답을 못하게 하는 공격
- SYN Flooding 공격은 포트를 지정해서 공격 ex) 80 web service port

![](../../assets/images/20220517-233006.png){: .center}

```bash
$ kali : sudo hping3 -S 192.168.5.129 -p 80 -i u1000
```

![](../../assets/images/20220517-233033.png){: .center}

##### Smurf Attack

- 출발지 IP를 Target으로 지정, 목적지 IP를 Direct Broadcast로 하는 Ping of Death
- ICMP Echo Request를 Broadcast로 뿌리면 해당 네트워크의 호스트들이 Echo Reply를 출발지로 보내려고 함
- 출발지는 Target의 IP 이므로 Target host로 같은 네트워크 내의 다른 PC들이 Echo Reply를 보내게 됨

``` bash
$ kali : sudo hping3 --icmp -d 65000 -i u1000 192.168.5.255 -a 192.168.5.129
```

![](../../assets/images/20220517-233220.png){: .center}

- 192.168.5.2 PC만 공격중 why? icmp_echo_ignore_broadcast 옵션이 1, broadcast echo 명령어를 무시하고있기 때문

![](../../assets/images/20220517-233247.png){: .center}

```bash
#  echo 관련된 환경변수를 확인하는 명령어
$ sudo sysctl -a | grep echo
# 환경변수를 write 하는 옵션
$ sudo sysctl -w net.ipv4.icmp_echo_ignore_broadcast=0
```

![](../../assets/images/20220517-233322.png){: .center}
- 192.168.5.128 에 해당 명령어를 추가했을때 공격에 가담한걸 확인할 수 있음 .

**Scanning vs DoS 차이?**
- scaning은 여러 포트를 보는 것 -> 응답이 있나 없나 확인
- DoS 는 한 포트만 조지는 것

#### DDoS 공격
* * *
**공격 단계**

- 관리가 소홀한 컴퓨터를 해킹해서 (C&C 프로그램을 설치 - Command & Control ) -> 30개 정도 확보
- Zombie로 감염시킬 악성코드 유포(맵핵 등과 같은 불법 프로그램) -> 2000대 ~ 3만대
- Zombie에 감염되면 C&C 에 자신이 감염되었음을 알림
- C&C에 Zombie 리스트 확보 됨 -> 판매 or 공격 시 활용
- Attacker는 C&C를 옮겨 다니면서 공격 명령을 내림

**보안 업체의 솔루션**

- 일부러 Zombie 에 감염을 시킴
- Zombie가 어디로 연락하는지 확인 -> Wireshark로 C&C IP 체크
- Zombie에게 신호를 보내는 C&C IP를 찾아서 차단

**DDoS 공격의 주기? 항상**

- 확인하는 사이트

- https://horizon.netscout.com/
- https://livethreatmap.radware.com/
- DDoS 공격은 사람이 하는 것이 아니고 Bot (자동화된 프로그램) 이 공격을 수행
- 암호화폐의 80%, 주식 거래의 70% 이상은 Bot이 거래함