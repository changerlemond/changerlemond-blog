---
title: "자바와 코틀린 차이로 공부하기 (6)"
date: 2022-12-22T18:51:56+09:00
tags: ['kotlin']
draft: false
---

자바와 코틀린 문법 차이 알아보기 (6)
<!--more--> 


### 6. 코틀린에서 반복문을 다루는 방법

#

#### (1) for-each문

**< Java >**

```java
class forEachTest() {
    private void forEach() {
        List<Long> numbers = Arrays.asList(1L, 2L, 3L);
        for (long number : numbers) {
            System.out.println(number);
        }   
    }
}
```

**< Kotlin >**

```kotlin
val numbers = listOf(1L, 2L, 3L)
for (number in numbers) {
    println(number)
}
```

자바와 동일하게 Iterable이 구현된 타입이라면 numbers에 모두 들어갈 수 있다.

#

#### (2) 전통적인 for문


**< Java >**

```java
class forTest() {
    private void test() {
        for (int i = 1; i <=3; i++ ) {
            System.out.println(i);
        }
    }
}
```

**< Kotlin >**

```kotlin
for (i in 1..3) {
    println(i)
}
```

<br>

숫자를 반대로 출력하고 싶다면

**< Java >**

```java
class forTest() {
    private void test() {
        for (int i = 1; i <=3; i-- ) {
            System.out.println(i);
        }
    }
}
```

**< Kotlin >**

```kotlin
for (i in 3 downTo 1) {
    println(i)
}
```

<br>

2칸씩 올라가는 경우라면

**< Java >**

```java
class forTest() {
    private void test() {
        for (int i = 1; i <=5; i+=2 ) {
            System.out.println(i);
        }
    }
}
```

**< Kotlin >**

```kotlin
for (i in 1..5 step 2) {
    println(i)
}
```

#

#### (3) Progression과 Range


```
.. 연산자: 범위를 만들어 내는 연산자
1..3: 1부터 3의 범위

IntRange -> IntProgression (시작 값부터 끝 값까지 공차로 등차수열)
등차수열을 만들어주고 있는 것이다!
```

```
여기서 downTo, step도 함수이다. (중위 호출 함수)
```

#

#### (4) While문

```java
class whileTest {
    private void test() {
        int i = 1;
        while (i <= 3) {
            System.out.println(i);
            i++;
        }
    }
}
```

```kotlin
var i = 1
while (i <=3) {
    println(i)
    i++
}
```

자바와 완전 동일하다!