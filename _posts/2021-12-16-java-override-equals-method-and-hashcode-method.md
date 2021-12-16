---
title : \[JAVA] Object 에서의 equals()와 hashCode(), 그리고 재정의를 해야 하는 이유에 대하여
date : 2021-12-16 00:00:00+0900
categories: [posts, java]
tags : [java]
toc: true
toc_sticky: ture
---

equals() 와 hashCode() 메소드는 모든 Java의 객체의 부모 객체인 java.lang.Object 클래스에 정의되어 있다.   
그렇기 때문에 Java의 모든 객체는 Object 클래스에 정의된 equals() 와 hashCode() 메소드를 상속받고 있다.

## equals() 메소드
equals() 메소드는 두 객체의 내용이 동일한지 확인하는 메소드이다.

기본 메소드를 그대로 사용할 경우, 올바른 비교가 되지 않기 때문에 실제 사용 시에는 해당 클래스에 맞게 Overriding 해서 사용해주는 것이 좋다.

자바에서는 equals() 메소드를 Overriding 할 때 '반드시' 아래 5가지 조건을 충족시켜야 한다고 한다.

```markdown
- It is reflexive: for any non-null reference value x, x.equals(x) should return true.
- It is symmetric: for any non-null reference values x and y, x.equals(y) should return true if and only if y.equals(x) returns true.
- It is transitive: for any non-null reference values x, y, and z, if x.equals(y) returns true and y.equals(z) returns true, then x.equals(z) should return true.
- It is consistent: for any non-null reference values x and y, multiple invocations of x.equals(y) consistently return true or consistently return false, provided no information used in equals comparisons on the objects is modified.
- For any non-null reference value x, x.equals(null) should return false.
```

- 재귀(reflexive) : null 이 아닌 x 라는 객체의 x.equals(x) 결과는 항상 true 여야만 한다.
- 대칭(symmetric) : null 이 아닌 x 와 y 객체가 있을 때, y.equals(x)가 true를 항상 리턴했다면, x.equals(y)도 반드시 true를 리턴해야만 한다.
- 타동적(transitive) : null 이 아닌 x, y, z 가 있을 때 x.equals(y)가 true를 리턴하고, y.equals(z)가 true를 리턴하면, x.equals(z)는 반드시 true를 리턴해야만 한다.
- 일관(consistent) : null 이 아닌 x 와 y 가 있을 때 객체가 변경되지 않은 상황에서는 몇 번을 호출하더라도, x.equals(y)의 결과는 항상 true 이거나 항상 false 여야만 한다.
- null 과의 비교 : null 이 아닌 x 라는 객체의 x.equals(null) 의 결과는 항상 false 여야만 한다.


## hashCode() 메소드
hashCode() 메소드는 기본적으로 객체의 해시 코드 값을 정수로 리턴한다.   
만일, 어떤 두 개의 객체가 서로 동일하다면 hashCode() 값은 무조건 동일해야만 한다.   
하지만 hash 값이 동일하더라도 값이 다를 수 있기 때문에 equals() 메소드와 같이 Overriding 하여 사용하는 것이 좋다.

자바에서는 hashCode() 메소드를 Overriding 할 때 '반드시' 아래와 같은 조건을 충족시켜야 한다고 한다.

```markdown
- Whenever it is invoked on the same object more than once during an execution of a Java application, the hashCode method must consistently return the same integer, provided no information used in equals comparisons on the object is modified. This integer need not remain consistent from one execution of an application to another execution of the same application.
- If two objects are equal according to the equals(Object) method, then calling the hashCode method on each of the two objects must produce the same integer result.
- It is not required that if two objects are unequal according to the equals(java.lang.Object) method, then calling the hashCode method on each of the two objects must produce distinct integer results. However, the programmer should be aware that producing distinct integer results for unequal objects may improve the performance of hash tables.
```

