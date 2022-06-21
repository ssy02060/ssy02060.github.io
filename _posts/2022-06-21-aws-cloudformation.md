---
title:  "AWS Cloud Formation"
excerpt: "Amazon CLI 실습"

categories: cloud
tags:
  - [aws, cloud-formation]

toc: true
toc_sticky: true
 
date: 2022-06-20-00:01
last_modified_at: 2022-06-20-00:02
---
### Cloud Formation
* * *
#### Cloud Formation 이란?

![](../../assets/images/20220621-094945.png){: .center}

- 리소스를 자동으로 생성해주는 서비스
- AWS 리소스를 템플릿 파일로 작성하면, Cloud Formation 이 AWS 리소스를 생성한다
- 이때 생성된 리소스를  Stack 이라고 한다

![](../../assets/images/20220621-093448.png){: .center}

![](../../assets/images/20220621-093544.png){: .center}

![](../../assets/images/20220621-093559.png){: .center}

![](../../assets/images/20220621-093749.png){: .center}

![](../../assets/images/20220621-093800.png){: .center}

```bash
sudo  usermod -a -G apache ec2-user
sudo  chown -R ec2-user:apache  /var/www
sudo  chmod 2775  /var/www
sudo  find  /var/www  -type d -exec chmod  2775 {} \;
sudo  find  /var/www  -type f -exec chmod  0664 {} \;
```