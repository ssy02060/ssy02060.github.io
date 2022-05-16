---
title:  "Apache2 서버(GNU Board) 생성 후 Modsecurity 웹 방화벽 적용 "
excerpt: "웹 방화벽 설정"

categories:
  - 보안...🖥
tags:
  - [apache, security, gnuboard, modsecurity, firewall]

toc: true
toc_sticky: true
 
date: 2022-5-16
last_modified_at: 2022-5-16
---

# Apache2 서버(GNU Board) 생성 후 Modsecurity 웹 방화벽 적용 
#### GNU Board 웹 서버 생성 
* * *
```bash
# apache2 설치
$ seo@seo: sudo apt install -y apache2
# vi editor 설치
$ seo@seo: sudo apt install -y vim
# php 언어설치
$ seo@seo: sudo apt install -y php php-mysql php-common php-gd php-fpm php-xml php-json php-curl
# git 설치
$ seo@seo: sudo apt install -y git
# "/var/www/html" 경로에 GNU Board project clone 
$ seo@seo: cd /var/www/html
$ seo@/var/www/html: sudo git clone https://github.com/gnuboard/gnuboard5
```
#### MySQL 데이터베이스, User 세팅
* * *
```sql
mysql > create user bts;
mysql > grant all privileages on bts
```
![picture 10](../assets/images/20220516-143455.png)  
