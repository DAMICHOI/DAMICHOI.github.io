---
title : \[JAVA] String 에서의 equals() 와 == 의 차이는 무엇인가?
date : 2021-12-16 00:00:00+0900
categories: [posts, java]
tags : [java]
toc: true
toc_sticky: ture
---

일반적으로 Java 에서의 equals() 와 "==" 연산자는 객체를 비교하여 같은지 여부를 확인하는 데 사용되지만, 차이점이 존재한다.

차이점을 정리하기 앞서, String 변수를 생성하는 두 가지 방식에 대해 먼저 짚고 넘어가려 한다.


## String 변수 생성하는 2가지 방식
1. 리터럴을 이용한 방식
2. new 예약어를 이용한 방식

위의 두 가지 방식에는 확실한 차이가 있다.

문자열 리터럴을 대입하여 변수를 생성하면, 해당 변수의 String 값은 Heap 영역 내 "String Constance Pool"에 할당 된다.   
이는 상수로 사용되기 때문에 여기에 할당되면 가변성(immutable)이 허용되지 않는다.

반면, new 예약어를 사용하여 문자열 변수를 생성하면 같은 내용이더라도 여러 개의 객체가 각각 Heap 메모리에 할당되며, 값의 변경이 가능해진다.

예를 들어,

**Main.java**
```java
package laura;

public class Main {
  public static void main(String[] args) {
    // 1. 리터럴을 이용한 방식
    String constantStr1 = "Hello";
    String constantStr2 = "Hello";
    
    System.out.println("constantStr1 주소값 : " + System.identityHashCode(constantStr1));
    System.out.println("constantStr2 주소값 : " + System.identityHashCode(constantStr2));
    
    // 2. new 예약어를 이용하여 객체를 생성하는 방식
    String heapStr1 = new String("Hello");
    String heapStr2 = new String("Hello");
    
    System.out.println("heapStr1 주소값 : " + System.identityHashCode(heapStr1));
    System.out.println("heapStr2 주소값 : " + System.identityHashCode(heapStr2));
  }
}
```

**출력 화면**   
![Two ways of declaring a string variable](/assets/images/2021-12-16-string-1.png)


리터럴 대입 방식을 통해서 선언한 문자열 변수들은 서로 같은 주소 값을 가지고 있었다.

반면, 생성자를 통해 선언한 문자열 변수는 같은 값을 가지고 있지만, 주소값은 서로 다르다는 것을 확인 할 수 있었다.

- String literal로 생성한 객체 constantStr1 의 값("Hello")이 Heap 영역 내 'String Constant Pool' 에 저장된다.
- String literal로 생성된 객체 constantStr2 의 값("Hello")이 이미 String Constant Pool에 존재하기 때문에, 해당 객체는 String Constant Pool의 reference를 참조한다.
- new 연산자로 생성된 String 객체(heapStr1, heapStr2) 는 같은 값("Hello")이 String Constant Pool 에 이미 존재하더라도, Heap 영역 내 별도의 객체를 가리키게 된다.


## equals() 와 == 의 차이
1. equals() 는 java.lang.Object 클래스 내부에 정의된 객체끼리의 내용을 비교하기 위한 메소드 이다.   
== 은 비교를 위한 연산자이며, Java 에서 등호 연산자를 사용하여 원시(Primitive) 및 객체(Object)를 모두 비교할 수 있다.

2. equals() 메소드는 객체의 내용을 비교하여 동등함을 확인한다.   
== 연산자는 비교하고자 하는 두 객체가 Heap 의 동일한 메모리 위치를 참조하는지 확인(주소값 비교)한다.

3. equals() 메소드를 재정의하고 객체의 동등성에 대한 기준을 정의할 수 있다.   
== 연산자의 동작을 변경할 수 없다.


앞서 말한 것 처럼 String 은 객체를 생성하는 방식에 따라 == 연산자 혹은 equals() 메소드를 사용할 때 결과가 다르게 나올 수 있다.

예를 들어,

**Main.java**
```java
package laura;

public class Main {
  public static void main(String[] args) {
    String str1 = new String("Hello");
    String str2 = new String("Hello");
    
    // equals()
    if (str1.equals(str2)) {
        System.out.println("str1.equals(str2) is true.");
    } else {
        System.out.println("str1.equals(str2) is false.");
    }
    
    // ==
    if (str1 == str2) {
        System.out.println("str1 == str2 is true.");
    } else {
        System.out.println("str1 == str2 is false.");
    }
  }
}
```

**출력 화면**   
![compare equality operator with equals method](/assets/images/2021-12-16-compare-equal-operator-1.png)


위와 같이 new 예약어를 이용한 문자열 변수 생성 방식은 Heap 메모리 내에 각각 다른 위치에 저장되기 때문에,   
값을 비교하는 equals() 메소드는 true를, 주소값을 비교하는 == 연산자는 false 를 반환하는 것을 확인할 수 있다.


## 기억해두어야 할 것
참고로, 사용자 정의 객체에서 equals() 메소드를 재정의하지 않으면, java.lang.Object 클래스의 기본 equals() 메소드에 정의된 참조나 메모리 주소만 비교하고 두 참조 변수가 동일한 객체를 가리키는 경우에만 true를 반환할 것이다.

따라서 사용자 정의 클래스에서 equals() 메소드 및 == 연산자는 모두 유사하게는 동작하지만 논리적으로 올바르지 않을 수 있으므로 사용자 정의 또는 도메인 객체에 대한 동등성 기준을 항상 정의해야 한다.


## reference

- [https://docs.oracle.com/javase/7/docs/api/java/lang/String.html](https://docs.oracle.com/javase/7/docs/api/java/lang/String.html){:target="_blank"}
- [https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html](https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html){:target="_blank"}
- [https://docs.oracle.com/javase/tutorial/java/nutsandbolts/op2.html](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/op2.html){:target="_blank"}
- [https://starkying.tistory.com/entry/what-is-java-string-pool](https://starkying.tistory.com/entry/what-is-java-string-pool){:target="_blank"}
- [https://youngjinmo.github.io/2021/01/two-ways-to-declare-string-variable/](https://youngjinmo.github.io/2021/01/two-ways-to-declare-string-variable/){:target="_blank"}
- [https://stackoverflow.com/questions/7520432/what-is-the-difference-between-and-equals-in-java](https://stackoverflow.com/questions/7520432/what-is-the-difference-between-and-equals-in-java){:target="_blank"}
- [https://www.geeksforgeeks.org/difference-equals-method-java/](https://www.geeksforgeeks.org/difference-equals-method-java/){:target="_blank"}
- [https://www.java67.com/2012/11/difference-between-operator-and-equals-method-in.html](https://www.java67.com/2012/11/difference-between-operator-and-equals-method-in.html){:target="_blank"}
