---
title : \[Algorithm] Leetcode - 394. Decode String (JAVA)
date : 2021-12-20 10:55:00+0900
categories: [posts, algorithm]
tags : [algorithm]
toc: true
toc_sticky: ture
---

## Decode String

Given an encoded string, return its decoded string.

The encoding rule is: `k[encoded_string]`, where the `encoded_string` inside the square brackets is being repeated exactly `k` times. Note that `k` is guaranteed to be a positive integer.

You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, `k. For example, there won't be input like `3a` or `2[4]`.


## Constraints

- `1 <= s.length <= 30`
- `s` consists of lowercase English letters, digits, and square brackets `'[]'`. 
- `s` is guaranteed to be **a valid** input. 
- All the integers in `s` are in the range `[1, 300]`.


## Example
### #1

```java
Input: s = "3[a]2[bc]"
Output: "aaabcbc"
```

### #2

```java
Input: s = "3[a2[c]]"
Output: "accaccacc"
```

### #3

```java
Input: s = "2[abc]3[cd]ef"
Output: "abcabccdcdcdef"
```

### #4
```java
Input: s = "abc3[cd]xyz"
Output: "abccdcdcdxyz"
```

---

## 내가 작성한 코드

사실 이 문제는 도저히 풀어도 답이 안나와서 다른 분의 풀이를 참고했다.
Stack 을 이용하여 풀이했다.

```java
class Solution {
    public String decodeString(String s) {
        int repeat = 0;
        StringBuilder output = new StringBuilder();
        Stack<StringBuilder> curs = new Stack<>();
        Stack<Integer> repeats = new Stack<>();
        
        for (int i = 0; i < s.length(); i++) {
            char temp = s.charAt(i);
        
            if (Character.isDigit(temp)) {
                repeat = repeat * 10 + Character.getNumericValue(temp);
            } else if (temp == '[') {
                curs.push(output);
                repeats.push(repeat);
                repeat = 0;
                output = new StringBuilder();
            } else if (temp == ']') {
                repeat = repeats.pop();
                StringBuilder toAdd = output;
                output = curs.pop();
                while (repeat > 0) {
                    output.append(toAdd);
                    repeat--;
                }
            } else {
                output.append(temp);
            }
        }
        
        return output.toString();
    }
}
```

또 다른 풀이방법은 뭐가 있을까 하고 봤는데 Deque 을 이용한 풀이도 있었다.

```java
class Solution {
    public String decodeString(String s) {
        Deque<Integer> countStack = new ArrayDeque();
        Deque<StringBuilder> resStack = new ArrayDeque();
        StringBuilder sb = new StringBuilder();
        int count = 0;
        
        for (int i = 0; i < s.length(); ++i) {
            char c = s.charAt(i);
            if (Character.isDigit(c)) {
                count = count * 10 + (c - '0');
            } else if (Character.isAlphabetic(c)) {
                sb.append(c);
            } else if (c == '[') {
                countStack.push(count);
                resStack.push(sb);
                sb = new StringBuilder();
                count = 0;
            } else if (c == ']') {
                StringBuilder prefix = resStack.pop();
                int k = countStack.pop();
                while (k > 0) {
                    prefix.append(sb);
                    k--;
                }
                sb = prefix;
            }
        }
        
        return sb.toString();
    }
}
```

### 성능 확인
![time complexity](/assets/images/2021-12-20-leetcode-394.png)

두 코드의 성능 차이는 크지 않다. 하지만, Java 공식문서에서는 이렇게 말하고 있다.

> A more complete and consistent set of LIFO stack operations is provided by the Deque interface and its implementations, which should be used in preference to this class.

보다 완전하고 일관된 LIFO 스택 작업은 Deque 인터페이스와 이 클래스(Stack)보다 우선적으로 사용해야 하는 해당 구현에 의해 제공됩니다.

즉, Stack 대신 Deque 의 구현체인 ArrayDeque 사용을 제안하고 있기 때문에 Stack 보다는 Deque 를 이용하여 풀이하는 것이 좋겠다.

## Reference
- [Java 공식 문서](https://docs.oracle.com/javase/8/docs/api/)

> 더 많은 알고리즘 문제 풀이는 [여기서](https://github.com/DAMICHOI/Algorithm) 확인할 수 있다.