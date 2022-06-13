---
title:  "AWS Windows Server - EC2"
excerpt: "Windows Server 실습"

categories: cloud
tags:
  - [aws, ec2, windows, apache2]

toc: true
toc_sticky: true
 
date: 2022-06-13-00:01
last_modified_at: 2022-06-13-00:01
---
### Windows 인스턴스 생성
* * *
- windows 2016 base 선택
- RDP (Remote Desktop Protocol) (내 IP만 허용) 외에 나머지 설정은 동일

#### Windows server RDP 연결
- 인스턴스 -> 연결 탭 -> RDP 연결
- 암호 가져오기
- 키페어 불러오기
- 암호해독
- 암호 복사 해놓기

#### Windows 원격 데스크톱 연결 실행
- IP 입력
- 사용자 : Administer
- 암호 : 복사한 암호 입력

#### Server Manager 세팅
- 시작 -> Server Manager 실행
- Add roles and features
- Server Roles 탭에서 Web Server(IIS) 만 체크
- next, next, next -> install
- Web Browser 에서 퍼블릭 IP 입력해서 잘 웹사이트가 잘 뜨는지 확인

![](../../assets/images/20220613-133626.png){: .center}

#### inetpub 에서 웹사이트 디렉토리 확인
- 윈도우 탐색기 열기
- this pc
- inetpub > wwwroot 가 웹사이트 경로

![](../../assets/images/20220613-133742.png){: .center}