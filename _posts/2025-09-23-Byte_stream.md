---
title: File I/O 정리
author: kongjihyun
date: 2025-09-23 00:00:00 +0800
categories: [리눅스]
tags: [Open(), Close()]
---

### File I/O

우리가 사용하는 리눅스 환경에서는 모든 것을 파일 취급한다.

1) Regular file

- Text file : 사람이 읽는 문자(.c, .html, .txt)

- Binary file : 기계가 읽는 이진 데이터(.out, .png)

2) Special file

### File I/O의 특징

리눅스/유닉스 계열에서 파일 입출력은 Byte Stream 방식이다.(데이터는 바이트 단위로 주고 받음) 가장 우선적으로 2가지 함수를 알아보자.
Open()과 Close()이다.

### Open()

디스크에 있는 파일을 메모리에 올려 커널이 관리하게끔 하는데 이를 프로그램이 파일을 직접 이름으로 접근하지 않고 번호로 접근한다.

```c
int open(const char *path, int flags, mode_t mode);
```
path : 열 파일의 이름
flags : 열기 방식
mode : 새 파일을 만들때 권한 

동작과정은 다음과 같다. 
- Open()을 호출
- 커널이 파일 테이블에 올린다. -> FD(번호) 부여
- 해당 번호를 이용해 read나 다른거 실행시킴

### Close()
 
 Close()는 이제 열린 파일들을 정리한다. 커널이 관리하던 파일 테이블 엔트리를 해제하고 다른 프로세스나 프로그램이 FD를 재사용할 수 있게 한다.

 이제 간단한 예제를 들어보자.

 ### 예제1

 ```c
#include <fcntl.h> //Open()에서 사용하는 상수들 정의
#include <unistd.h> //I/O 상수들 정의
#include <stdio.h> //perror 입출력 관련 함수정의

int main() {
    int fd = open("log.txt", O_WRONLY | O_CREAT | O_APPEND, 0644); 
    if (fd == -1) { perror("open"); return 1; }

    if (close(fd) == -1) { perror("close"); return 1; }
    return 0;
}

 ```
 아래 줄을 보자.
```c
 int fd = open("log.txt", O_WRONLY | O_CREAT | O_APPEND, 0644);
 if (fd == -1) { perror("open"); return 1; }
```
- log.txt 파일 접근, 반환값 fs는 성공이면 0>= 실패면 -1 반환.

```c
if (close(fd) == -1) { perror("close"); return 1; }
```
- Close()로 파일 닫기. 성공하면 0, 실패하면 -1, 실패하면 perror로 에러를 출력하고 종료코드 1을 반환한다. 
