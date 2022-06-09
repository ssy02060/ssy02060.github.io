---
title:  "AWS Storage Service S3"
excerpt: "S3 실습"

categories: cloud
tags:
  - [aws, s3]

toc: true
toc_sticky: true
 
date: 2022-06-09-00:00
last_modified_at: 2022-06-09-00:01
---

### AWS Store Service - S3 실습
* * *
#### 버킷 만들기
- 일반 구성
  - 버킷 이름 : 고유 ID 로 입력
  - 리전 : 아시아 태평앙(서울)
- 객체 소유권
  - ACL 활성화 됨
  - 객체 소육권 : 객체 라이터
- 이 버킷의 퍼블릭 액세스 차단 설정
  - 파일을 인터넷을 통해 불특정 다수에게 공개하려면? 버킷도 퍼블릭, 객체(파일)도 퍼블릭 (AND)
  - 둘 중 하나라도 퍼블릭으로 설정하지 않으면 공유 불가

![](../../assets/images/20220609-130927.png){: .center}

#### 파일 올리고 권한 주기
- 업로드 클릭

![](../../assets/images/20220609-133501.png){: .center}

- 업로드하고 업로드한 파일 클릭 -> 권한 -> 편집
  
![](../../assets/images/20220609-133057.png){: .center}

- 권한이 변경되면 url로 접근이 가능해짐
- url 규칙
  - [버킷이름].s3.[region].amazonaws.com/[파일명.확장자]

![](../../assets/images/20220609-133126.png){: .center}

#### 정적 웹사이트 업로드하기
- 동일하게 업로드
- html 파일 작성해서 권한 설정 후 업로드

```html
<html>
<head>
<title>ddomi, i miss you </title>
</head>
<body>
<center>
    <img src="https://[버킷이름].s3.ap-northeast-2].amazonaws.com/[파일명]">
    <br>
    <h1>ddomi, i miss you</h1>
    <br>
    <h6>ddomi, where are you,,,</h6>
</center>
</body>
</html>
```
![](../../assets/images/20220609-141207.png){: .center}

![](../../assets/images/20220609-141346.png){: .center}

- 버킷에 업로드한 파일을 수정할 수 없음 -> 객체 스토리지
#### 스토리지의 종류
- 파일 스토리지
  - 일상적인 컴퓨터의 파일 시스템과 동일
  - NAS 도 이와 동일
  - 직접 수정 가능
- 블록 스토리지
  - 블록 단위로 분산해서 저장
  - SAN과 비슷
  - SQL과 비슷
  - ex) drop box, one drive 등
- 객체 스토리지
  - 파일을 업로드하면 수정할 수 없음
  - 지우고 다시 업로드 해야 함

![](../../assets/images/20220609-142245.png){: .center}

[출처] Dell EMC

#### 한글 인코딩 추가
- meta 태그에 charset 추가

```html
<html>
<head>
<title>ddomi, i miss you </title>
<meta charset="utf-8">
</head>
<body>
<center>
    <img src="https://[버킷 이름].s3.ap-northeast-2.amazonaws.com/[파일명]">
    <br>
    <h1>또미 보고싶어,,</h1>
    <br>
    <h6>또미 잘하구 왕</h6>
</center>
</body>
</html>
```

![](../../assets/images/20220609-143223.png){: .center}

#### 정적 웹사이트 호스팅
- 버킷의 속성 탭 하단에 정적 웹사이트 호스팅
- 편집
- 활성화, 인덱스 문서명 입력

![](../../assets/images/20220609-143817.png){: .center}

![](../../assets/images/20220609-143859.png){: .center}

- 정적 웹사이트 업로드한 것과 차이점
- 파일명이 보이지 않음

#### S3를 활용하는 방법
- 무거운 컨텐츠를 보관 : mp4, 이미지 등
- 사용자 업로드한 파일을 저장 (게시판 등에서)
- 자주 사용하지 않는 파일 보관
- 영원히 보관해야 할 파일 ex) 어릴적 사진, 백업 파일 등

