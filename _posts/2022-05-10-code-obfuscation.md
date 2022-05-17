---
title:  "난독화"
excerpt: "code obfuscation"

comments: true
categories: web-security
tags: [code-obfuscation, xss, csrf]
toc: true
toc_sticky: true
 
date: 2022-05-10-22:00
last_modified_at: 2022-05-10
---

#### 개요 (web 보안 용어 설명)
* * *
- XSS (Cross Site Scripting)
    - script 를 웹서버에 올려서 방문한 클라이언트를 공격하는 기법
- CSRF (Cross Site Request Forgrey)
    - 사용자의 요청을 위조해서 서버에 요청을 보내는 기법 (웹서버 공격)
    - EX) 자동으로 좋아요 눌러지기, 자동 댓글 달기, 회원탈퇴 요청 보내기 등등…
- XSS에서 스크립트를 그냥 올리면, 공격자의 스크립트가 그대로 노출 됨.
    - 악성드 유포지의 IP 주소 또는 URL, 악성코드 이름 등등.
    - 알아보기 어렵게 난독화를 함

#### 난독화
* * *
- 인코딩을 여러 번 하거나 복잡한 함수를 사용
- 사람은 읽기 어렵지만, 웹부라우저는 어렵지 않음.

##### ASCII

- 대/소/숫/특수문자를 표시하기 위한 방법
- 127 자 이내 2의 7승 = 7bit

##### Hexa-Decimal(16진수)

- 0~F (16개)
- 2의 4승 = 4bit, 두 자리 씩 묶으면 8bit = 1byte 단위 사용
- %   %eb %03 …
- \x  \x3a \x00 …
- usc

##### Encoding

- EUC-kr : MS windows에서 사용하는 한글 인코딩 (완성형의 발전된 형태)
- UTF-8  : Web, Database, Linux 등에서 한글 인코딩 (조합형의 발전된 형태)
- Base64 
    - 64진법 (64는 2의 6승이므로 6bit, 2bit단위로 3개 씩 사용
    - 3자리가 되지 않으면 패딩을 사용(패딩은 =)
    - 대/소/숫자를 활용해서 문자를 표현
    - 전체 크기가 약 1.4배 증가

![](../../assets/images/20220518-034121.png){: .center}

**스크립트를 실행하면 악성코드 유포지를 방문할 우려가 있으니 방문하지 않고 URL 만 알아내려면?** 

- proxy 활용 URL만 확인하고 Drop
- 주소를 알아낸 후 방화벽에 방문 금지 목록(black list)에 추가.

**encoding decoding 관련 툴**

- malzilla, bluebell