- 자바 애플리케이션이 수행되는 동안에 어떤 객체에 대해서 이 메소드가 호출될 때에는 항상 동일한 int 값을 리턴해주어야 한다. 하지만, 자바를 실행할 때마다 같은 값이어야 할 필요는 없다.
- 어떤 두 개의 객체에 대하여 equals() 메소드를 사용하여 비교한 결과가 true 일 경우에, 두 객체의 hashCode() 메소드를 호출하면 동일한 int 값을 리턴해야만 한다.
- 두 객체를 equals() 메소드를 사용하여 비교한 결과 false를 리턴했다고 해서, hashCode() 메소드를 호출한 int 값이 무조건 달라야 할 필요는 없다. 하지만, 이 경우에 서로 다른 int 값을 제공하면 hashtable의 성능을 향상시키는 데 도움이 된다.


## equals() 와 hashCode() 를 재정의해야 하는 이유

Joshua Bloch는 Effective Java 에 대해 이렇게 말한다.

```markdown
You must override hashCode() in every class that overrides equals(). 
Failure to do so will result in a violation of the general contract for Object.hashCode(), which will prevent your class from functioning properly in conjunction with all hash-based collections, including HashMap, HashSet, and Hashtable.
```

equals() 메소드를 재정의하는 모든 클래스에서 hashCode() 메소드를 재정의해야 한다.   
그렇게 하지 않으면 Object.hashCode() 에 대한 일반 계약을 위반하게 되어 모든 클래스가 HashMap, HashSet 및 Hashtable을 포함한 모든 해시 기반 컬랙션과 함께 제대로 작동하지 못하게 된다.

여기서 왜 equals() 와 hashCode() 를 같이 정의해야 할까?


### Object 에서의 equals() 메소드

우선 Object를 매개변수로 하는 equals() 메소드의 소스를 보자.

![Object class equals() method](/assets/images/2021-12-16-object-equals-1.png)

this 와 매개변수 obj 를 == 연산자로 비교하는 것을 확인할 수 있다.

== 연산자는 주소 값을 비교하기 때문에 new 예약어로 생성된 객체의 경우 return 값이 false 가 나올 수 밖에 없다.

사람으로 예를 들자면 사람에겐 고유한 id 인 주민등록번호를 가지고 있고, 그 id의 값이 동일하다면 같은 사람이라고 볼 수 있다.   
아래 예시를 보자.


**Person.java**
```java
package laura;

public class Person {
    private int id;
    private String name;
    
    public Person(int id, String name) {
        this.id = id;
        this.name = name;
    }
    
    public int getId() {
        return id;
    }
    
    public void setId(int id) {
        this.id = id;
    }
    
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        this.name = name;
    }
}
```

**Main.java**
```java
package laura;

public class Main {
    public static void main(String[] args) {
        Person laura1 = new Person(1, "laura");
        Person laura2 = new Person(1, "laura");
        
        System.out.println(laura1.equals(laura2));
    }
}
```

**출력 화면**   
![the equals() method](/assets/images/2021-12-16-object-equals-2.png)

equals() 메소드를 재정의하지 않으면 위와 같이 java.lang.Object 클래스의 기본 equals() 메소드에 정의된 참조나 메모리 주소만 비교하여 false 가 나오는 것을 확인할 수 있다.

그럼 우선 Person 클래스에 equals() 메소드만 재정의 해보자.

### equals() 메소드 재정의

**Person.java**
```java
package laura;

import java.util.Objects;

public class Person {
    private int id;
    private String name;
    
    public Person(int id, String name) {
        this.id = id;
        this.name = name;
    }
    
    // Getter, Setter 생략
    
    @Override
    public boolean equals(Object o) {
        if (this == o)
            return true;
        if (o == null || getClass() != o.getClass())
            return false;
        Person person = (Person)o;
        return id == person.id && Objects.equals(name, person.name);
    }
}
```

**Main.java**
```java
package laura;

import java.util.HashSet;

public class Main {
    public static void main(String[] args) {
        Person laura1 = new Person(1, "laura");
        Person laura2 = new Person(1, "laura");
        
        HashSet<Person> set = new HashSet<>();
        set.add(laura1);
        set.add(laura2);
        
        System.out.println("laura1.equals(laura2) = " + laura1.equals(laura2));
        System.out.println("laura1.hashCode() = " + laura1.hashCode());
        System.out.println("laura2.hashCode() = " + laura2.hashCode());
        System.out.println("HashSet.size() = " + set.size());
    }
}
```

**출력 화면**   
![Override the equals() method](/assets/images/2021-12-16-object-equals-3.png)

