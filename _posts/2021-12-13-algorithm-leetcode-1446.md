---
title : \[Algorithm] Leetcode - 1446. Consecutive Characters (JAVA)
date : 2021-12-13 00:00:00+0900 
tags : [algorithm]
toc: true
toc_sticky: ture
---

## Consecutive Characters

The **power** of the string is the maximum length of a non-empty substring that contains only one unique character.

Given a string `s`, return the **power** of `s`.

## Constraints

- `1 <= s.length <= 500`
- `s` consists of only lowercase English letters.

## Example

### #1

```java
Input:s="leetcode"
	Output:2
	Explanation:The substring"ee"is of length 2 with the character'e'only.
```

### #2

```java
Input:s="abbcccddddeeeeedcba"
	Output:5
	Explanation:The substring"eeeee"is of length 5 with the character'e'only.
```

### #3

```java
Input:s="triplepillooooow"
	Output:5
```

---

## 내가 작성한 코드

문자열 내에서 동일한 알파벳으로만 이루어진 부분의 문자열의 최대 길이를 반환하는 문제이다.   

이전의 문자열이 같으면 `count = count + 1` 해주고, 그렇지 않으면 `count = 1`로 초기화한다.   
문자열의 최대 길이를 반환하는 문제이기 때문에 maxCount보다 count가 클 때, 갱신해주는 방식으로 풀이했다.

``` java
class Solution {
    public int maxPower(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        
        int count = 0, maxCount = 0;
        char prevC = ' ';
        
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            
            if (prevC == c) {
                count++;
            } else {
                count = 1;
                prevC = c;
            }
            
            maxCount = Math.max(count, maxCount);
        }
        
        return maxCount;
    }
}
```

