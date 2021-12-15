---
title : \[JAVA] 접근 제어자란? 그리고 접근 제어자를 사용하는 이유는 무엇인가?
date : 2021-12-15 00:00:00+0900
categories: [posts, java]
tags : [java]
toc: true
toc_sticky: ture
---

접근 제어자란? 그리고 접근 제어자를 사용하는 이유는 무엇인가?   
자바의 신 1권을 읽으면서 문득 이런 의문이 들었다.

그래서, 찾아봤다. 접근 제어자는 무엇이고, 접근제어자를 사용하는 이유에 관해서.


## 접근 제어자 (Access Modifier) 란?
접근제어자는 클래스, 메소드, 변수의 접근 수준을 설정하는데 도움이 된다.   
클래스, 메소드, 인스턴스 및 클래스 변수를 선언할 때 사용된다.


## 접근 제어자의 종류
Java 에는 다음과 같이 네 가지의 접근 제어자가 있다.

- public   
  누구나 접근 가능 (같은 프로젝트 내에서 사용 가능)

- protected   
  같은 패키지 내에 있거나 상속 받은 경우에만 사용 가능

- package-private (접근제어자 없음)   
  같은 패키지 내에서만 접근 가능

- private   
  같은 클래스 내에서만 접근 가능

접근 권한은 **public > protected > (package-private) > private 순**으로 보다 넓은 범위의 접근을 허용하며, 간략하게 표로 정리하면 다음과 같다.

|   | 해당 클래스 안에서 | 같은 패키지 안에서 | 상속 받은 클래스에서 | import한 클래스에서 |   
| :--- | :---: | :---: | :---: | :---: |  
| public | O | O | O | O |
| protected | O | O | O | X |
| (package-private) | O | O | X | X |
| private | O | X | X | X |


### public
메서드, 변수, 클래스 등이 public 으로 선언되면 어디서나 접근할 수 있다.   
public 접근 제어자에는 범위 제한이 없다.

예를 들어,

**Animals.java**
``` java
package laura.animal;

public class Animal {
    // public 변수
    public int legCount;
    
    // public 메소드
    public void display() {
        System.out.println("I am an animal.");
        System.out.println("I have " + legCount + " legs.");
    }
}
```

**Main.java**
``` java
package laura;

import laura.animal.Animal;

public class Main {
    public static void main(String[] args) {
        // public 클래스에 접근
        Animal animal = new Animal();
    
        animal.legCount = 4;	// public 변수에 접근
        animal.display();	// public 메소드에 접근
    }
}
```

**출력 화면**   
![public access modifier](/assets/images/2021-12-15-access-modifier-public-1.png)

위와 같이,
- public 클래스인 Animal 은 Main 클래스에서 접근된다.
- public 변수인 legCount 는 Main 클래스에서 접근된다.
- public 메소드인 display() 는 Main 클래스에서 접근된다.

게다가 다른 패키지 안에 있기 때문에 import 하여 접근 가능한 것도 확인할 수 있다.


### protected
메소드와 데이터 멤버가 protected로 선언되면, 같은 패키지 내에서 뿐만 아니라 하위 클래스에서도 접근할 수 있다.

예를 들어,

**Animal.java**
```java
package laura.animal;

public class Animal {
	// protected 메소드
	protected void display() {
		System.out.println("I am an animal.");
	}
}
```

**Dog.java**
```java
package laura.animal;

public class Dog extends Animal {
	public static void main(String[] args) {
		Dog dog = new Dog();    // Dog 클래스의 객체 생성
		dog.display();    // protected 메소드 접근
	}
}
```

**출력 화면**   
![protected access modifier](/assets/images/2021-12-15-access-modifier-protected-1.png)

위의 예시에서 Animal 클래스 내부에 display() 라는 protected 메소드를 가지고 있다.   
그리고 Animal 클래스는 Dog 클래스에 의해 [상속](https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html){:target="_blank"}
된다.

그런 다음 Dog 클래스의 dog 객체를 생성했다. 이 객체를 사용해서 부모 클래스의 protected 메소드에 접근하려고 했다.

보호된 메소드는 자식 클래스에서 접근할 수 있으므로, Dog 클래스에서 Animal 클래스의 메소드에 접근할 수 있다.

그렇다면 다른 패키지에서 Animal 클래스의 display() 메소드를 호출하면 어떻게 될까?

**Main.java**
```java
package laura;

import laura.animal.Animal;

public class Main {
	public static void main(String[] args) {
		Animal animal = new Animal();
		animal.display();    // protected 메소드에 접근
	}
}
```

**출력 화면**   
![protected access modifier error](/assets/images/2021-12-15-access-modifier-protected-2.png)

위와 같이 Animal 클래스에 보호된 접근이 있다는 오류 메시지를 보여주는 것을 확인할 수 있다.


### package-private (접근제어자 없음)
default 접근 제어자라고도 하며, 클래스나 메소드, 변수 등에 대한 접근 제어자를 명시적으로 지정하지 않을 때를 말한다.

예를 들어,

**Logger.java**
```java
package laura.util;

class Logger {
	void message() {
		System.out.println("This is a message");
	}
}
```

**PackagePrivateExample.java**
```java
package laura.util;

public class PackagePrivateExample {
	public static void main(String[] args) {
		Logger logger = new Logger();    // default 클래스 호출
		logger.message();    // default 메소드 호출
	}
}
```

**출력 화면**   
![package-private access modifier](/assets/images/2021-12-15-access-modifier-package-private-1.png)

