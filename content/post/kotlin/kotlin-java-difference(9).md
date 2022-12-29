---
title: "자바와 코틀린 차이로 공부하기 (9)"
date: 2022-12-25T23:11:40+09:00
tags: ['kotlin']
draft: false
---

자바와 코틀린 문법 차이 알아보기 (9)
<!--more--> 

### 9. 코틀린에서 클래스를 다루는 방법

#

#### (1) 클래스와 프로퍼티

**< Java >**

```java
class Person {

    private final String name;

    private int age;

    public JavaPerson(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

}
```

**< Kotlin >**

```kotlin
class Person(name: String, age: Int) {
    val name = name
    val age = age
}

val person = Person("클레어", 20)
println(person.name)
person.age = 10
println(person.age)
```

클래스의 필드 선언과 생성자를 동시에 선언할 수 있다.  
.필드를 통해 getter, setter 를 바로 호출할 수 있다.

#

#### (2) 생성자와 init

**< Java >**

```java
class Person {
    public Person(String name, int age) {
        if (age <= 0) {
            throw new IllegalArgumentException(String.format("나이(%s)는 1이상이어야 합니다.", age));
        }
        this.name = name;
        this.age = age;
    }
}
```

**< Kotlin >**

```kotlin
class Person(
    val name: String,
    age: Int,
) {
    init {
        if (age < 0) {
            throw IllegalArgumentException("나이는 ${age}일 수 없습니다.")
        }
    }
}
```

init (초기화) 블록은 생성자가 호출되는 시점에 호출된다.  
값을 적절히 만들어주거나, validation 하는 로직을 여기서.

최초로 태어나는 사람은 무조건 1살이니까 생성자를 하나 만들어준다면

**< Java >**

```java
class Person {
    public JavaPerson(String name) {
        this(name, 1);
    }
}
```

**< Kotlin >**

```kotlin
class Person(
    val name: String,
    age: Int,
) {
    init {
        if (age < 0) {
            throw IllegalArgumentException("나이는 ${age}일 수 없습니다.")
        }
    }

    constructor(name: String) : this(name, 1)
}
```

constructor 로 생성자를 추가한다, (this(name, 1)로 위에 있는 생성자를 호출한다.)

주생성자는 반드시 존재해야 한다. 단 파라미터가 하나도 없을 땐 생략할 수 있다.  
부생성자는 있을 수도 있고, 없을 수도 있다.  
최종적으로 주생성자를 this 로 호출해야 하며, body 를 가질 수 있다.

본문은 역순으로 실행된다!

***부생성자보다는 default parameter 를 권장한다.**  
**converting 과 같은 경우 부생성자보다는 정적 팩토리 메소드를 추천!**

#

#### (3) 커스텀 getter, setter

**< Java >**

```java
class getterTest {
    public boolean isAdult() {
        return this.age >= 20;
    }
}
```

**< Kotlin >**

```kotlin
fun isAdult(): Boolean {
    return this.age >= 20
}
```

커스텀 getter 로 만들 수 있다.

```kotlin
val isAdult: Boolean
    get() {
        return this.age >= 20
    }
```

모두 동일한 기능이고 표현 방법에만 차이가 있다.  
객체의 속성이라면 customer getter, 그렇지 않다면 함수 추천!

customer getter 를 사용하면 자기 자신을 변형해줄 수 있다.

```kotlin
class Person(
    name: String,
    var age: Int,
) {
    val name: String = name
        get() = field.uppercase()
}
```

주생성자에서 받은 name 을 불변 프로퍼티인 name 에 바로 대입.  

#

#### (4) backing field

여기서 field 는?  
무한 루프를 막기 위한 예약어로 자기 자신을 가리킨다.  

이를 backing field 라고 부른다.