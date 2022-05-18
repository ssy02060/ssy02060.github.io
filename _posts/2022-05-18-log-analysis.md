---
title:  "Ubuntu 환경에서의 Log 분석"
excerpt: "로그 분석"

categories: forensic
tags:
  - [ubuntu, log, reglar expression]

toc: true
toc_sticky: true
 
date: 2022-5-18
last_modified_at: 2022-5-18
---

#### 로그 분석 기본
* * *
##### 리눅스 환경에서 로그 분석

- 많은 오픈 소스
- 로그분석에 필요한 CLI 환경
- 파이프라인을 통한 분석 툴의 유기적 조합 가능
- 쉘 스크립트를 통한 자동화된 분석 가능
- 대용량 로그 분석 가능
  - 윈도에서 10GB 로그 분석 불가능
  - 리눅스에서는 10G 로그 분석 가능
- 다수의 파일에 반복적 작업 가능

##### 정규표현식(PCRE : Peal compatible Regular Expression)

- 문자를 그대로 기술하지 않으면서 문자들의 순열이나 패턴을 기술
- 한 정규펴현식이 다른 스타일의 정규표현식에 다르게 동작 할 수 있음
- 파일 검색 시 사용되는 와일드 카드 문자와 정규표현식에서의 문자는 다른 의미

- **특수문자 읽는 법**
  - "^" : 캐럿 (carrot)
  - "-" : 틸드 (tilde)
  - "*" : 아스테리스크 (asterisk)
  - "[]" : 브라킷 (bracket)
  - "." : 닷(dot), 포인트(point)
  - "@" : 앳(at)

**메타문자**

![](../../assets/images/20220518-092908.png){: .center}

- [0-9]     : 0~9 사이
- {3}       : 3번 사용
- {1,3}     : 1번~3번 사용
- A{3}      : AAA
- [a-z]     : 소문자만
- [a-z]{3}  : aaa ~ zzz
- [a-z]{1,3}: a, b, ..., aa, ..., aaa ~ zzz
- [^a-z]    : 소문자가 아닌 것
- [^']      : ' 가 아닌 것
- i..o      : i와 o사이 아무 문자 가능
- 특수문자의 Escape(특수문자가 고유한 기능을 못하기 하는 것) 처리 : \ (역슬래시)
 
##### awk (문자열 다루는 툴)

- file로부터 레코드를 선택하고 선택된 레코드에 포함된 값을 조작하거나 데이터화 하는 것
- 문자열 데이터 편집과 정규표현식을 주로 사용하는 언어
- Sed 등과 결합하면     간결하면서도 강력한 스크립트 가능
- awk 명령의 입력으로 지정된 파일로부터 데이터를 분류한 다음, 분류된 텍스트 데이터를 바탕으로 **패턴 매칭 여부를 검사**하거나 **데이터 조작 및 연산** 등의 액션을 수행하고, 그 결과를 출력

![](../../assets/images/20220518-094735.png){: .center}
출처:https://recipes4dev.tistory.com/171

![](../../assets/images/20220518-102734.png){: .center}

##### sed (데이터 처리 - 치환)

```bash
$ sed 's/old/new/g
```

![](../../assets/images/20220518-102920.png){: .center}

**사용 예**
- DNS 로그는 구분자를 [**]로 사용함,,,
- awk문은 공백을 구분자로 사용하기 때문에 DNS로그를 읽을 수 없음
- [**]를 \|(파이프) 로 치환하기

```bash
$ sed 's///g'
$ sed 's/[**]/|/g
$ sed 's/\[\*\*\]/\|/g
```

- \ 사용여부에 따른 변화 캡쳐


![](../../assets/images/20220518-103535.png){: .center}


```bash
# .7z 파일 압축 해제 툴
$ sudo apt-get install p7zip p7zip-full
$ sudo 7zr x log.7z
```

![](../../assets/images/20220518-111421.png){: .center}

```bash
# auth.log 파일 확인
$ sudo tail -10 auth.log
# 11번 칼럼만 골라서 확인
$ sudo cat auth.log | awk '{print $10}' | sort | uniq -c | sort -rn
# 의미 없는 데이터 (한 두번 나온거) 제거
$ sudo cat auth.log | awk '{print $10}' | sort | uniq -c | sort -rn | awk '$1>2'
# 저장
$ sudo cat auth.log | awk '{print $10}' | sort | uniq -c | sort -rn | awk '$1>2' > my.log
# 어떤 로그가 가장 많이, 몇 번 사용됐는지
$ sudo cat auth.log | awk '{print $5}' | sort | uniq -c | sort -rn
# 브라켓 제거
$ sudo cat auth.log | awk '{print $5}' | awk -F"[" '{print $1}' | sort | uniq -c | sort -rn 
```

![](../../assets/images/20220518-111524.png){: .center}

![](../../assets/images/20220518-113304.png){: .center}

```bash
# 6번 째 칼럼만 골라서 보려면?
$ sudo cat bee_access.log | awk '{print $6}'
# 무슨 행위가 있었는지 보려면?
$ sudo cat bee_access.log | awk '{print 
$6, $7, $8}'
# 192.168.5.1에서 몇번 접속했는지 확인하려면?
$ sudo cat bee_access.log | awk '{print $1}' | sort | uniq -c | sort -rn
```

![](../../assets/images/20220518-105623.png){: .center}

``` bash
# 공격이 언제 시작해서 언제 끝났는지?
# 시작 시간
$ sudo cat bee_access.log | grep '192.168.5.1' |awk '{print $1,$4,$5}' | head -1
# 종료 시간
$ sudo cat bee_access.log | grep '192.168.5.1' |awk '{print $1,$4,$5}' | tail -1 
```

![](../../assets/images/20220518-105345.png){: .center}

```bash
# dvwa log 확인
$ tail -10 dv_access.log
# post로 시작하는 로그만 출력
# / : 찾기
$ sudo cat dv_access.log | awk '$6~/"POST/{print $11}' | sort | uniqu -c | sort -rn

```

##### CERT (침해사고 대응팀)
- 회사 내에 존재, 보안 업체들이 전문인력으로 구성
- ahnlab, A-team, sk shielders