---
title : \[Algorithm] Leetcode - 860. Lemonade Change (JAVA) / Greedy 알고리즘
date : 2022-02-07 12:38:00+0900
categories: [posts, algorithm]
tags : [algorithm]
toc: true
toc_sticky: ture
---

## Greedy (탐욕법)

그리디 알고리즘을 사용하면 어떤 기준에 따라 매 순간 가장 좋아보이는 것을 선택한다.
그러나 현재의 선택이 미래에 미칠 영향까지는 고려하지 않기 때문에 항상 최적의 답을 도출하는 것은 아니다.

그리디 알고리즘은
- 탐욕 선택 속성(Greedy choice property) : 현재 선택이 이후의 선택에 영향을 주지 않음
- 최적 부분 구조(Optimal Substructure) : 매 순간의 최적의 해가 문제 전체에 대한 최적의 해여야 함

특성을 가지는 문제들을 해결하는 데 강점이 있다. 즉, 한 번의 선택이 다음 선택에는 전혀 무관한 값이어야 하며, 매 순간의 최적해가 문제에 대한 최적해야 한다는 의미이다.

> **용어 참조**
> - 최적해 : 선형 계획법에서, 제약 조건을 충족시킬 수 있는 해 가운데 목적 함수값을 최대 또는 최소로 만드는 값.

## Lemonade Change

At a lemonade stand, each lemonade costs `$5`. Customers are standing in a queue to buy from you and order one at a time (in the order specified by bills). Each customer will only buy one lemonade and pay with either a `$5`, `$10`, or `$20` bill. You must provide the correct change to each customer so that the net transaction is that the customer pays `$5`.

Note that you do not have any change in hand at first.

Given an integer array `bills` where `bills[i]` is the bill the i<sup>th</sup> customer pays, return `true` if you can provide every customer with the correct change, or `false` otherwise.


## Constraints

- 1 <= bills.length <= 10<sup>5</sup>
- `bills[i]` is either `5`, `10`, or `20`.


## Example
### #1

```java
Input: bills = [5,5,5,10,20]
Output: true
Explanation:
From the first 3 customers, we collect three $5 bills in order.
From the fourth customer, we collect a $10 bill and give back a $5.
From the fifth customer, we give a $10 bill and a $5 bill.
Since all customers got correct change, we output true.
```

### #2

```java
Input: bills = [5,5,10,10,20]
Output: false
Explanation:
From the first two customers in order, we collect two $5 bills.
For the next two customers in order, we collect a $10 bill and give back a $5 bill.
For the last customer, we can not give the change of $15 back because we only have two $10 bills.
Since not every customer received the correct change, the answer is false.
```

---

## 내가 작성한 코드

Greedy 알고리즘의 대표적인 예로 거스름돈 문제를 들 수 있다.

우선 문제 그대로 아래와 같이 풀어볼 수 있다.

``` java
class Solution {
    public boolean lemonadeChange(int[] bills) {
        if (bills[0] != 5) {    // 첫 고객이 5달러가 아닌 비용을 지불할 경우,
            return false;       // 거스름돈을 줄 수 없으므로 false 반환
        }
        
        int fiveCount = 0;  // 5달러 지폐의 갯수
        int tenCount = 0;   // 10달러 지폐의 갯수
        
        for (int i = 0; i < bills.length; i++) {
            if (bills[i] == 5) {
                fiveCount++;        // 고객이 5달러 지불할 경우, fiveCount 1 증가
                
            } else if (bills[i] == 10) {    // 고객이 10달러 지불할 경우 (5달러를 거스름돈으로 줘야 한다.)
                if (fiveCount <= 0) {
                    return false;       // fiveCount가 0 이하면, 거스름돈을 줄 수 없기 때문에 false 반환
                }
                fiveCount--;    // 5달러 거스름돈을 주기 때문에 fiveCount 1 감소
                tenCount++;     // 10달러를 받았기 때문에 tenCount 1 증가
                
            } else {    // 고객이 20달러를 지불할 경우 (15달러를 거스름돈으로 줘야 한다.)
                if (fiveCount > 0 && tenCount > 0) {    // 5달러와 10달러를 가지고 있다면,
                    tenCount--;     // 10달러 거스름돈을 주기 때문에 tenCount 1 감소
                    fiveCount--;    // 5달러 거스름돈을 주기 때문에 fiveCount 1 감소
                } else if (fiveCount >= 3) {    // (10달러 짜리가 없고) 5달러를 3개 이상 가지고 있다면,
                    fiveCount -= 3;             // 15달러 거스름돈을 5달러 짜리로 주기 때문에 fiveCount 3 감소
                } else {
                    return false;   // 5달러와 10달러 모두 없으면, 거스름돈을 줄 수 없기 때문에 false 반환
                }
            }
        }
        return true;
    }
}
```
위와 같이 풀이할 경우,
- 시간 복잡도 : n개의 요소에 대해 O(1) 의 탐색을 하므로 시간 복잡도는 O(n) 이다.
- 공간 복잡도 : n의 값에 상관없이 변수 fiveCount, 변수 tenCount 만 필요하므로, 공간 복잡도는 O(1) 이다.


