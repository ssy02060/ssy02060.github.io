---
title:  "디지털 포렌식 - imaging"
excerpt: "디지털 포렌식 증거 획득 방법 - imaging"

categories: forensic
tags:
  - [regedit, ftk imager]

toc: true
toc_sticky: true
 
date: 2022-05-20-00:00
last_modified_at: 2022-05-20-00:01
---

#### 복제본 만들기
* * *
##### regedit으로 logical write blocking 설정
- 복제본 만들기에 앞서 증거(원본)이 훼손되는 것을 방지하기 위해 Write Block 설정을 해야함
- 실행창에서 regedit 실행
- \HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\ 경로에 StorageDevicePolies 새로 만들기
- DWORD(32비트)로 WriteProtect 만들어서 값 1로 변경

![](../../assets/images/20220520-143256.png){: .center}

![](../../assets/images/20220520-143422.png){: .center}

![](../../assets/images/20220520-143434.png){: .center}

##### FTK imager 사용해서 file imaging
- create disk
- 파일 선택
- add > e01 선택
![](../../assets/images/20220520-143759.png){: .center}

![](../../assets/images/20220520-144516.png){: .center}

![](../../assets/images/20220520-144522.png){: .center}

- start

#### Hashing
- 원본과 사본의 동일함을 입증
- 무결성 : 압수 -> 공판 전까지 입증

![](../../assets/images/20220520-152633.png){: .center}


##### hashmyfiles 활용 해쉬 실습
- 10개의 파일의 해쉬를 확인
- hash가 같은 파일 존재
- 어느게 진짜인지 판단하기 위해 실행,, 하는 방법도 좋겠지만
- HxD 를 활용해서 raw파일 확인 ㄱㄱ
- 2번과 7번 중 어느게 진짜?
- 7번 docx이고 파일 시그니쳐 PK : 진짜 파일일 확률 높음
- 2번은 .jpg 파일인데 file 시그니쳐가 PK로 되어있음 확장자만 바꾼 것.
  
![](../../assets/images/20220520-153225.png){: .center}

![](../../assets/images/20220520-153235.png){: .center}

##### Autopsy 활용 Hash Set 분석 
- new case

![](../../assets/images/20220520-162506.png){: .center}

##### Autopsy 활용 data recovery