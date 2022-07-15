---
title:  "Javascript Study 05. type coericon"
excerpt: "자바스크립트 스터디 05. 타입 변환"

categories: development
tags:
  - [javascript, type-coericon]

toc: true
toc_sticky: true
 
date: 2022-07-13-00:01
last_modified_at: 2022-07-13-00:02
---
<center><i>이웅모 저자의 모던 javascript deep dive를 공부한 내용을 정리한 글입니다.</i></center>

### 타입 변환이란?
- 개발자의 의도대로 타입을 변환하는 것 : 명시적 타입 변환, 타입 캐스팅
- 개발자의 의도와 상관 없이 타입을 변환하는 것 : 암묵적 타입 변환, 타입 강제 변환
- 타입 변환을 한다고 해서 원시 값이 변경되지 않음
- 타입 변환은 기존 원시 값을 사용해 새로운 타입을 생성하는 것

```javascript
// 명시적 타입 변환
var x = 10;
var str = x.toString();

console.log(typeof str, str); // string 10
console.log(typeof x, x);     // number 10
```

```javascript
// 암묵적 타입 변환
var x = 10;
var str = x + '';

console.log(typeof str, str); // string 10
console.log(typeof x, x);     // number 10
```

```javascript
1 + '2'  // 12
`1 + 1` = ${1 + 1}  // "1 + 1 = 2"
```

#### Truthy값과 Falsy값
- Falsy 값들
  - false
  - undefined
  - null
  - 0, -0
  - NaN
  - ''

```javascript
// Truthy, Falsy 값을 판별하는 함수
function isFalsy(v){
  return !v;
}

function isTruthy(v){
  return !!v;
}

```
#### 단축 평가
- 논리 연산자 표현식의 평가 결과가 불리언 값이 아닌 경우

```javascript
// && : Dog 까지 봐야 truthy를 판단할 수 있기 때문에 Dog가 출력
// || : Cat 만 봐도 truthy를 판단할 수 있기 때문에 Cat이 출력
'Cat' && 'Dog'    // "Dog"
'Cat' || 'Dog'    // "Cat"
```