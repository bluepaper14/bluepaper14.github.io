---
title: 리눅스 명령어 정리 
author: kongjihyun
date: 2025-09-09 00:00:00 +0800
categories: [리눅스]
tags: [리눅스]
---

### 시작부터..

먼저 리눅스를 실습하기 이전에 파워쉘에서 wsl을 다운로드해 실습할 수도 있고 버튜얼 박스를 설치하고 우분투로 사용할 수 있다. 편한걸로하자. 

일단 기본적으로 숙지해야할 리눅스 명령어를 숙지해보자.

### man

리눅스 man은 메뉴얼(manual)의 줄임말이다.

```bash
man ls
```

### ls

ls는 현재 디렉토리 파일의 목록을 출력이다.

```bash
ls
```
여기서 ls -al 이렇게 작성되면 숨김 파일 모두 출력가능하다. 

### cd

cd는 디렉토리 변경(change directory)의 줄일말로 현재 디렉토리에서 다른 디렉토리로 이동할 때 사용된다. 예를 들어

```bash
cd fruit
```
해당 디렉토리를 이동하는 것이다.

이외에 여러 기능들이 포함되어 있다.
```bash
cd .. //상위 디렉토리로 이동
cd ~  //바로 직전 디렉토리로 이동
cd /  //루트 디렉토리로 이동
```

### pwd

pwd는 현재 작업 디렉토리 출력(print working directory)의 줄임말로 지금 위치하는 디렉토리를 출력하는 명령어이다.

```bash
pwd
```

### mkdir

mkdir은 디렉토리 생성(make directory)의 줄임말이다. 예를 들어

```bash
mkdir test
```
이런식으로 하나의 디렉토리를 생성할 수 도 있고

```bash
mkdir dogs cars
```
한번에 두개의 디렉토리를 생성할 수 있다.

```bash
mkdir -p project/src/utils
```
이런식으로 디렉토리 안으로 여러개 생성 가능하다.

### rmdir

rmdir은 디렉토리 제거(remove directory)디렉토리 제거하는 명령어다. 

```bash
rmdir test
```

### mv

mv는 이동의 줄임말인데 일반적으론 이름 변경하는데 사용된다. 예를 들어
```bash
mv oldname newname
```
### cp

cp는 복사의(copy) 줄일말로 복사할때 사용되는 명령어이다. 예를 들어
```bash
cp [원본파일] [목적지]
```

### open

open은 파일을 연결된 프로그램으로 여는 명령어인데 리눅스에서는 사용하지 못하고 macOs에서 사용된다.

```bash
open apple.txt
```
### touch

touch는 새파일을 생성할 때 자주 사용된다.

```bash
touch apple.txt
```




