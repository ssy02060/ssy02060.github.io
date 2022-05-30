---
title:  "암호학 - 공개키"
excerpt: "공개키"

comments: true
categories: cryptograpy
tags: [public-key]
toc: true
toc_sticky: true
 
date: 2022-05-03-22:03
last_modified_at: 2022-05-03
---

### 공개키 (기밀성, 인증)
* * *
#### Diffie-Hellman : 최초의 공개키 (1976년)
- 개인키를 이용해 공개키를 생성하고, 상대방에게 공개키를 제공
- 상대방의 공개키와 자신의 개인키를 연산해서 세션키를 만들어 냄
- 상대방과 나만 가지는 키이므로 다른 사람은 가질 수 없음(비밀성 보장)
- 세션 키를 교환할 필요가 없어져 키 교환 문제 해결.
- 이산대수의 어려움에 근거한 방식
- 누구나 개인키와 공개키 한쌍을 가짐 (개인키로 공개키를 생성)
- 개인키(Private Key) 는 자기 혼자만 안전하게 보관하고 사용
- 공개키(Public Key) 는 거래 상대에게 제공함

**대칭키 방식의 가장 큰 문제인 키 전달의 문제를 해결**
- 공개키를 서로 교환한 후에, 자신의 개인키와 상대의 공개키를 연산해서 대칭키(세션키)를 생성해 냄
- 데이터를 대칭키(세션키)로 암호화해서 보내면, 상대방도 복호화 가능
- 대칭키(세션키)를 교환하는 문제 해결

![](../../assets/images/20220523-180032.png){: .center}

##### Diffie-Hellman 방식의 취약점 (Man in the Middle Attack)

- Mallory는 자신의 공개키를 속여서 양쪽에 줌
- Alice는 Bob의 공개키인 줄 알고 세션키를 생성하고 해당 세션키로 암호화
- Mallory 는 Alice의 평문을 Bob의 공개키로 대칭키를 이용해 암호화해 Bob에게 전달
- 이때 평문을 Mallory 마음대로 바꿀 수 있음

![](../../assets/images/20220523-180109.png){: .center}

#### RSA (사실상의 표준 공개키) : Rivest, Shamir, Adleman
- 누구나 개인키와 공개키 한 쌍을 가짐
- 하나의 키로 암호화하면, 쌍이 되는 다른 키로만 복호화 됨(양방향 암호화)
- 소인수 분해의 어려움에 근거

**기밀성**
- 송신자가 "수신자의 공개키" 로 암호화해서 보냄 -> 수신자는 자신의 개인키로 복호화

**인증**
- 송신자가 "자신의 개인키" 로 암호화해서 보냄 -> 수신자는 송신자의 공개키로 복호화

##### Boss 와 Killer 의 상황(예시)

1. 기밀성
- Q) Boss가 Killer 에게 'Kill Johnny' 라는 메시지를 비밀리에 보내려고 한다.
- Boss 는 무슨 키로 암호화 해야 할까??
- : Killer의 공개키
- Killer는 자신의 개인키로 복호화
- Killer는 Johnny를 죽이지 않았다. Why? Boss가 시켰다는걸 인증할 수 없음

2. 인증
- Q) Boss 가 자신의 개인키를 사용해서 'Kill Johnny' 를 암호화 한다음 Killer에게 보냈다.
- Killer는 Boss의 공개키로 복호화 했음
--> Boss의 공개키로 복호화 된다는 것은 Boss가 개인키를 사용했다는 인증
- Killer는 Johnny를 죽이지 못했다. Why? Johnny도 Boss의 공개키로 해당 메시지를 복호화 했기 때문

3. 기밀성 + 인증
- Q) Boss가 메시지를 자신의 개인키로 암호화한 뒤(인증) Killer의 공개키로 암호화(기밀성)해서 Killer 에게 보냄
- Killer 는 자신의 개인키로 복호화하고, Boss의 공개키로 복호화 하면 메시지를 볼 수 있음
- Killer 가 죽이러 가지 못한 이유는? 메시지가 변조되지 않았다는 증거가 없어서(그리고, 시간이 너무 오래걸려서)
- cf. 공개키 방식은 연산의 복잡성 때문에 시간이 많이 걸림 -> 데이터는 대칭키로 암호화 해야 함.
- 공개키는 대칭키를 전달하는 용도로만 사용해야 함 => TLS(https를 사용할 때, 서버의 공개키를 받아옴, 대칭키를 공개키로 암호화)