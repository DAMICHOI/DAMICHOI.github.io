---
layout: posts
title : \[Algorithm] Programmers - 12917. 문자열 내림차순으로 배치하기
date : 2021-07-25 00:00:00+0900
categories: [posts, algorithm]
tags : [algorithm]
toc: true
toc_sticky: ture
---

# 문자열 내림차순으로 배치하기
문자열 s에 나타나는 문자를 큰것부터 작은 순으로 정렬해 새로운 문자열을 리턴하는 함수, solution을 완성해주세요.
s는 영문 대소문자로만 구성되어 있으며, 대문자는 소문자보다 작은 것으로 간주합니다.

### 제한 사항
str은 길이 1 이상인 문자열입니다.

### 입출력 예

| s | return | 
| :---: | :---: | 
| "Zbcdefg" | "gfedcbZ" | 


---

### 내가 작성한 코드
``` java
import java.util.*;

class Solution {
    public String solution(String s) {
        String answer = "";
        String tempL = "", tempU = "";
        String[] arr = s.split("");

        for (String str : arr) {
            if (Character.isLowerCase(str.charAt(0))) {
                tempL += str;
            }

            if (Character.isUpperCase(str.charAt(0))) {
                tempU += str;
            }
        }
        answer += reverseString(tempL);
        answer += reverseString(tempU);

        return answer;
    }

    public String reverseString(String str) {
        String result = "";
        String[] sortArr = str.split("");

        Arrays.sort(sortArr);

        for (int i=(sortArr.length-1); i>=0; i--) {
            result += sortArr[i];
        }

        return result;
    }
}
```

<br/>   
StringBUilder로 풀어볼까 하다가 위와 같이 작성하였는데, StringBuilder를 이용하여 짧은 코드로 풀이하신 분들이 많았다.

### 다른 사람 풀이

``` java 
import java.util.Arrays;

public class ReverseStr {
    public String reverseStr(String str){
        char[] sol = str.toCharArray();
        Arrays.sort(sol);
        return new StringBuilder(new String(sol)).reverse().toString();
    }

    // 아래는 테스트로 출력해 보기 위한 코드입니다.
    public static void main(String[] args) {
        ReverseStr rs = new ReverseStr();
        System.out.println( rs.reverseStr("Zbcdefg") );
    }
}
```