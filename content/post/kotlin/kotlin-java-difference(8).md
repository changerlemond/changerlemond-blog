---
title: "자바와 코틀린 차이로 공부하기 (8)"
date: 2022-12-24T16:40:10+09:00
tags: ['kotlin']
draft: false
---

자바와 코틀린 문법 차이 알아보기 (8)
<!--more--> 


### 8. 코틀린에서 함수를 다루는 방법

#

#### (1) 함수 선언 문법

**< Java >**

```java
class functionTest {
    public int max(int a, int b) {
        if (a > b) {
            return a;
        }
         return b;
    }
}
```

**< Kotlin >**

```kotlin
fun max(a: Int, b: Int): Int {
    return if (a > b) {
        a
    } else {
        b
    }
}
```

함수가 하나의 결과값이면 block 대신 = 사용 가능
```kotlin
fun max(a: Int, b: Int): Int =
    if (a > b) {
        a
    } else {
        b
    }
```

한 줄로 변경 가능
````kotlin
fun max(a: Int, b: Int): Int = if (a > b) a else b
````

= 을 사용하는 경우 반환 타입 생략 가능
```kotlin
fun max(a: Int, b: Int) = if (a > b) a else b
```

block 을 사용하는 경우에는 반환 타입이 Unit 이 아니면 명시적으로 작성해야 한다.

코틀린의 경우 함수는 클래스 안에 있을 수도 있고, 파일 최상단에 있을 수도 있다.

#

#### (2) default parameter


**< Java >**

```java
class defaultTest {
    public void repeat(String string, int number, boolean useNewLine) {
        for (int i = 1; i < num; i++) {
            if (useNewLine) {
                System.out.println(string);   
            } else {
                System.out.print(string);
            }
        }
    }
}
```

많은 코드에서 useNewLine 에 true 를 사용한다면?  
자바의 오버로딩 사용!

**< Java >**

```java
class defaultTest {
    public void repeat(String string, int number) {
        repeat(string, number, true);
    }
}
```

출력을 3번씩 사용하고 있다면?

**< Java >**

```java
class defaultTest {
    public void repeat(String string, int number) {
        repeat(string, 3, true);
    }
}
```

함수가 3개 존재!

**< Kotlin >**

```kotlin
fun repeat(string: String, number: Int = 3, useNewLine: Boolean = true) {
    for (i in 1..number) {
        if (useNewLine) {
            println(string)
        } else {
            print(string)
        }
    }
}
```

코틀린에서도 오버로드 기능은 있다.


#

#### (3) named argument

만약 위 코드에서 number 3은 그대로 쓰고, useNewLine 을 false 로 쓰고 싶다면,

**< Kotlin >**

```kotlin
repeat("Hello World", useNewLine = false)
```
매개변수 이름을 통해 직접 지정하고, 지정하지 않은 매개변수는 기본값을 사용!   
builder 를 직접 만들지 않고 builder 의 장점을 가지게 된다.

단 코틀린에서 자바 함수를 가져다 사용할 때는 named argument 를 사용할 수 없다.


#

#### (4) 같은 타입의 여러 파라미터 받기

**< Java >**

```java
class printAllTest {
    public static void printAll(String... strings) {
        for (String string : strings) {
            System.out.println(string);
        }
    }
}
```

**< Kotlin >**

```kotlin
fun printAll(vararg strings: String) {
    for (string in strings) {
        println(string)
    }
}
```

...을 타입 뒤에 쓰는 대신 제일 앞에 vararg 를 적어주어야 한다.


**< Java >**

```java
class arrayTest {
    public static test() {
        String[] array = new String[]{"A", "B", "C"};
        printAll(array);
        
        printAll("A", "B", "C");
    }
}
```
**< Kotlin >**

```kotlin
val array = arrayOf("A", "B", "C")
printAll(*array)

printAll("A", "B", "C")
```

배열을 바로 넣는 대신 스프레드 연산자인 * 를 붙여주어야 한다.
