---
title: promise객체(동기와 비동기2)
author: kongjihyun
date: 2025-09-18 00:00:00 +0800
categories: [리엑트]
tags: [리엑트, 동기, 비동기, promise]
---

### 에제1

예제 코드를 보자.

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
이번엔 비동기 파일을 중첩하였다. 해당 첫번째 파일이 성공적으로 읽으면 그 안에서 두번째 파일을  읽도록 하였다. 이렇게 실행순서를 보장해주고 있지만 너무 복잡하다.

> 일명.. 콜벡지옥

 그래서 다음엔 해결책으로 Promise에 대해 살펴보자.

### Promise

Promise는 객체이다. 비동기 작업을 위해 만들어졌다. 결론적으로 미래에 완료될 작업의 최종 성공 또는 실패를 나타낸다. 기본구조를 보자.

```javascript
const promise = new Promise((resolve, reject) => {
    // 비동기 작업
    if (성공) resolve(result);
    else reject(error);
});
```
먼저 new 키워드를 사용해 Promise 인스턴스를 만들었다. 

> 클래스 : 붕어빵 틀 
> 인스턴스 : 실체. 붕어빵임

이후 (resolve, reject) => { .. }는 실행함수를 의미한다. 이제 바로 실행하는 대신 .then과 .catch를 이용해서 결과를 처리한다. 

### .then()

then은 Promise 객체 전용 메서드이다.
.then() 안의 함수는 promise가 성공했을 때 실행된다. Promise 함수에서 얻은 객체에서만 사용 가능하다. 

```javascript
const fs = require('fs');

console.log(`start`);
const promise = new Promise((resolve, reject) => {
    fs.readFile('./data/readme1.txt', (err, data1) => {
        if (!err) resolve(data1);
        else reject(err);
    })
});

promise
.then(data1 => {
    console.log('1st reading:', data1.toString());
    return new Promise((resolve, reject) => {
        fs.readFile('./data/readme2.txt', (err, data2) => {
            if (!err) resolve(data2);
            else reject(err);
        })
    })
})
.then(data2 => {
    console.log('2nd reading:', data2.toString());
    return new Promise((resolve, reject) => {
        fs.readFile('./data/readme3.txt', (err, data3) => {
            if (!err) resolve(data3);
            else reject(err);
        })
    })
})    
.then(data3 => console.log('3rd reading:', data3.toString()))
.catch(err => console.error(err.message))
.finally(() => console.log('end'))
    
```
첫번째 Promise의 객체가 생성되고 readme1.txt를 읽기 시작한다. 이후 
```
promise
then.
```
이부분이 실행되며 1st reading이 출력됨 

결론적으로 promise는 딱 한번만 const로 객체를 만들어서 promise라느 객체의 변수에 담았다. 이후 완료 되었을때 실행되게 만들었고 then은 새로운 promsie를 만들지만 굳이 const promise로 새로 안 만들어도 된다.(항상 새로운 promise반환)

- Promise 객체는 여러개 만들어진다.
- 다만 변수를 담아 관리할 필요는 없음. then으로 연결하면 자연스럽게 prommise가 새롭게 만들어짐.
