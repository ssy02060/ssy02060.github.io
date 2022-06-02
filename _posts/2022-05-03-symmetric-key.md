---
title:  "암호학 - 대칭키"
excerpt: "암호학 개요 및 대칭키"

comments: true
categories: cryptograpy
tags: [symmetric-key]
toc: true
toc_sticky: true
 
date: 2022-05-03-22:02
last_modified_at: 2022-05-03
---

### 개요
* * *
#### 암호학이란?
- 누군가가 가로챘을 때 알아보지 못하도록 하는 것
- caesar가 사용한 최초의 암호화 방식 (치환 : 다른 글자로 바꾸는 것)
- 평문 : attack paris next friday
- 암호문 : cvvcem rctku pgzv htkfca
- 암호화 키 : shift + 2, 복호화 키 : shift - 2 --> 서로 대칭을 이룸 (대칭키)

#### 용어 정리
- 평문 (Plain Text) : 누구나 읽을 수 있는 글 (p1, p2, p3 ...)
- 암호문 (Ciper Text) : 아무도 읽을 수 없는 글 (c1, c2, c3 ...)
- 암호화 (Encryption) : 평문을 암호문으로 만드는 것
- 복호화 (Decryption) : 암호문을 평문으로 만드는 것
- 암호화 키 (Encryption Key) : 평문을 암호문으로 만드는 규칙
- 복호화 키 (Decryption Key) : 암호문을 평문으로 만드는 규칙
- 대칭 키 (Symmetric Key) : 암호화 키와 복호화 키가 서로 대칭을 이룸
= 비밀 키(Secret Key) : 사용자들이 비밀리에 보관해야 하기 때문)
= 단일 키(Single Key) : 암호화 키를 알면 대칭되는 복호화키도 알 수 있기 때문 --> 사실상 하나의 키
= 세션 키 (Session Key)

### 대칭키 (기밀성)
- 치환 (Substitution) : 문자를 다른 문자로 바꾸는 것
- 순열 (Permutation) : 알파벳 순서를 바꾸는 것
  - 평문    : attac kpari snext frida y
  - 암호문 : tactaakiprestnxy 
  - 암호화 키 : 12345 -> 31524
  - 복호화 키 : 31524 -> 12345
- 매트릭스 (Matrix)

![](../../assets/images/20220523-174138.png){: .center}

![](../../assets/images/20220523-174142.png){: .center}

- 가로 -> 세로로 읽도록. 이때 몇 X 몇 매트릭스인지 알 수 없도록.

- ROT13 : 알파벳을 반으로 나누어서 매칭을 시키는 방법
  - 단점 : 숫자와 특수문자를 변환할 수 없음, 대소문자 구분 X
- ROT47 : 대,소,숫,특수문자를 섞어서 매칭을 시키는 방법
  - 그리스, 이집트 (스키테일) --> 컴퓨터에서 사용하지 않음
  - 시저의 암호화 --> 컴퓨터에서 사용되고 있음

- 종류 : DES, 3DES, AES, SEED, HIGHT, LEA, ARIA, RC6, Blowfish 등
- 대칭키의 장점과 단점
  - 장점 : 암호화 / 복호화 속도가 빠름 (데이터 암호화에 사용)
  - 단점
    - 키 전달의 문제(직접 전달)
    - 키 개수의 문제 -> 키는 n(n - 1) / 2개 필요함

#### 대칭키의 종류
* * *
##### DES (Data Encryption Standard)
- 최초의 표준 대칭키, IBM의 Lucifer System을 기반으로 만듦
- NIST에서 표준으로 지정
- 2000년에 크래킹 됨 -> 표준에서 제외
- 64bit 키 중에서 56Bit만 암호화키이고 8bit 는 패리티비트(오류검사)로 구성
- **3DES** : DES를 3번 반복 연산 -> 임시 표준

##### AES (Advanced Encryption Standard)
- 표준 대칭키 (2001 ~ 현재)
- SPN(Substitution Permutaion Network, 치환과 전치의 반복)
- 벨기에의 Rijindael을 AES로 채택(Rijmen, Daemen이 만듦)
- 128bit, 192bit, 256bit 중에 선택해서 사용

##### SEED : 우리나라에서 만듦(은행망에서 사용)
- 소스코드 공개 (백도어가 없음을 증명)
- AES를 100% 신뢰할 수 없기 때문

##### 실습

**ECB (electronic code book)**
- hello           : 98AA900AAE9856EF
- hello world     : 98AA900AAE9856EF**567DDC800F2609DC**
- world           : 567DDC800F2609DC
- 암호화 부분을 블럭 단위로 따로따로 계산해서 붙이는 방식
- 부분부분 복호화 가능해짐

![](../../assets/images/20220523-175407.png){: .center}

![](../../assets/images/20220523-175411.png){: .center}

![](../../assets/images/20220523-175416.png){: .center}

**CBC (Cipher Block Chaining)**
- 앞의 암호문을 뒤의 평문과 XOR 연산을 한 다음, 암호화를 하는 방식
- 연쇄 효과가 생겨서 앞의 암호문을 깨지 못하면 뒤의 암호문을 깰 수 없음.

![](../../assets/images/20220523-175502.png){: .center}

![](../../assets/images/20220523-175507.png){: .center}

![](../../assets/images/20220523-175511.png){: .center}