---
title:  "Burp Suite를 활용한 Brute Force 공격"
excerpt: "Metasploitable2의 DVWA brute-force tab"

categories: security
tags:
  - [ metasploitable2, dvwa, burp-suite]

toc: true
toc_sticky: true
 
date: 2022-5-17
last_modified_at: 2022-5-17
---
#### metasploitable2의 DVWA 접속
* * *
metasploitable2 -> DVWA 에서 brute-force tab으로 이동

internet explorer 에서 proxy 허용 설정

![picture 6](/assets/images/20220517-111925.png){: .center}  


연결에서 LAN 설정

![picture 15](/assets/images/20220517-102247.png){: .center}  


프록시 서버 아래와 같이 설정

![picture 7](/assets/images/20220517-112022.png){: .center}  


brute-force 탭에서 타겟 user, password 아무렇게나 입력


![picture 9](/assets/images/20220517-112303.png){: .center}  


burp suite proxy-> history 탭에서 방금 보낸 요청이 있는지 확인


![picture 10](/assets/images/20220517-112353.png){: .center}  


해당 히스토리를 선택 후 send to instruder


![picture 11](/assets/images/20220517-112526.png){: .center}  


비밀번호 제외하고 모두 clear


![picture 16](/assets/images/20220517-102406.png){: .center}


payload 탭으로 가서 dictionary를 load 하고 start


![picture 17](/assets/images/20220517-102639.png){: .center}  


패스워드를 찾아도 멈추지 않음

그렇다면 어떻게 구분할까?

-> lengh 가 다름


![picture 18](/assets/images/20220517-103156.png){: .center}  

log 실시간으로 확인하는 법

```bash
tail -f /var/log/apache2/access.log
```

![picture 1](/assets/images/20220517-110639.png){: .center}


brute-force 숫자로 하는 법
admin 계정 비밀번호를 1234로 변경

![picture 3](/assets/images/20220517-111239.png){: .center}  


length 가 다른 request 확인


![picture 5](/assets/images/20220517-111619.png){: .center}  
