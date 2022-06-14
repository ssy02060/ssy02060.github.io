---
title:  "AWS Amazon Linux - EC2"
excerpt: "Amazon Linux 실습"

categories: cloud
tags:
  - [aws, ec2, amazon-linux, apache2]

toc: true
toc_sticky: true
 
date: 2022-06-13-00:00
last_modified_at: 2022-06-11-00:01
---
### Amazon Linux 특징
- AWS 에서 가장 많이 사용(AWS  환경에 최적화 시켜서 가볍게 동작)
- yum으로 자동 설치 (Rad Hat의 old version과 비슷)
- Database가 Maria DB를 기본으로 설치함
- Amazon
  
#### apache2 설치 및 설정
```bash
$ sudo yum update -y
$ sudo yum install httpd
# lamp : linux, apache, mysql, php (Web server 4종 세트)
$ sudo amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2
$ sudo  systemctl  start  httpd
$ sudo  systemctl  enable  httpd
# 아파치 그룹(-G apache)에 ec2-user를 추가(-a : add)함
$ sudo  usermod -a -G apache ec2-user
# 웹서버 루트 디렉토리(/var/www)의 소유권을 apache그룹의 ec2-user로 변경함
$ sudo  chown -R ec2-user:apache  /var/www
# 웹서버 루트 디렉토리의 권한을 2775로 변경(other는 read와 executable만 가능)
$ sudo  chmod 2775  /var/www
# 웹서버 루트 디렉토리의 하위 디렉토리가 2775로 되어있는지 확인
$ sudo  find  /var/www  -type d -exec chmod  2775 {} \;
# 웹서버 루트 디렉토리의 하위 디렉토리가 0664로 되어있는지 확인
$ sudo  find  /var/www  -type f -exec chmod  0664 {} \;
# 웹브라우저를 열고 접속해서 잘 실행되는지 확인해봅니다. 
$ echo "<?php phpinfo(); ?>" > /var/www/html/phpinfo.php
```

#### gnuboard 설정
```bash
$ sudo yum install -y git
$ cd /var/www/html
$ git clone https://github.com/gnuboard/gnuboard5
$ cd  gnuboard5
$ sudo mkdir data
# 777인 이유는 amazon linux 에서는 apache라는 그룹을 만들고 그 그룹에 user를 추가했기 때문
$ sudo chmod 777 data 
$ sudo  yum install  php-gd  php-common  php-xml  php-json  php-fpm  php-curl php
$ sudo systemctl restart httpd
```