#### S3 요금 정책
- 아래 이미지 참고

![](../../assets/images/20220609-152205.png){: .center}

<https://aws.amazon.com/ko/s3/pricing/?nc=sn&loc=4>

- Standard : (꺼내는 비용 ↓ , 보관 비용 ↑)
- Glacier : 빙하 (꺼내는 비용 ↑ 보관 비용 ↓ )

- 수명주기(Lifecycle)
  - 설정을 해놓고 일정 시간이 지나면 다른 클래스로 이동하게 할 수 있음

- S3 Inteligent Tiering
  - 사용자의 엑세스 패턴, 주기에 따라 자동으로 클래스를 옮겨주는 요금제
  - 30일 동안 사용하지 않으면 Frequent Access -> Infrequent Access
  - 90일 동안 사용하지 않으면 Infrequent Access -> Archive instant Access
  - 자동으로 이동하는 방식
  - 접근이 발생하면 다시 Frequent Access Tier로 이동함
  - 접근에 대한 예측이 어려운 경우 : 담당자가 초보인 경우, 처음 다뤄보는 데이터

#### Region & Zone
- Region : 특정 지역에서 클라우드 서비스를 하기 위한 단위
  - ex) 서울 리전(ap-northeast-2), 도쿄 리전(ap-northeast-1), 오하이오 리전, 버지니아 리전(aws의 첫번째 리전)
- Zone (가상의 데이터 센터) : 데이터 센터 단위
  - ex) 서울 리전의 zone 이름 : ap-northeast-2a, ap-northeast-2b, ap-northeast-2c, ap-northeast-2d

![](../../assets/images/20220609-161135.png){: .center}

##### Cross Region Replication(리전 간의 복제, CRR)
- 다른 리전으로 데이터를 복사하는 것
- **특별한 경우가 아니면 할 필요 없음**
- 대부분의 서비스는 리전 내에서 충분히 가능

##### Same Region Replication(리전 내 복제, SRR)
- 같은 리전의 다른 Zone으로 데이터를 복사하는 것
- **자동으로 지원 됨** ex) S3 Standard, Glacier 등 대부분의 서비스는 리전 내 다른 Zone에 자동으로 백업 됨
- 항상 동작함
- 하나의 Zone에 문제가 생기더라도 다른 두 개의 Zone에 저장된 데이터로 복구 가능
- 최대 2개의 Zone이 고장 나더라도 데이터는 보존 됨

#### 실습 수명주기 정책 적용
- 수명 주기 정책을 정용해서 2주가 지나면 Glacier에 저장하도록 설정하기

![](../../assets/images/20220609-165525.png){: .center}

![](../../assets/images/20220609-165559.png){: .center}

https://aws.amazon.com/ko/s3/pricing/?nc=sn&loc=4

링크에 요금 정책 나열 되어 있음

standard(frequent(자주)) -> infrequent(덜 자주) access -> glacier(빙하) 순으로 월별 저장 비용 감소

why?
standard(frequent) 는 hdd, ssd 같은 빠른 스토리지에 저장
infrequent, glacier는 테이프 스토리지 같은 느린 스토리지에 저장

hdd,ssd 는 보관 비용은 많이 들지만 금방금방 읽을 수 있음
테이프 스토리지는 보관 비용은 적게 들지만 읽어들일때 오래 걸림

요금제를 데이터 접근 빈도에 따라 딱 맞게 설정하면 가장 좋지만
예측이 불가능한 경우가 있음 ex) 관리자가 초보, 처음 다루는 데이터

이때 사용하는 요금제가 intelligent(똑똑한) tiering


intelligent(똑똑한) tiering은 접근 빈도에 따라 클래스를 바꿈
- 30일 동안 접근 없으면 infrequent(덜 자주) access 로 변경
- 90일 동안 접근 없으면 glacier(빙하)로 변경