위와 같이 equals() 메소드를 재정의하여 출력하면 결과 값이 true 가 나온다.   
하지만 laura1 과 laura2 는 동일한 사람이기 때문에 HashSet 에 담았을 때 사이즈가 1개가 나와야 하지만, 실제로는 2개로 다른 객체 취급을 하는 것을 확인할 수 있다.

이와 같이, equals() 메소드만 재정의하는 경우 HashSet 과 같이 해시를 이용한 자료구조(HashSet, HashMap, Hashtable)를 사용할 때 문제가 발생한다.

왜 이러한 문제가 발생할까?

hash 값을 사용하는 Collection 은 객체가 논리적으로 같은지를 비교할 때 아래와 같은 과정을 거친다.

![The process of determining whether an object is the same or not](/assets/images/2021-12-16-object-equals-5.png)

hashCode() 메소드의 리턴 값이 우선 일치하고 equals() 메소드의 리턴 값이 true 여야 논리적으로 같은 객체라고 판단한다.

위의 Main.java 의 main 메소드에서 HashSet 에 Person 객체를 추가할 때도 이와 같은 과정으로 중복 여부를 판단하고 HashSet에 추가됐다.   
다만, Person 클래스에서는 hashCode() 메소드가 재정의되어 있지 않아서 부모 클래스인 Object 클래스의 hashCode() 메소드가 사용 되었다.

Object 클래스의 hashCode() 메소드는 객체의 고유한 주소 값을 int 값으로 변환하기 때문에 객체마다 다른 값을 반환하게 된다.   
두 개의 Person 객체는 equals 로 비교를 하기 전에, 이미 hashCode() 메소드의 서로 다른 반환 값으로 인해서 각각 다른 객체로 판단된 것이다.

그럼 이번에는 hashCode() 메소드까지 재정의해보자.

### equals() 메소드와 hashCode() 메소드 재정의

**Person.java**
```java
package laura;

import java.util.Objects;

public class Person {
    private int id;
    private String name;
    
    public Person(int id, String name) {
        this.id = id;
        this.name = name;
    }
    
    // Getter, Setter 생략
    
    @Override
    public boolean equals(Object o) {
        if (this == o)
            return true;
        if (o == null || getClass() != o.getClass())
            return false;
        Person person = (Person)o;
        return id == person.id && Objects.equals(name, person.name);
    }
    
    @Override
    public int hashCode() {
        return Objects.hash(id, name);
    }
}
```

**Main.java**
```java
package laura;

import java.util.HashSet;

public class Main {
    public static void main(String[] args) {
        Person laura1 = new Person(1, "laura");
        Person laura2 = new Person(1, "laura");
        
        HashSet<Person> set = new HashSet<>();
        set.add(laura1);
        set.add(laura2);
        
        System.out.println("laura1.equals(laura2) = " + laura1.equals(laura2));
        System.out.println("laura1.hashCode() = " + laura1.hashCode());
        System.out.println("laura2.hashCode() = " + laura2.hashCode());
        System.out.println("HashSet.size() = " + set.size());
    }
}
```

**출력 화면**   
![Override the equals() method and hashCode() method](/assets/images/2021-12-16-object-equals-4.png)

위와 같이 equals() 메소드와 hashCode() 메소드를 같이 재정의하니 laura1 과 laura2를 동일한 hash 값을 반환하여 1개의 객체로 취급하는 것을 확인할 수 있다.

위와 같은 이유로, Java API 문서에서 설명한 것과 같이 equals() 메소드를 재정의할 때 hashCode() 메소드도 같이 정의해주는 것이 좋겠다.

## reference
- [https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html#equals(java.lang.Object)](https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html#equals(java.lang.Object)){:target="_blank"}
- [https://stackoverflow.com/questions/2265503/why-do-i-need-to-override-the-equals-and-hashcode-methods-in-java](https://stackoverflow.com/questions/2265503/why-do-i-need-to-override-the-equals-and-hashcode-methods-in-java){:target="_blank"}
- [https://memostack.tistory.com/232](https://memostack.tistory.com/232){:target="_blank"}
- [https://tecoble.techcourse.co.kr/post/2020-07-29-equals-and-hashCode/](https://tecoble.techcourse.co.kr/post/2020-07-29-equals-and-hashCode/){:target="_blank"}
