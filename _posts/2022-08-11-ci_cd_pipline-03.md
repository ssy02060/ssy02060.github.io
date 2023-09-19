---
title:  "CI/CD Pipeline with Github Actions and terraform - 03"
excerpt: "Jenkins 와 테라폼을 사용한 CI/CD 파이프라인 (젠킨스 설치 및 github 연동)"

categories: CI-CD
tags:
  - [kubernetes, terraform, eks, ecr, jenkins]

toc: true
toc_sticky: true
 
date: 2022-08-10-00:02
last_modified_at: 2022-08-10-00:03
---
### 파이프라인 구성도
* * *
*변경 예정*
![](../../assets/images/20220812-162004.png){: .center}

### jenkins 설치
install-jenkins.sh

```bash
#!/bin/bash
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
    /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt install -y openjdk-8-jdk
sudo apt install -y jenkins
```
host ip:8080 로 접속 가능

![](../../assets/images/20220816-015730.png){: .center}

만약 설치 했는데도 젠킨스 서버가 실행이 안된다면 java 환경변수 설정 되었는지 확인
#### 환경변수 설정

[java 환경변수 설정 참고 블로그]<https://jungkeung.tistory.com/12>

### github 연동
ssh-rsa private key, public 키 생성 후 github에는 public key, jenkins 에는 private key 등록

#### 키 생성

```bash
# ssh 키 생성 (rsa)
ssh-keygen -t rsa -f ~/.ssh/jenkins-github-key
# 확인
cat ~/.ssh/jenkins-github-key
```

![](../../assets/images/20220816-020437.png){: .center}

![](../../assets/images/20220816-020617.png){: .center}

#### github Deploy key 등록
자신의 프로젝트의 github 레포지터리 > settings > Deploy keys > add new

![](../../assets/images/20220816-020012.png){: .center}


title 에 jenkins-github-key
key 에 아래 명령어의 결과 복사해서 입력
add

```bash
cat ~/.ssh/jenkins-github-key.pub
```

![](../../assets/images/20220816-020740.png){: .center}

#### jenkins에 credential 등록
credentials 클릭

![](../../assets/images/20220816-020855.png){: .center}

![](../../assets/images/20220816-020928.png){: .center}

add credentials 클릭

![](../../assets/images/20220816-020939.png){: .center}

![](../../assets/images/20220816-021019.png){: .center}

enter directly 클릭하고
아래 명령어의 결괏값 복사해서 입력
```bash
cat jenkins-github-key
```

#### 새 파이프라인 만들기
freestyle project로 생성
![](../../assets/images/20220816-021215.png){: .center}