---
title : \[JAVA] 대소문자 변환 메서드 - toUpperCase(), toLowerCase()
date : 2021-08-03 00:00:00+0900
categories: [posts, java]
tags : [java,codingTest]
toc: true
toc_sticky: ture
---

Java 에서 영문자를 대문자 혹은 소문자로 일괄 변환시켜주는 메서드가 있다.

## `String toUpperCase()`
문자열을 전부 대문자로 변환한다.

``` java
String str = "Hello, World";

System.out.println(str.toUpperCase());  // 출력값 : HELLO, WORLD
```

## `String toLowerCase()`
문자열을 전부 소문자로 변환한다.

``` java
String str = "Hello, World";

System.out.println(str.toLowerCase());  // 출력값 : hello, world
```