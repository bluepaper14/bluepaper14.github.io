---
title: var let const 비교하기
author: kongjihyun
date: 2025-09-02 00:00:00 +0800
categories: [리엑트]
tags: [var, 리엑트, 변수, 상수]
---

# var let const 비교

### var let const 정의

먼저 var의 사용법을 알아보자. var은 자바스크립트에서 일반적으로 사용하는 변수 선언이다. 예를 들어보자.

```javascript
var pizza = true
pizza = false
console.log(pizza)
```

var로 변수를 선언한 동시에 false로 재할당할 수 있다. 하지만 const는 var와 다르다.

const는 재할당 할 수 없는 변수를 선언할 때 사용되기 때문에 

```javascript
const pizza = true
pizza = false
console.log(pizza)
```

이렇게 작성하면 출력오류가 나게된다.(Uncaught TypeError TypeError: Assignment to
constant variable.)

마지막으로 let은 재할당은 가능하지만 재선언은 불가능한 변수 선언법이다. 

| 키워드   | 스코프 | 재선언 | 재할당 |
| ----- | --- | --- | --- |
| var   | 함수  | 가능  | 가능  |
| let   | 블록  | 불가  | 가능  |
| const | 블록  | 불가  | 불가  |

### 스코프 블록

이제 var과 let const를 사용할때 스코프의 기준으로 달라진다. 예를 들어보자.

```javascript
var topic = "자바스크립트"
if (topic) {
var topic = "리액트"
console.log('블록', topic)
}
console.log('글로벌', topic)

블록 리엑트
글로벌 리엑트
```
이렇게 var를 if 안의 스코프 밖에 탈출하더라도 topic의 값은 재할당 후 변하지 않는 모습이다. 

반면 let을 이용할시
```javascript
var topic = "자바스크립트"
if (topic) {
let topic = "리액트"
console.log('블록', topic)
}
console.log('글로벌', topic)

블록 리액트
글로벌 자바스크립트
```
이런식으로 let을 이용하여 재할당을 하고, 탈출후 블록 밖에서는 전역 변수의 값으로 사용됨을 볼 수 있다.

### var let 예제

```javascript
for (var i = 0; i < 5; i++) {
  div.onclick = function() {
    alert('이것은 박스 #' + i + '입니다.')
  }
}
```
var 변수는 전역에 묶이기 때문에 클릭시 항상 i = 5가 출력된다.

```javascript
for (let i = 0; i < 5; i++) {
  div.onclick = function() {
    alert('이것은 박스 #' + i + '입니다.')
  }
}
```
만약 let 사용시 변수는 블록에 묶기기 때문에 각 루프마다 i가 새로 만들어져서 고유의 값을 유지하며 클릭된다. 
