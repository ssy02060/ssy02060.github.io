---
title:  "CI/CD Pipeline with Github Actions and terraform - 01"
excerpt: "Github Actions 와 테라폼을 사용한 CI/CD 파이프라인"

categories: CI/CD
tags:
  - [kubernetes, terraform, eks, ecr, github-actions]

toc: true
toc_sticky: true
 
date: 2022-08-09-00:02
last_modified_at: 2022-08-09-00:03
---

### CI/CD 파이프라인이란?
* * *
- 지속적 통합, 지속적 배포
- 소프트웨어의 새로운 버전을 신뢰도를 유지한 채 짧은 사이클로 배포하는 것을 의미한다.
- DevOps와 SRE 방식을 통해 효과적으로 제공하는 데 초점을 맞춤.
#### CI/CD 파이프라인의 요소
- **build**     : 애플리케이션을 컴파일하는 단계, (현 프로젝트로 예로 들면 container 이미지를 빌드하는 단계)
- test          : 코드를 테스트하는 단계, 코드의 신뢰성을 높이기 위해 원하는 결괏값이 나오는지 확인하는 단계
- **release**   : 애플리케이션을 리포지토리에 제공하는 단계 (현 프로젝트로 예로 들면 container 이미지를 registry에 올리는 단계)
- **deploy**    : 애플리케이션을 배포하는 단계 (현 프로젝트로 예로 들면 registry에서 쿠버네티스 클러스터에 배포하는 단계)
- validation & compliance : Clair와 같은 이미지 보안 스캔 툴을 사용하여 알려진 취약점(CVE)과 비교하는 방법으로 이미지의 품질을 보장할 수 있습니다.

*이 블로그에선 굵게 처리된 부분만 다룹니다.*

#### CI/CD에 대한 내 방식대로 설명
- 위의 요소들을 수행하는 것은 명령어들을 순서대로 입력하는 것이다.
- 이때 한 두개의 컨테이너, 서비스일때는 위 요소들을 수행하는 것이 쉽지만
- Microservice Architecture 의 경우 각 기능마다 컨테이너로 서비스를 제공하게 되는데
- 이때 입력해야 하는 명령어는 수백, 수천이 될것이다.
- 이를 code, Commit, push 이벤트 등을 감지하여 자동으로 수행할 수 있도록 한 것이 CI/CD 파이프라인이다.
- 예시로는 Jenkins, Github Acitons가 있다.
- 이 포스팅에선 **Github Actions**를 사용할 예정이다.

#### CI/CD 파이프라인 예시
- 아래 이미지는 최근 진행하고 있는 프로젝트의 파이프라인 예시이다.

![](../../assets/images/20220812-162004.png){: .center}

- AWS public 클라우드 환경에서의 Microservice 아키텍처로 영화 추천 사이트를 개발하는 프로젝트이고
- Infra에 대한 관리는 Terraform
- CI/CD 파이프라인은 Github Actions (Jenkins로 전환할수도 있음)
- 

