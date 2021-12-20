---
title : \[Algorithm] Leetcode - 1200. Minimum Absolute Difference (JAVA)
date : 2021-12-20 10:12:00+0900
categories: [posts, algorithm]
tags : [algorithm]
toc: true
toc_sticky: ture
---

## Minimum Absolute Difference

Given an array of **distinct** integers `arr`, find all pairs of elements with the minimum absolute difference of any two elements.

Return a list of pairs in ascending order(with respect to pairs), each pair `[a, b]` follows

- a, b are from arr
- a < b
- b - a equals to the minimum absolute difference of any two elements in arr


## Constraints

- `2 <= arr.length <= 10^5`
- `-10^6 <= arr[i] <= 10^6`


## Example
### #1

```java
Input: arr = [4,2,1,3]
Output: [[1,2],[2,3],[3,4]]
Explanation: The minimum absolute difference is 1. List all pairs with difference equal to 1 in ascending order.
```

### #2

```java
Input: arr = [1,3,6,10,15]
Output: [[1,3]]
```

### #3

```java
Input: arr = [3,8,-10,23,19,-4,-14,27]
Output: [[-14,-10],[19,23],[23,27]]
```

---

## 내가 작성한 코드

문제를 읽어보면 이미 푸는 방법이 나와있다.
1. 오름차순으로 반환하라는 것.
2. 절대값이 최소인 요소의 모든 쌍을 찾으라는 것.

오름차순으로 정렬을 하는 것만으로는 절대값의 최소값을 구할 수 없다.   
첫 번째 절대값이 두 번째 절대값보다 작을 것이라는 장담은 할 수 없으니까 말이다.

그래서 우선 반복문을 이용하여 절대값의 최소값을 구한다.   
그 다음 반복문을 통해 최소값과 절대값이 일치하면 list에 추가하는 방식으로 풀었다.

``` java
class Solution {
   public List<List<Integer>> minimumAbsDifference(int[] arr) {
      Arrays.sort(arr);
      List<List<Integer>> arrayList = new ArrayList<>();
      int abs = 0;
      int absMin = (int)1e6;
      
      for (int i = 0; i < arr.length - 1; i++) {
         absMin = Math.min(absMin, arr[i + 1] - arr[i]);
      }
      
      for (int i = 0; i < arr.length - 1; i++) {
         List<Integer> list = new ArrayList<>();
         abs = Math.abs(arr[i + 1] - arr[i]);
         if (abs == absMin) {
            list.add(arr[i]);
            list.add(arr[i + 1]);
            arrayList.add(list);
         }
      }
      return arrayList;
   }
}
```

### 성능 확인
![time complexity](/assets/images/2021-12-20-leetcode-1200.png)

속도면에서 상위 3% 이내로 다른 제출물들보다 빠른 것을 확인했다. 상대적으로 메모리 사용률은 아쉬운 것 같다.


> 더 많은 알고리즘 문제 풀이는 [여기서](https://github.com/DAMICHOI/Algorithm) 확인할 수 있다.