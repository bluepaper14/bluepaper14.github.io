---
title: 동기와 비동기
author: kongjihyun
date: 2025-09-16 00:00:00 +0800
categories: [리엑트]
tags: [리엑트, 동기, 비동기]
---

### 에제1

예제 코드를 보자. 

```javascript
const fs = require('fs'); //node.js 모듈 불러오기

fs.readFile('./data/readme1.txt', (err, data) => {
    if (err) {
        console.error(err);
    }
    else {
        console.log('1st reading:', data.toString());
    }
});

```
위 코드에서 fs.reaFile을 호출했다. 그렇다면 해당 함수의 의미를 알아보기 위해선 노드 공식 사이트를 이용하자.

https://nodejs.org/api/fs.html#fsreaddirpath-options-callback

현재 <u>reaFile은 파일을 비동기적으로 읽는 함수이다.</u> 비동기란..

>비동기 : 파일 읽기 작업이 완료될 까지 다른 코드를 계속 실행할 수 있게 한다.

위 예제가 아닌 기본형을 찾아보자.

```javascript
import { readFile } from 'node:fs';

readFile('/etc/passwd', (err, data) => {
  if (err) throw err;
  console.log(data);
});
```
여기서 '/etc/passwd'는  읽어올 파일의 경로이고 중요한 점은 
```javascript
(err, data) => { ... }
```
이부분을 콜벡함수 파일 읽기 작업이 끝나면 해당 함수가 실행되는데..
err는 오류 객체를 담고 data는 파일의 내용을 담는 객체이다. 결론적으로 오류가 발생하면 프로그램을 중단하고 없다면 콘솔에 출력하는 함수이다. 결론적으로 <u>해당 경로의 파일을 읽고 그 내용을 출력하는 것이다.

그렇다면 왜 위 예제가 비동가 함수랑 관련이 있을까?</u>

### 동기와 비동기의 의미

- 동기

>동기는 하나의 작업이 완료될때까지 다음 작업이 대기한다.

- 비동기

>비동기는 하나의 작업의 완료를 기다리지 않고 바로 다음 작업으로 간다.

여기서 또 다른 예제를 보자.

### 예제2

```javascript
const fs = require('fs');

fs.readFile('./data/readme1.txt', (err, data) => {
    if (err) {
        console.error(err);
    }
    else {
        console.log('1st reading:', data.toString());
    }
});
```
결과를 실행해보면 

```
1st reading: This file contains sample text #1.
```
이렇게 출력된다. 만약에..출력결과를

```
start
1st reading: This file contains sample text #1.
2st reading: This file contains sample text #2.
3st reading: This file contains sample text #3.
end
```
이렇게 만들기 위해선 어떻게 위 코드를 변형해야할까.

```javascript
const fs = require('fs');

console.log('start');
fs.readFile('./data/readme1.txt', (err, data) => {
    if (err) {
        console.error(err);
    }
    else {
        console.log('1st reading:', data.toString());
    }
});

fs.readFile('./data/readme2.txt', (err, data) => {
    if (err) {
        console.error(err);
    }
    else {
        console.log('2nd reading:', data.toString());
    }
});

fs.readFile('./data/readme3.txt', (err, data) => {
    if (err) {
        console.error(err);
    }
    else {
        console.log('3rd reading:', data.toString());
    }    
});

console.log('end');
```
1번안이다. 이렇게 하면 출력은 이렇다.
```
start
end
1st reading: This file contains sample text #1.
2nd reading: This file contains sample text #2.
3rd reading: This file contains sample text #3.
```
결론적으로 console.log는 동기이고 아래 배웠던 readFile은 비동기 함수이기 떄문에 <u>fs는 요청만 던져놓고 다음줄인 end를 출력한 것이다.</u> 또한 세 파일의 결과 순서도 보장되지 않는다. OS의 상황에 따라 다르다. 그렇다면 다음 2번안이다.

```javascript
const fs = require('fs');

console.log(`start`);
fs.readFile('./data/readme1.txt', (err, data) => {
    if (err) {
        console.error(err);
    }
    else {
        console.log('1st reading:', data.toString());
        fs.readFile('./data/readme2.txt', (err, data) => {
            if (err) {
                console.error(err);
            }
            else {
                console.log('2nd reading:', data.toString());
                fs.readFile('./data/readme3.txt', (err, data) => {
                    if (err) {
                        console.error(err);
                    }
                    else {
                        console.log('3rd reading:', data.toString());
                    }                    
                    console.log(`end`);
                });
            }
        });
    }
});
```
이번엔 비동기 파일을 중첩하였다. 해당 첫번째 파일이 성공적으로 읽으면 그 안에서 두번째 파일을  읽도록 하였다. 이렇게 실행순서를 보장해주고 있지만 너무 복잡하다. 그래서 다음엔 해결책으로 Promise에 대해 살펴보자.