그리고 Hash를 이용하여 아래와 같이 풀어볼 수도 있다.

```java
class Solution {
    public boolean lemonadeChange(int[] bills) {
        if (bills[0] != 5) {    // 첫 고객이 5달러가 아닌 비용을 지불할 경우,
            return false;       // 거스름돈을 줄 수 없으므로 false 반환
        }
        
        Map<Integer, Integer> change = new HashMap<>(); // 거스름돈
        change.put(5, 0);   // 거스름돈을 5달러 0개로 초기화
        change.put(10, 0);  // 거스름돈을 10달러 0개로 초기화
        
        for (int i = 0; i < bills.length; i++) {
            if (bills[i] == 5) {
                change.put(bills[i], change.getOrDefault(bills[i], 0) + 1); // 고객이 5달러를 지불할 경우, 거스름돈 5달러 +1
            } else if (bills[i] == 10) {    // 고객이 10달러를 지불할 경우
                if (change.get(5) < 1) {
                    return false;   // 5달러짜리 지폐가 없을 경우, 거스름돈을 줄 수 없으므로 false 반환
                } else {    // 있을 경우,
                    change.put(5, change.get(5) - 1);   // 5달러를 거스름돈으로 줘야 하므로, 5달러 -1
                    change.put(bills[i], change.getOrDefault(bills[i], 0) + 1); // 10달러를 받았으므로, 거스름돈 10달러 +1
                }
            } else {    // 고객이 20달러를 지불할 경우
                if (change.get(5) > 0 && change.get(10) > 0) {  // 거스름돈 5달러와 10달러가 있을 경우
                    change.put(5, change.get(5) - 1);   // 5달러를 거스름돈으로 줘야 하므로, 5달러 -1
                    change.put(10, change.get(10) - 1); // 10달러를 거스름돈으로 줘야 하므로, 10달러 -1
                } else if (change.get(10) == 0 && change.get(5) > 2) {  // 거스름돈 10달러가 없고, 5달러가 2개보다 많을 경우
                    change.put(5, change.get(5) - 3);   // 5달러 3개를 거스름돈으로 줘야 하므로, 5달러 -3
                } else {
                    return false;   // 거스름돈 5달러와 10달러 모두 없을 경우, 거스름돈을 줄 수 없으므로 false 반환
                }
            }
        }
        return true;
    }
}
```
위와 같이 풀이할 경우에도,
- 시간 복잡도 : n개의 요소에 대해 O(1) 의 탐색을 하므로 시간 복잡도는 O(n) 이다.
- 공간 복잡도 : n의 값에 상관없이 key 5와 key 10의 value 값만 저장할 공간이 필요하므로, 공간 복잡도는 O(1) 이다.

### 성능 확인
시간 복잡도와 공간복잡도가 같지만, Runtime은 Map을 이용한 두 번째 풀이보다 첫 번째 풀이 방법이 속도 면에서 10ms 정도 빨랐으나,
Memory 면에서 Map을 이용한 두 번째 풀이가 1.3MB 정도 더 적은 것을 확인할 수 있었다.


> 더 많은 알고리즘 문제 풀이는 [여기서](https://github.com/DAMICHOI/Algorithm) 확인할 수 있다.