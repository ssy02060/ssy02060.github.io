---
title:  "AWS Load Balancer - LB"
excerpt: "Amazon 로드 밸런서 실습"

categories: cloud
tags:
  - [aws, ec2, load-balancer]

toc: true
toc_sticky: true
 
date: 2022-06-17-00:01
last_modified_at: 2022-06-17-00:02
---
### Load Balancer
* * *
#### Load Balancer 란?
- 서버의 부하 분산을 위한 기기
- L4, L7 이 있음
- L4 : Network Load Balancer (NLB)
- L7 : Application Load Balancer (ALB)

##### Network Load Balancer

![](../../assets/images/20220620-171638.png){: .center}

- TCP / IP 헤더를 보고 적절한 패킷으로 전송
- IP + 포트 번호를 보고 스위칭
- SSL 적용이 인프라 단에서 불가능 하므로 Application 에서 따로 적용해야 함

##### Application Load Balancer

![](../../assets/images/20220620-172102.png){: .center}

- HTTP / HTTPS 헤더를 보고 적절한 패킷으로 전송
- IP + Port + 패킷 내용을 보고 스위칭
- SSL 적용 가능

- 
- How? 
- 여러 클라이언트가 동시에 접속 했을때 다수의 서버에 돌아가면서 접속하도록 함.
  - Round Robin 기법 : 1 번 서버 -> 2번 서버 -> 3번 서버 -> 1번 서버 ...
  - Weighted Round Robin 기법 : 각 서버에 다른 가중치를 줘서 성는 좋은 서버에 더 많은 요청이 가도록
  - Least Connection 기법 : 연결이 가장 적은 서버에 요청을 분배
  - Weighted Least Connection 기법 : Least Connection + 서버 성능 고려
  - Fastest Response Time : 응답 시간이 가장 적은 서버에 분배
