---
title : \[Algorithm] Leetcode - 13. Roman to Integer (JAVA)
date : 2021-12-21 13:50:00+0900
categories: [posts, algorithm]
tags : [algorithm]
toc: true
toc_sticky: ture
---

## Roman to Integer

Roman numerals are represented by seven different symbols: `I`, `V`, `X`, `L`, `C`, `D` and `M`.

```java
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

For example, `2` is written as `II` in Roman numeral, just two one's added together. 
`12` is written as `XII`, which is simply `X + II`. 
The number `27` is written as `XXVII`, which is `XX + V + II`.

Roman numerals are usually written largest to smallest from left to right. 
However, the numeral for four is not `IIII`. 
Instead, the number four is written as `IV`. 
Because the one is before the five we subtract it making four. 
The same principle applies to the number nine, which is written as `IX`. 
There are six instances where subtraction is used:

- `I` can be placed before `V` (5) and `X` (10) to make 4 and 9.
- `X` can be placed before `L` (50) and `C` (100) to make 40 and 90.
- `C` can be placed before `D` (500) and `M` (1000) to make 400 and 900.

Given a roman numeral, convert it to an integer.


## Constraints

- `1 <= s.length <= 15`
- `s` contains only the characters `('I', 'V', 'X', 'L', 'C', 'D', 'M')`.
- It is **guaranteed** that s is a valid roman numeral in the range `[1, 3999].


## Example
### #1

```java
Input: s = "III"
Output: 3
Explanation: III = 3.
```

### #2

```java
Input: s = "LVIII"
Output: 58
Explanation: L = 50, V= 5, III = 3.
```

### #3

```java
Input: s = "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

---

## 내가 작성한 코드

처음엔 indexOf 로 위치 비교도 했다가 switch 도 썼다가 결국 아래와 같이 HashMap 을 이용하여 작성했다.

먼저 HashMap에 문자열과 숫자 값을 세팅해준다.   
그리고 for 반복문을 이용하여 문자열의 맨 오른쪽에서 두 번째부터 반복하여 비교해준다.
앞의 문자열이 뒤의 문자열보다 숫자 값이 크거나 같을 경우 더해주고, 작을 경우에는 빼준다. 

``` java
class Solution {
    public int romanToInt(String s) {
        int result;
        HashMap<Character, Integer> map = new HashMap<>();
        
        map.put('I', 1);
        map.put('V', 5);
        map.put('X', 10);
        map.put('L', 50);
        map.put('C', 100);
        map.put('D', 500);
        map.put('M', 1000);
        
        result = map.get(s.charAt(s.length() - 1));
        
        for (int i = s.length() - 2; i >= 0; i--) {
            if (map.get(s.charAt(i)) >= map.get(s.charAt(i + 1))) {
                result += map.get(s.charAt(i));
            } else {
                result -= map.get(s.charAt(i));
            }
        }
        
        return result;
    }
}
```

속도 면에서 더 빠른 코드가 있어서 가져와 보았다. 아래 코드는 switch 문을 이용하여 풀이했다.

```java
class Solution {
    public int romanToInt(String s) {
        int sum = 0;
        char prev = '?';
        
        for(int i = s.length()-1; i >= 0; i--)    {
            char curr = s.charAt(i);
            switch(curr)    {
                case 'I'    : sum += prev == 'V' || prev == 'X' ? -1 : 1;
                    break;
                case 'V'    : sum += 5;
                    break;
                case 'X'    : sum += prev == 'L' || prev == 'C' ? -10 : 10;
                    break;
                case 'L'    : sum += 50;
                    break;
                case 'C'    : sum += prev == 'D' || prev == 'M' ? -100 : 100;
                    break;
                case 'D'    : sum += 500;
                    break;
                case 'M'    : sum += 1000;
                    break;
                default     : 
                    break;
                }
                prev = curr;
            }
        return sum;        
    }
}
```


> 더 많은 알고리즘 문제 풀이는 [여기서](https://github.com/DAMICHOI/Algorithm) 확인할 수 있다.