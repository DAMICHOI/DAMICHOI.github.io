---
title : \[JAVA] 대소문자 변환 - ASCII Code 이용
date : 2021-08-03 00:00:00+0900
tags : [java,codingTest]
---

Java 에서 영문자를 대문자 혹은 소문자로 일괄 변환시켜주는 방법에는 [`toUpperCase()` 와 `toLowerCase()` 메서드를 사용하는 방법](/java-toUpperCase-toLowerCase)이 있다.   

이러한 내장함수를 사용하지 않는 경우에는 ASCII Code 를 이용하여 변환할 수 있는데 이번에는 이 방법을 이용하여 대소문자 변환을 할 수 있다.

대문자에서 `32`를 더하면 소문자가 되고, 소문자에서 `32`를 빼면 대문자가 이용하는 방법이다.

```java
import java.util.Scanner;

public class Main {
    public String convert (String str) {
        String result = "";
        
        for (char c : str.toCharArray()) {
            if (c >= 65 && c <= 90) {   // 영소문자 : 65~90
                result += (char)(c+32);
            } else if (c >= 97 && c <= 122) {   // 영대문자 : 97~122 
                result += (char) (c-32);
            }
        }
        
        return result;
    }
    
    public static void main (String[] args) {
        Main T = new Main();
        Scanner scanner = new Scanner(System.in);
        String input = scanner.next();
        System.out.println(T.convert(input));
    }
}
```