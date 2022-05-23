---
title:  "암호학 - 해쉬 함수"
excerpt: "해쉬함수"

comments: true
categories: cryptograpy
tags: [hash-fucntion]
toc: true
toc_sticky: true
 
date: 2022-05-03-22:04
last_modified_at: 2022-05-03
---

#### 해쉬 함수 (무결성)
* * *
- 의미 없는 값, 길이가 일정, 원문이 같으면 해쉬값도 같음 (무결성)
- 원문을 해쉬 함수에 넣으면 해쉬값(Hash Code)이 출력됨
- 해쉬 함수의 특징
 1. 고정길이 출력 : 원문의 길이(Text, 동영상, 사진)와 관계없이 해쉬값은 일정한 길이로 출력
  ex) MD5(128bit), SHA-1(160bit), SHA-256(256bit)
 2. 일방향 함수 : 해쉬 함수와 해쉬 값을 알아도 원문 복구는 불가능

##### 실습) Kali Linux에서 해시함수 사용하기
- md5, sha1, sha256으로 파일 변환 해보기
- md5       : 16진수 32자리 -> 128bit
- sha-1     : 16진수 40자리 -> 160bit
- sha-256   : 16진수 64자리 -> 256bit

![](../../assets/images/20220523-182735.png){: .center}

- cf. 블록체인의 원리는 해쉬값을 모든 채굴기에게 공유 -> 함부로 바꿀 수 없다