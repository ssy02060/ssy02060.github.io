---
title:  "JAVA 스터디 DAY 1"
excerpt: "프로그래밍 언어 교양"

categories: development
tags:
  - [java, 객체지향언어, OOP]

toc: true
toc_sticky: true
 
date: 2022-06-17-00:01
last_modified_at: 2022-06-17-00:02
---
### 프로그래밍 언어 교양
* * *
#### 프로그래밍 언어 역사
![](../../assets/images/20220617-181122.png){: .center}

- B -> C -> C++ -> JAVA -> C#
- C언어부터 high level language 라고 함
- high 와 low 의 차이
  - 컴퓨터와 가까울수록 Low
  - 사람에 가까울수록 High 
  - 지금 C언어는 Low 한 언어로 평가 받음

#### 프로그래밍 정의
- 컴퓨터, 기계에게 무언가를 시키는 행위
- 어떻게 시키는가?
- 프로그래밍 언어를 사용해서
- 그럼 프로그래밍 언어는 무엇인가?
- 기계어와 자연어의 중간 역할
- 기계가 어떻게 알아먹나?
- 컴파일러라는 번역기가 있음 : 프로그래밍언어 -> 어셈블리어 -> 기계어


- 자연어

```java
10이랑 20 더한 값은 ?
```

- 프로그래밍 언어 (자바)

```java
a = 10
b = 20
c = a+b
```

- 어셈블리어

![](../../assets/images/20220617-225915.png){: .center}

- 기계어

![](../../assets/images/20220617-225806.png){: .center}

![](../../assets/images/20220617-230517.png){: .center}

### JAVA 와 C언어의 차이
- 가장 큰 차이는 절차지향언어 VS 객체지향언어
- 컴파일러 과정의 차이

#### C 컴파일 과정

![](../../assets/images/20220618-012806.png){: .center}

#### JAVA 의 컴파일 과정

![](../../assets/images/20220618-012748.png){: .center}

#### JAVA 가 사랑받을 수 있었던 이유

- 높은 이식성, 높은 생산성 + 유지보수성, 보안

##### JAVA의 높은 이식성
![](../../assets/images/20220618-012835.png){: .center}

- JAVA 가 실행 되려면 실행될 기기에 무조건 JVM (Java Virtual Machine) 이 있어야 함
- JVM이란?
  - JAVA 프로그램이 실행될 수 있도록 도와주는 아이
  - 컴파일의 결과물인 .class 파일을 실행시켜 줌
  - (+ 가비지 컬렉터라는 아이로 메모리 관리해 줌)
  - 어떤 디바이스에서든 JVM만 있으면 자바로 작성된 프로그램을 실행 시킬 수 있음.


#### 절차지향언어란?
- 프로그램의 흐름이 코드 작성 흐름과 동일
- 예를들어 여러 개의 함수가 있을 때 코드 상 위에 있는 함수에서 아래에 있는 함수를 호출하지 못함
- 장점 : 성능 좋음, 단순한 구조일 때 코드 생산성이 높은 편임
- 단점 : 코드 양이 늘어나거나 복잡한 로직인 경우 코드 생산성이 낮아짐

#### 객체지향언어란?
- 앞으로 배워나갈거임.
- 객체라는 개념을 가져와 현실 세계의 개념을 표현하기 위해 노력했음 -> 추상화

##### 객체(Object)란?
- 구조체 + 함수
- 구조체 : 변수들의 집합
- 함수 : 어떤 행위를 하는 코드의 집합