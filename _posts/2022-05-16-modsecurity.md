---
title:  "Apache2 서버(GNU Board)에 Modsecurity (웹 방화벽)적용"
excerpt: "웹 방화벽 적용"

comments: true
categories: [web-security]
tags: [web-security, gnuboard, modsecurity, firewall]

toc: true
toc_sticky: true
 
date: 2022-5-16
last_modified_at: 2022-5-16
---
#### 웹 방화벽 설정

##### web 방화벽 오픈소스인 modsecurity 설치

```bash
$ seo@seo: sudo apt-get install -y libapache2-mod-security2
```

##### 설정 파일 수정

```bash
$ seo@seo: cd /etc/modsecurity
$ seo@seo: sudo mv modsecurity.conf-recommended modsecurity.conf
$ seo@seo: sudo vi modsecurity.conf
```

**vi에서 line number 보는 법 esc 후 ":set nu"**

**vi에서 검색하는 법 esc 후 "/'찾고자 하는 내용'"**

![](../../assets/images/20220518-021020.png){: .center}

- 7번 line SecRuleEngine DetectionOnly -> On

![](../../assets/images/20220518-021214.png){: .center}

- 187번 line SecAuditLogParts ABCDEFHIJZ -> ABCEFHJKZ

![](../../assets/images/20220518-021624.png){: .center}

- restart apache2

```bash
$ seo@seo: sudo systemctl restart apache2
```

##### Modsecurity Rules 다운로드

- snort 와 동일하게 Pattern Matching으로 악성코드 및 접근을 차단
- rules 다운로드

```bash
$ seo@seo: cd ~
# rule set 설치
$ seo@seo: sudo wget https://github.com/coreruleset/coreruleset/archive/v3.3.0.tar.gz
# 압축 풀기 x : extract, v : 자세한 정보 표시, f : file 이름 지정
$ seo@seo: sudo tar xvf v3.3.0.tar.gz
$ seo@seo: sudo mv coreruleset-3.3.0/ /etc/apache2/modsecurity-crs/
$ seo@seo: cd /etc/apache2/modsecurity-crs
$ seo@seo: sudo mv crs-setup.conf.example crs-setup.conf
$ seo@seo: sudo vi /etc/apache2/mods-enabled/security2.conf
```

- 아래와 같이 수정

![](../../assets/images/20220518-025203.png){: .center}

```bash
$ seo@seo: sudo apache2ctl -t
$ seo@seo: sudo systemctl restart apache2
```

![](../../assets/images/20220518-025304.png){: .center}

- 정상 동작

##### 웹 방화벽 정상 동작하는지 test

- gnuboard에서 sql injection 공격 시도하기

![](../../assets/images/20220518-025454.png){: .center}


![](../../assets/images/20220518-031522.png){: .center}

- forbidden 뜨면 정상 modsecurity 가 정상 동작하는 중