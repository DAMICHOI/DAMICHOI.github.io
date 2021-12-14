---
title : \[Algorithm] Leetcode - 9. Palindrome Number (JAVA)
date : 2021-12-14 00:00:00+0900 categories: [posts, algorithm]
tags : [algorithm]
toc: true toc_sticky: ture
---

## Palindrome Number

Given an integer `x`, return `true` if `x` is palindrome integer.

An integer is a **palindrome** when it reads the same backward as forward.

- For example, `121` is a palindrome while `123` is not.

## Constraints

- `-231 <= x <= 231 - 1`

## Follow up

Could you solve it without converting the integer to a string?

## Example

### #1

```java
Input:x=121
 Output:true
```

### #2

```java
Input:x=-121
 Output:false
 Explanation:From left to right,it reads-121. From right to left,it becomes 121-.Therefore it is not a palindrome.
```

### #3

```java
Input:x=10
 Output:false
 Explanation:Reads 01from right to left.Therefore it is not a palindrome.
```

### #4

```java
Input:x=-101
 Output:false
```

---

## 내가 작성한 코드

펠린드롬은 거꾸로 읽어도 제대로 읽는 것과 같은 문장이나 낱말, 숫자, 문자열이다.   
이효리는 거꾸로 읽어도 이효리인 것 처럼 말이다.

사실 펠린드롬이 뭔지 모르고 문제의 내용과 예시만 보고 문자열을 절반으로 잘라서 비교하면 되겠다 싶어 아래와 같이 코드를 작성했다.

``` java
class Solution {
    public boolean isPalindrome(int x) {
        boolean result = false;
        String str = String.valueOf(x);
        int middle = str.length() / 2;
        StringBuilder left = new StringBuilder();
        StringBuilder right = new StringBuilder();

        for (int i = 0; i <= middle - 1; i++) {
            left.append(str.charAt(i));
        }

        for (int i = str.length() - 1; i >= str.length() - middle; i--) {
            right.append(str.charAt(i));
        }

        if (left.toString().equals(right.toString())) {
            result = true;
        }

        return result;
    }
}
```

이것도 맞는 방식이긴 하다. 하지만 시작과 끝이 같기 때문에 굳이 문자열을 반으로 나누지 않아도 된다.

우선, 모든 음수는 펠린드롬이 아니다. 예를 들어 -121은 '-'와 '1'이 같지 않기 때문이다.

그 다음 문자열과 끝부분부터 뒤집은 문자열을 비교하여 일치하면 true 를 반환해주면 된다.

``` java
class Solution {
    public boolean isPalindrome(int x) {
        int reversed = 0;
        int temp = x;
        while (temp > 0) {
            reversed = reversed * 10 + temp % 10;
            temp /= 10;
        }

        return x == reversed;
    }
}
```

문제는 펠린드롬 숫자인지 여부에 관해 묻고 있기 때문에, 이와 같이 풀면 된다. 풀이 방법이 마치 그리디 알고리즘 같았다.

1. 뒤집은 숫자를 담을 `reversed` 변수를 선언한다.
2. `temp` 변수에 주어진 숫자 `x`를 담는다. `x` 값은 나중에 `reversed`와 비교할 것이기 때문에, `x` 변수를 바로 사용하지 않는다.
3. `temp` 가 0보다 클 때까지 while 문을 실행한다.
    1. 숫자를 `reversed` 뒤에 붙이기 위해서 10을 곱한 후, `temp` 값을 10으로 나눈 나머지 값을 더한다.
    2. `temp` 의 다음 숫자를 가져오기 위해 10으로 나눈다.
4. `x` 의 값과 `reversed`의 값을 비교하여 결과값을 반환한다.


![time complexity comparison](/assets/images/2021-12-14-leetcode-9.png)

위와 같이 첫번째 풀이보다 두번째 풀이가 속도도 빠르고 메모리도 적게 쓰는 것을 알 수 있다.


> 더 많은 알고리즘 문제 풀이는 [여기서](https://github.com/DAMICHOI/Algorithm) 확인할 수 있다.