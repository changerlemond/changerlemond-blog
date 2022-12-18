---
title: "자바와 코틀린 차이로 공부하기 (2)"
date: 2022-12-18T20:09:58+09:00
tags: ['kotlin']
draft: false
---

자바와 코틀린 문법 차이 알아보기 (2)
<!--more--> 

### 1. 코틀린에서 null을 다루는 방법
#
#### (1) kotlin에서의 null 체크

```java
class nullTest() {
    public boolean startsWithA(String string) {
        return string.startsWith("A");
    }
}
```
위 코드는 안전한 코드라고 할 수 없다. NPE가 나게 된다.  
자바에서는 null 체크를 이렇게 3가지 방식으로 가능하다. 

<br>

1. string이 null일 경우 Exception을 발생시킨다.
```java
class nullTest() {
    public boolean startsWithA(String string) {
        if (string == null) {
            throw new IllegalArgumentException("null이 들어왔습니다.");
        }
        return string.startsWith("A");
    }
}
```

2. string이 null일 때 null을 반환한다.
```java
class nullTest() {
    public Boolean startsWithA(String string) {
        if (string == null) {
            return null;
        }
        return string.startsWith("A");
    }
}
```
3. string이 null일 경우 false를 반환한다.
```java
class nullTest() {
    public boolean startsWithA(String string) {
        if (string == null) {
            return false;
        }
        return string.startsWith("A");
    }
}
```

<br>

코틀린에서는 위의 자바 표현방식을 이렇게 표현할 수 있다.
1. 자바의 1번 방식
```kotlin
fun startsWithA1(string: String): Boolean {
    if (strilng == null) {
        throw IllegalArgumentException("null이 들어왔습니다.")
    }
    return string.startsWith("A")
}
```

2. 자바의 2번 방식
```kotlin
fun startsWithA2(string: String): Boolean? {
    if (string == null) {
        return null
    }
    return string.startsWith("A")
}
```

3. 자바의 3번 방식
```kotlin
fun startsWithA3(string: String?): Boolean {
    if (string == null) {
        return false
    }
    return string.startsWith("A")
}
```

<br>

```
kotlin에서는 null이 가능한 타입을 완전히 다르게 취급한다!
```  

<br>

#
#### (2) Safe Call과 Elvis 연산자
**null이 가능한 타입만을 위한 기능**
1. Safe Call: null이 아니면 실행하고, null이 아니면 실행하지 않는다. (?를 사용)
2. Elvis 연산자: 앞의 연산 결과가 null이면 뒤의 값을 사용한다. (?: 를 사용)

<br>

그럼 이 기능들을 사용해서 위 3개의 함수를 다시 코틀린 코드를 작성해보면?

<br>

```kotlin
fun startsWithA1(string: String?): Boolean {
    return string?.startsWith("A")
        ?: throw IllegalArgumentException("null이 들어왔습니다.")
}
```

```kotlin
fun startsWithA2(string: String?): Boolean? {
    return string?.startsWith("A")
}
```

```kotlin
fun startsWithA3(string: String?): Boolean {
    return string?.startsWith("A") ?: false
}
```

#
#### (3) null이 아님을 단언하는 경우
nullable type이지만 아무리 생각해도 null이 될 수 없는 경우에는 이렇게 쓸 수도 있다.  
정말 null이 아닌게 확실한 경우에만 사용한다. (혹시나 모를 NPE 방지)
```kotlin
fun startsWithA1(string: String): Boolean {
    return string!!.startsWith("A")
}
```


#
#### (4) 플랫폼 타입
코틀린에서 자바 코드를 가져다 쓸 경우에는 플랫폼 타입 사용에 유의해야 한다.  

@Nullable이나 @NotNull과 같은 어노테이션이 없을 경우 코틀린에서는 nullabled인지 non-nullable인지 알 수가 없다.  
코틀린에서는 null이 들어갈 수 있는 타입은 완전히 다르게 간주되기 때문에 이를 고려하지 않고 사용할 경우 Runtime 시 Exception이 발생할 수 있다.  

자바 코드를 읽으면서 null 가능성을 체크하고 코틀린으로 wrapping 해줘야 한다.
