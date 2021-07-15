---
title : \[JAVA] Scanner(스캐너)
date : 2021-07-12 00:00:00+0900
tags :
- JAVA
- coding_test
---
   
Scanner 클래스는 읽은 바이트를 **문자, 정수, 실수, 불린, 문자열 등 다양한 타입으로 변환하여 리턴**하는 클래스로 **java.util.Scanner**를 import하여 사용한다.

## Scanner(스캐너) 기본 사용법

``` java
import java.util.Scanner; //기본적으로 import 문 필요
Scanner scan = new Scanner(System.in); // Scanner 객체
```

- System.in을 사용하여 키보드 입력 값을 읽고 원하는 타입으로 변환하여 리턴한다.

## Scanner 주요 메서드

| 메소드 | 설명 |   
| :--- | :--- |
| **String** next() | 다음 토큰을 문자열로 리턴 |
| **byte** nextByte() | 다음 토큰을 **byte** 타입으로 리턴  |
| **short** nextShort() | 다음 토큰을 **short** 타입으로 리턴 |
| **int** nextInt() | 다음 토큰을 **int** 타입으로 리턴 |
| **long** nextLong() | 다음 토큰을 **long** 타입으로 리턴 |
| **float** nextFloat() | 다음 토큰을 **float** 타입으로 리턴 |
| **double** nextDouble() | 다음 토큰을 **double** 타입으로 리턴 |
| **String** nextLine() | '\n'을 포함하는 한 라인을 읽고 '\n'을 버린 나머지만 리턴 |
| void close() | Scanner의 사용 종료 |
| boolean hasNext() | 현재 입력된 토큰이 있으면 true, 아니면 새로운 입력이 들어올 때까지 무한정 기다려서, 새로운 입력이 들어오면 그 때 true 리턴. ctrl + z 키가 입력되면 입력 끝이므로 false 리턴 |
