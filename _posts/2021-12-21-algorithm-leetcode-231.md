---
title : \[Algorithm] Leetcode - 231. Power of Two (JAVA)
date : 2021-12-21 11:30:00+0900
categories: [posts, algorithm]
tags : [algorithm]
toc: true
toc_sticky: ture
---

## Power of Two

Given an integer `n`, return `true` if it is a power of two. Otherwise, return `false`.

An integer `n` is a power of two, if there exists an integer `x` such that `n == 2^x`.


## Constraints

- `-231 <= n <= 231 - 1`


## Example
### #1

```java
Input: n = 1
Output: true
Explanation: 2^0 = 1
```

### #2

```java
Input: n = 16
Output: true
Explanation: 2^4 = 16
```

### #3

```java
Input: n = 3
Output: false
```

---

## 내가 작성한 코드

2<sup>n</sup> 의 특성을 이용하면 아래와 같다.

|   1   |   2   |   4   |   8   |   16  |   ... |
|   --- |   --- |   --- |   --- |   --- |   --- |
|   1   |   10  |   100 |   1000    |   10000   |   ... |

반복문과 오른쪽 시프트 연산자을 이용하여 각 비트의 값을 차례로 결정할 수 있다.

``` java
class Solution {
    public boolean isPowerOfTwo(int n) {
        if (n < 1)
            return false;
        
        while (n != 1) {
            if (n % 2 != 0)
                return false;
            n >>= 1;
        }
        return true;
    }
}
```

간단하게 2로 계속 나눠주는 방법으로도 풀이할 수 있다.

```java
class Solution {
    public int romanToInt(String s) {
        if (n == 0)
            return false;
        while (n != 1) {
            if (n % 2 != 0)
                return false;
            n /= 2;
        }
        return true;        
    }
}
```

### 성능 확인
![time complexity](/assets/images/2021-12-21-leetcode-231.png)

상단 결과는 성능은 2로 반복해서 나눠주는 방법을 이용했고, 하단은 오른쪽 시프트 연산자를 이용해서 나온 결과이다.

속도의 차이는 거의 없었으나, 메모리 면에서 2로 반복해서 나눠주는 방법을 이용한 풀이가 더 효율적인 것으로 확인 가능하다.


> 더 많은 알고리즘 문제 풀이는 [여기서](https://github.com/DAMICHOI/Algorithm) 확인할 수 있다.