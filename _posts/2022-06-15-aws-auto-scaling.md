---
title:  "AWS Auto Scaling - EC2"
excerpt: "Amazon CLI 실습"

categories: cloud
tags:
  - [aws, ec2, amazon-linux, auto-scaling]

toc: true
toc_sticky: true
 
date: 2022-06-15-00:01
last_modified_at: 2022-06-15-00:02
---

### Auto Scaling 이란?
* * *
- 스케일링 : 클라이언트가 집중되는 경우 서버에 부하 발생을 완화시키기 위해서 서버의 개수를 늘리거나 스펙을 변경하는 방법
- Auto Scaling : 자동으로 서버의 규모 또는 스펙을 조정하는 것 (규칙 : cpu 사용량 등을 기준으로 함)
- Scale Out : 서버의 개수를 늘리는 것 (양적 증대)
- Scale In : 서버의 개수를 줄이는 것 (양적 감소)
- Scale Up : 서버의 성능을 올리는 것 (질적 증대)
- Scale Down : 서버의 성능을 낮추는 것 (질적 감소)

#### EC2 요금제
- 온디맨드 : 내가 필요할 때 필요한 기간 동안 EC2를 사용하는 것
- 스팟 인스턴스 : 내가 제시한 금액이 시세보다 높으면 실행, 낮으면 중지
  - 급하지 않은 일을 처리하는 서버가 필요할 때 사용
  - ex) 필름으로 보관하던 신문을 PDF로 변환하는 작업
- 에약 인스턴스 : 약정 요금 (일정 기간 동안 사용하기로 계약, 장기간 사용할 경우 적합)

#### Auto Scaling에 적합한 요금제?
- 스팟 인스턴스 : 시세보다 낮은 금액으로 스팟 인스턴스를 사용하거나 시세가 올라가는 경우, 스팟 인스턴스로 해놓은 EC2 들이 중지 상태로 변경 됨
  - 오토스케일링으로 EC2를 늘려야 하는 시점에 중지되면 안되기 때문
- **온디맨드** : 사용량에 따라 증가하는 요금제가 적합

- 오토 스케일링이 필요한 경우
  - 공연 티켓 구입
  - 수강신청
  - 핫타임 딜
  - 인플루언서들이 컨텐츠 업로드 했을 때

### 실습
* * *
- Load Balancer
  - Client 의 요청을 분산해주는 네트워크 장치
  - Port 번호를 기준으로 분산하면 --> L4 Switch, Network Load Balancer(NLB)
  - Application의 일부를 기준으로 분산하면 -> L7 Switch, Application Load Balancer(ALB)

#### 스냅 샷 및 개인 AMI 생성

![](../../assets/images/20220615-135605.png){: .center}

![](../../assets/images/20220615-135707.png){: .center}

![](../../assets/images/20220615-135734.png){: .center}

![](../../assets/images/20220615-133555.png){: .center}

![](../../assets/images/20220615-135951.png){: .center}

#### 시작 구성
- 오토 스케일링을 하기 위한 준비 작업
- 어떤 이미지를 사용해서 서버를 활성화 시킬 것인지

![](../../assets/images/20220615-140018.png){: .center}

![](../../assets/images/20220615-143520.png){: .center}

![](../../assets/images/20220615-143622.png){: .center}

![](../../assets/images/20220615-143701.png){: .center}

![](../../assets/images/20220615-143737.png){: .center}

#### Auto Scaling Group
- 스케일링 규칙 등을 결정하는 방법
1. 시작 템플릿 또는 구성 선택 (시작 구성 선택)

![](../../assets/images/20220615-144118.png){: .center}

2. 인스턴스 시작 옵션 선택

![](../../assets/images/20220615-144150.png){: .center}

3. 고급 옵션 구성 (로드 밸런서 없음)

![](../../assets/images/20220615-144309.png){: .center}

4. 그룹 크기 및 크기 조정 정책 구성
- 용량 : 서버 개수
- 대상 추적 크기 조정 정책 : CPU, network 입출력 상태에 따라 동적으로 용량 증가

![](../../assets/images/20220615-145046.png){: .center}

![](../../assets/images/20220615-145200.png){: .center}

5. 알림 추가
- Auto Scaling에 대한 알람 설정

![](../../assets/images/20220615-145247.png){: .center}

6. 태그 추가

![](../../assets/images/20220615-145407.png){: .center}