여기에서 Logger 클래스에는 default 접근 제어자가 있다. 그리고 이 클래스에서는 util 패키지에 속한 모든 클래스를 볼 수 있다.   
그러나 util 외부의 다른 클래스에서 Logger 클래스를 사용하려고 하면 컴파일 오류가 발생한다.

아래와 같이 말이다.

**Main.java**
```java
package laura;

import laura.util.Logger;

public class Main {
	public static void main(String[] args) {
		Logger logger = new Logger();
		logger.message();
	}
}
```

**출력 화면**   
![package-private access modifier error](/assets/images/2021-12-15-access-modifier-package-private-2.png)

Logger 클래스에 아무런 접근 제어자를 적어주지 않았기 때문에, 다른 패키지에 있는 Main 클래스에서 Logger 클래스를 호출할 경우 위와 같이 import 선언부 부터 오류가 발생한다.


### private
변수와 메소드가 private로 선언되면 클래스 외부에서 접근할 수 없다.

예를 들어,

**Main.java**
```java
package laura;

public class Main {
	public static void main(String[] args) {
		// laura 객체 생성
		Person laura = new Person();

		// 다른 클래스에서 private 변수 및 필드에 접근
		laura.name = "Laura";
	}
}

class Person {
	private String name;
}
```

**출력 화면**   
![private access modifier error](/assets/images/2021-12-15-access-modifier-private-1.png)

위의 예시에서 name 이라는 private 변수를 선언했고, 프로그램을 실행하니 위와 같은 오류가 발생했다.   
Main 클래스에서 Person 클래스의 private 메소드에 접근하려고 했기 때문이다.

이러한 private 변수에 접근하려면 어떻게 해야할까?   
이럴 경우에는 getter, setter 메소드를 사용할 수 있다.

예를 들어,

**Main.java**
```java
package laura;

public class Main {
	public static void main(String[] args) {
		// laura 객체 생성
		Person laura = new Person();

		// getter 과 setter 메소드를 이용하여 private 변수에 접근
		laura.setName("Laura");
		System.out.println(laura.getName());
	}
}

class Person {
	private String name;

	// getter 메소드
	public String getName() {
		return name;
	}

	// setter 메소드
	public void setName(String name) {
		this.name = name;
	}
}
```

**출력 화면**   
![private access modifier with getter, setter](/assets/images/2021-12-15-access-modifier-private-2.png)

위의 예시에서는 외부 클래스에서 name 이라는 private 변수에 접근하기 위해서 getName() 및 setName() 메소드를 사용했다.   
이러한 메소드를 Java에서는 getter, setter 라고 한다.

여기서는 setter 메소드(setName()) 를 사용하여 변수의 값을 할당하고 getter 메소드(getName())를 사용하여 변수에 접근했다.


## 접근 제어자는 왜 필요할까?
중요한 계산을 하는 로직을 어떤 메소드에 만들었다고 가정해보자.   
그 메소드는 클래스 내에서만 호출해서 사용할 수 있고, 다른 클래스에서 절대 가져다쓰면 안된다면?

이럴 때 (외부에서 접근하지 못하도록 설정하기 위해) 사용하는 것이 접근 제어자이다.   
public으로 선언했다면, 누구나 그 메소드를 사용해서 계산을 했을 것이다.

이처럼 어떤 메소드를 구현했는데 다른 개발자들이 그 메소드를 마음대로 호출하여 사용하면 안될 경우, 접근 제어자로 통제하여 데이터를 보호할 수 있다.   
마치 은행에서 누구나 접근 가능한 창구와 관계자 외에는 출입이 엄격하게 통제되는 금고를 구분하는 것과 마찬가지로 말이다.

변수의 경우, 직접 접근해서 변수를 변경하지 못하게 하고 꼭 Getter, Setter 메소드를 통해서 변경 혹은 조회만 할 수 있도록 할 때 접근 제어자를 많이 사용한다.

자바에서는 접근 수준 선택을 위한 Tip 을 다음과 같이 설명한다.

```markdown
If other programmers use your class, you want to ensure that errors from misuse cannot happen. Access levels can help
you do this.

* Use the most restrictive access level that makes sense for a particular member. Use private unless you have a good
  reason not to.
* Avoid public fields except for constants. (Many of the examples in the tutorial use public fields. This may help to
  illustrate some points concisely, but is not recommended for production code.) Public fields tend to link you to a
  particular implementation and limit your flexibility in changing your code.
```

해석해보자면,
- 특정 구성원에 대해 가장 제한적인 액세스 수준을 사용하라. 정당한 사유가 없는 한 private 를 사용해라.
- 상수를 제외한 public 필드를 피하라. (튜토리얼의 많은 예시들이 public 필드를 사용한다. 이것은 일부 요점들을 간결하게 설명하는 데 도움이 될 수 있지만, 프로덕션 코드에는 권장되지 않는다.)
  public 필드는 특정 구현에 연결하는 경향이 있으며, 코드 변경 시 유연성을 제한한다.


이렇게 가능한 private 수준의 접근 제어자를 사용하는 것을 권장한다.


## reference
- [https://docs.oracle.com/javase/tutorial/java/javaOO/accesscontrol.html](https://docs.oracle.com/javase/tutorial/java/javaOO/accesscontrol.html){:target="_blank"}
- [https://www.programiz.com/java-programming/access-modifiers](https://www.programiz.com/java-programming/access-modifiers){:target="_blank"}