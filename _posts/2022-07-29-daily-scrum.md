---
title:  "Daily 회의 기록 7/29"
excerpt: "Daily 회의 기록 7/29"

categories: last-project
tags:
  - [daily-scrum, last-project]

toc: true
toc_sticky: true
 
date: 2022-07-29-00:02
last_modified_at: 2022-07-29-00:03
---
### 오늘 할 일
- 오전
  - 어떤 서비스 할지 좀 더 구체적으로 얘기하기
  - 역할까지 나누기
- 오후
  - 역할 나눠졌으니까 각각의 서비스들 초기 세팅
  - 

### 팀원
- 편한 언어
- 웹서버 기준
- 서상윤 - javascript(react, express)
- 김주만 - python (html, css 알아볼 수 있을 정도)
- 김유진 - python 
- 백예슬 - python
- 최세리 - python (php, html 가능)
- 권영호 - python, javascript

### Destiny 서비스 정리
영화 추천 사이트 - 데스티니
- 추천 알고리즘
- 선호 태그 선택
- 평점, 리뷰 시스템
- 보관함
- 랭킹
- 로그인, 회원가입
- 프론트 엔드
- 예고편 스트리밍
- 영화 정보 수집(api) 
- 검색 기능 
------------------------
Test...
CI/CD
### 역할 나누기 (다음 스프린트)
- 서상윤 : 프론트 엔트 및 환경 세팅(terraform, git action ...) hello.txt 만들어서 add, commit까지 한번 해보세용
- 최세리 : 로그인, 회원가입 (소셜로그인)                  -> login 
- 백예슬 : 검색 기능                                    -> search
- 김유진 : 평점, 리뷰 시스템                            -> review
- 김주만 : 영화정보 수집(api)                           -> movie-data
- 권영호 : 선호 태그 선택                               -> tag

### 마이크로서비스 통신 RestAPI
API Application Programing Interface
API : 요청에 대한 답을 어떤 방식으로 할지 약속
- REST, GraphQL
- REST API : 경로(Path)를 기반으로 
- 각각의 마이크로서비스들은 경로, 도메인을 기반으로 통신한다

### CI / CD
- 웹서버가 로컬에서 돌리는 경우가 있고 -> 개발자의 컴퓨터
- 클라우드 환경에서 돌리는 경우가 있다 -> 서비스하는 중
- 로컬의 코드 변화를 자동으로 클라우드 환경에 배포하는 과정

### git 사용법
1. git clone 명령어
- repository -> 소스코드 저장소
- repository 에 있는 소스코드를 다운 받는 명령어
2. git add
- 커밋할 파일(변경사항을)을 스테이지에 올리는 명령어
3. git commit
- 현재 상태를 기록, 저장하는 명령어
4. git push
- 로컬의 변경사항들을 원격서버(repository)에 업로드하는 명령어
5. git pull
- 원격의 변경사항들을 로컬에 다운로드 하는 명령어

### nodejs express 서버 만들기
- java랑 javascript랑 완전히 다른 언어다
- javascript 브라우저에서만 쓰는 언어였는데
- 프론트 <-> 서버(백엔드)랑 언어가 달라서 불편해서

- nodejs 라는 개발환경을 만들어서
- 웹브라우저 이외의 작업공간에서 javascript 코드가 실행되게끔 만들었어요
- javascript를 서버로 돌릴 수 있게 됐음

```bash
cd ~/Documents/projects/destiny/frontend
npm init -y
npm install --save express
# --save-dev 옵션은 개발 단계에 설치되도록 하는 옵션
npm install --save-dev nodemon
```

디렉토리 구조
![](../../assets/images/20220729-180208.png){: .center}

package.json 파일
```json
{
  "name": "frontend",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node ./src/index.js",
    "start:dev": "nodemon ./src/index.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.18.1"
  },
  "devDependencies": {
    "nodemon": "^2.0.19"
  }
}
```

index.js 파일
```javascript
var express = require('express'); // 설치한 express module을 불러와서 변수(express)에 담습니다.
var app = express(); //express를 실행하여 app object를 초기화 합니다.

app.get('/', function (req, res) { // '/' 위치에 'get'요청을 받는 경우,
    res.send('Hello fffff!'); // "Hello World!"를 보냅니다.
});

var port = 3000; // 사용할 포트 번호를 port 변수에 넣습니다. 
app.listen(port, function () { // port변수를 이용하여 3000번 포트에 node.js 서버를 연결합니다.
    console.log('server on! http://localhost:' + port); //서버가 실행되면 콘솔창에 표시될 메세지입니다.
});
```


Dockerfile-dev 파일
```dockerfile
FROM node:10.15.2-alpine

WORKDIR /usr/src/app
COPY package*.json ./
COPY ./src ./src

CMD npm config set cache-min 9999999 && \
    npm install && \
    npm run start:dev
```

### python 웹 프레임워크 django, **flask**
- django, flask
- django 는 좀 무겁고, 기본으로 깔아주는 패키지들이 많다,,,
- flask 가볍고 필요한 것만 설치해서 쓸 수 있다.


### Flask 환경세팅
- pip install --user -r requirements.txt

requirements.txt
```txt
Flask==2.1.0
```

app.py
```python
from flask import Flask

app = Flask("api_test")

@app.route('/')
def hello():
    return 'Hello'

if __name__ == '__main__':
    app.run('0.0.0.0', port=5000, debug=True)
```

- 로컬환경에서 컨테이너 없이 서버를 돌릴 때

```bash
pip install --user -r requirements.txt
python3 app.py
```


- 도커 컨테이너에서 서버를 실행

```dockerfile
FROM python:3.8.5-alpine

WORKDIR /test
COPY . .
RUN /usr/local/bin/python -m pip install --upgrade pip
RUN pip install -r requirements.txt

EXPOSE 5000

CMD python3 ./app.py
```

```bash
docker build -t python-web --file Dockerfile .
docker run -p 5000:5000 python-web 
```