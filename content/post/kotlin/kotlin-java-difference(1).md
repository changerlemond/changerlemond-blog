---
title: "자바와 코틀린 차이로 공부하기 (1)"
date: 2022-12-11T20:09:58+09:00
tags: ['kotlin']
draft: false
---

자바와 코틀린 문법 차이 알아보기 (1)
<!--more--> 

### 1. 변수 선언 키워드
#
#### (1) var과 val의 차이점

자바에서는 가변, 불변을 이렇게 표시한다.  


**Java**
```java
public class variableClass {
    long number1 = 10L;
    final long number2 = 10L;
}
```

코틀린에서는 모든 변수에 수정 가능 여부(var / val)를 명시해주어야 한다.  

**Kotlin**
```kotlin
var number1 = 10L
val number2 = 10L
```
- var: Variable의 약자 / "발"이라고 부른다.
- val: Value의 약자 / "밸"이라고 부른다.

타입을 쓰지 않아도 코틀린이 알아서 타입을 추론하지만, 명시적으로 작성해줄 수도 있다.  

**Kotlin**
```kotlin
var number1: Long = 10L
val number2: Long = 10L
```

초기값을 지정해주지 않는 경우는?
**Kotlin**
```kotlin
var number: Int
print(number) // 컴파일 에러 발생, Variable 'number' must be initialize
```

val 컬렉션에는 element를 추가할 수 있다.  
```
*TIP: 모든 변수는 우선 val로 만들고 꼭 필요한 경우 var로 변경한다.
```

#
#### (2) primitive type
**Java**
```java
public class primitiveClass {
    long number1 = 10L;
    Long number3 = 1_000L;
}
```
자바에서는 primitive type과 reference type이 존재하는데, 코틀린에서는 이러한 구분 없이 사용한다.


**Kotlin**
```kotlin
var number1: Long = 10L
val number2: Long = 10L
```
숫자, 문자, 불리언과 같은 몇몇 타입은 내부적으로 특별한 표현을 갖는데 이 타입들은 실행 시 primitive value로 표현되지만, 
코드에서는 평범한 클래스처럼 보인다.

```
즉 프로그래머가 boxing, unboxing을 고려하지 않아도 되도록 알아서 처리해준다. (성능상 문제되지 않는다.)
```

#
#### (3) nullable 변수
**Kotlin**
```kotlin
var number3: Long? = 1_000L
number3 = null // 가능
```
null이 변수에 들어갈 수 있다면 "타입?" 을 사용해야 한다. 아예 다른 타입으로 간주된다.


#
#### (4) 객체 인스턴스화
**Java**
```java
Person person = new Person("claire");
```

**Kotlin**
```kotlin
val person = Person("claire")
```

코틀린에서는 객체 인스턴스화를 할 때에는 new를 붙이지 않아야 한다.