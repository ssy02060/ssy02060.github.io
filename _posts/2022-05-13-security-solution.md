---
title:  "보안 솔루션 이론"
excerpt: "방화벽, IPS, IDS 정리"

comments: true
categories: security-solution
tags: [security, firewall]
toc: true
toc_sticky: true
 
date: 2022-05-13-22:00
last_modified_at: 2022-05-13
---

#### 용어 정리
* * *
- 침입 차단 시스템(Intrusion Blocking System) : 인터넷 방화벽
- 침입 탐지 시스템(Intrusion Detection System) : IDS
- 침입 방지 시스템(Intrusion Prevention System) : IPS (IDS + FW + 기타기능)
- 통합 위협 관리(Unified Threat Management) : UTM (IPS + Virus 탐지)


#### Router
* * *
- 서로 다른 네트워크를 연결하는 장치
    - ex) 이더넷과 Wi-Fi(공유기도  일종), 이더넷과 PPP를 연결, 이더넷과 HDLC연결 등등
- LAN과 WAN을 연결
    - LAN의 종류 : Ehternet, Token Ring, FDDI, Wi-Fi
    - WAN의 종류 : PPP, HDLC, ATM, X.25 등등
- IP주소 대역이 다른 네트워크를 연결(L3) ---> Routing Table에서 관리
- Router에 ACL(Access Control List)라우터의 를 설치하면 Screen Router라고 함
- ACL
    - 패킷이 지나가는 것을 허용 또는 거부(Permit or Deny)
    - ACL에는 허용할 IP 주소, 거부할 IP 주소 등이 적혀 있음(살생부)

#### Firewall
- 1세대
- 2세대 : Application Level (Proxy) Firewall
    - 7계층 헤더를 보고
- 3세대 : Statefull Packet Inspection(SPI) : 상태기반 방화벽
    - State Table(상태 테이블)에 트래픽의 정보를 저장하고 관리하는 방식 : 출 IP, 목 IP, 출 Port, 목 Port, 프로토콜, 방향 등등
    - 3계층과 4계층을 위주로 트래픽 상황을 보고 판단하기 때문에 안전하면서도 매우 빠름 (잘 팔림) (Check point - hardware는 nokia, sw는 check point에서,,)
    - 내부망에서 요한 트래픽을 기록하기 떄문에 외부에서 들어오는 응답 트래픽은 허용(네트워크의 정황(context)를 고려)
    - 외부망에서 들어오려는 트래픽은 거부
    - Stateful : 네트워크의 흐름을 고려해서 처리한다는 의미
- 4세대 : Dynamic Packet Filter
    - 능동적으로 차단하는 기능이 있는 방화벽
    - 요청 개수가 임계값을 초과하는 경우 --> 공격으로 간주 --> 스스로 차단
- 5세대 : Kernel Proxy, Secure OS 개념 도입

#### 방화벽 배치 방법
- Screened Host
    - 모든 트래픽은 Bastion Host(Proxy Server)를 들렀다가 가야 함.
    - 웹 브라에서 Proxy 주소를 Bastion Host로 설정하면 됨(안하면 인터넷 연결 X)
    - 외부에서는 Bastion Host외에는 보이지 않기 때문에 가려진 호스트라고 함
- Dual Home 방식
    - 외부망 인터페이스와 내부망 인터페이스가 분리 되어 있음.(물리적 분리)
    - 트래픽을 내부망과 외부망으로 서로 다른 네트워크로 구성할 수 있음(내부망은 사설IP, 외부방은 공인IP--> NAT 설정)
    - ex) 공유기 포트 WAN port 와 LAN port 다름
    - 방화벽에 문제가 생기면 네트워크가 분리된 상태가 되므로 상당히 안전
- Screened Subnet
    - DMZ라는 영역이 생겨 외부에서는 DMZ 영역만 보임.
    - 외부망과 내부망 사이 DMZ 구간을 두어서 네트워크를 완전히 분리


#### Smart View Tracker 실습
* * *
##### 용어설명
- Module : 방화벽 본체
- smart console : check point 관리하는 컴퓨터

##### 방화벽 규칙 작성 방법(Rule Set)
- 맨 마지막 줄에는 모두 거부를 배치(Deny All Philosophy : 모두 거부 원칙)
- 최소한 하나의 허용이 있어야 함
- 큰 조건은 아래로, 작은 조건은 위에 배치-> 작은 조건이 아래에 있으면 아래 있는 조건이 의미 없어짐.
 
##### 방화벽 적용 방법
- 위에서 내려가면서 적용
- 해당 규칙을 찾으면 그 조건을 적용
- 해당 규칙을 찾지 못하면 젤 밑에 마지막 조건에 걸림
