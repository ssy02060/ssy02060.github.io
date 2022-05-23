---
title:  "웹 보안 - 이론"
excerpt: "HTTP Protocol에 대한 이해"

comments: true
categories: web-security
tags: [http]
toc: true
toc_sticky: true
 
date: 2022-05-04-22:02
last_modified_at: 2022-05-04
---

### HTTP 프로토콜이란?
- Stateless (비연결 유지 프로토콜) : 이미지, 텍스트 등을 다운로드 받으면 연결이 끊김
- cf. Telnet, SSH, FTP --> 연결이 계속 유지됨
- HTTP/1.0 : 파일 다운로드 받을 때마다 연결 설정 해제를 반복
- HTTP/1.1 : 한번의 연결에 여러 개의 파일을 다운로드
- HTTP/2 : 헤더 압축(바이너리), 파이프라인, Server Push 기능
  - 지원되는 브라우저 : Win10 IE11, Chrome, Edge, Firefox9 이상
  - HEADERS frame 과 DATA frame 으로 구성

![](../../assets/images/20220523-192549.png){: .center}

![](../../assets/images/20220523-192600.png){: .center}