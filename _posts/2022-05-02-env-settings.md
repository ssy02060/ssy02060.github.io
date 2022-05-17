---
title:  "Kali linux와 Metasploitable2 및 툴 설치"
excerpt: "환경 설정"

comments: true
categories: network-security
tags: [kali, metasploitable2, vmware, wireshark]

toc: true
toc_sticky: true
 
date: 2022-05-01
last_modified_at: 2022-05-01
---
### 주의 !!
* * *
해당 게시물은 공부를 하기 위해 작성된 글입니다.

아래 내용을 악의적으로 이용할 시 법적으로 처벌 받을 수 있습니다.

보안은 윤리의식도 매우 중요하답니다. 사용하기 전에 한번 더 생각 해보세요!

### [1] 개요 (툴 소개 및 환경설정)
* * *
##### Kali linux란?

- Offensive Security가 개발한 컴퓨터 운영 체제, 공격과 방어에 필요한 툴을 기본으로 탑재한 OS
- [kali]<https://kali.org> 에서 설치가능 (for VMWare 추천)

##### Metasploitable2란?

- metasploit 측에서 제공하는 취약한 VM. 우리의 희생양이 될 예정(Victim)
- [metasploitable]<https://sourceforge.net/projects/metasploitable/> 여기서 설치

##### VMWare 란?

- 위의 두 PC를 가상머신으로 돌리는 툴.
- [vmware]<https://www.vmware.com/kr/products/workstation-player/workstation-player-evaluation.html> 윈도우용 설치
- vmnet8 서브넷 마스크 192.168.5.0으로 변경 why? 다시 깔 때 마다 IP 달라지는데 공격PC 와 대상 PC의 IP 통일 시키기 위해.
- [네트워크 설정]<https://www.tobias-hartmann.net/2018/12/download-vmnetcfg-exe-fuer-vmware-workstation-15-x-player/>
- 위에 링크에서 다운받고, VMWare가 설치되어 있는 경로에다가 넣기
(C:\Program Files (x86)\VMware\VMware Player)

![](../../assets/images/20220517-230154.png){: .center}

![](../../assets/images/20220517-230201.png){: .center}

##### WireShark란?

- 네트워크 패킷 캡쳐 툴. 공격되는 모습을 실시간으로 캡쳐하고, 눈으로 볼 수 있음.

- [wireshark]<https://www.wireshark.org/>