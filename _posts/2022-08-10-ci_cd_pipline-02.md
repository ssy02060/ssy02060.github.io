---
title:  "CI/CD Pipeline with Github Actions and terraform - 02"
excerpt: "Github Actions 와 테라폼을 사용한 CI/CD 파이프라인 (환경 구성 및 각 요소 설명)"

categories: CI-CD
tags:
  - [kubernetes, terraform, eks, ecr, github-actions]

toc: true
toc_sticky: true
 
date: 2022-08-10-00:02
last_modified_at: 2022-08-10-00:03
---
### 파이프라인 구성도
* * *
![](../../assets/images/20220812-162004.png){: .center}

### Terraform 이란?
* * *
- 인프라를 코드로 관리해주기 위한 소프트웨어
- 멱등성을 유지
  - 멱등성이란? 변경된 사항을 감지하고 변경된 사항만 적용되도록 하는 특성
- AWS, GCP, Azure 등 대부분의 퍼블릭 클라우드에 호환성을 가짐
- 인프라 구성 뿐만 아니라 CI/CD 파이프라인을 구성할때도 도움을 줌

#### Terraform 설치
install-terraform.sh
```bash
#!/bin/bash
wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install terraform
```

### Github Actions 란?
* * *
- Github 에서 제공하는 CI/CD 파이프라인
- Push, Pull 등의 이벤트 발생 시 개발자가 지정한 태스크를 실행
- jobs, steps, run 등으로 구성

#### github의 actions 탭에서 확인 가능
![](../../assets/images/20220812-165336.png){: .center}

### Github Secret이란?
* * *
- 인프라를 구성하거나 배포할 때 Access key, secret key, storage access key, db host 등의 민감한 정보를 관리 할 때 사용

![](../../assets/images/20220812-165541.png){: .center}

- 아래와 같이 사용 가능

![](../../assets/images/20220812-165928.png){: .center}

### ECR
* * *
- Elastic Container Registry
- 빌드한 컨테이너를 버전별로 관리하는 레지스트리
- Docker Hub 와 동일한데 프라이빗으로 사용가능
- eks 와 사용 시 편리

![](../../assets/images/20220812-170038.png){: .center}

### EKS
* * *
- Aws 에서 제공하는 Kubernetes Cluster
- 컨테이너를 배포하고, 관리하는데 사용
- ECR 에 올라가 있는 컨테이너를 실행시키는 영역

### S3
* * *
- terraform 으로 관리 중인 인프라의 상태를 원격에 두어 어느 환경에서 Push 이벤트가 발생해도 멱등성을 유지하는데 사용
- terraform 에선 backend라는 리소스?로 사용

![](../../assets/images/20220812-170745.png){: .center}