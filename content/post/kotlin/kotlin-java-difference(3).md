---
title: "자바와 코틀린 차이로 공부하기 (3)"
date: 2022-12-19T14:40:00+09:00
tags: ['kotlin']
draft: false
---

자바와 코틀린 문법 차이 알아보기 (3)
<!--more--> 

### 3. 코틀린에서 Type을 다루는 방법
#
#### (1) 기본 타입

코틀린에서는 선언된 기본값을 보고 타입을 추론한다.

```
Java에서는 기본 타입간의 변환은 암시적으로 이루어질 수 있지만,
Kotlin에서는 기본 타입간의 변환은 명시적으로 이루어져야 한다.
```

```java
class type() {
    int number1 = 4;
    long number2 = number1;
    
    System.out.println(number1 + number2);
}
```

```kotlin
val number1 = 4
val number2: Long = number1 // Type miss match

val number2: Long = number1.toLong() // to변환타입()을 사용해야 한다.
```

<br>

변수가 nullable이라면 적절한 처리가 필요하다.

#
#### (2) 타입 캐스팅
기본 타입이 아닌 일반 타입의 경우

< Java >
```java
public static void printAgeIfPerson(Object object) {
    if (object instanceof Person) {
        Person person = (Person) object;
        System.out.println(person.getAge());
    }
}
```

< Kotlin >
```Kotlin
fun printAgeIfPerson(object: Any) {
    if (object is Person) {             // is: Java의 instanceof
        val person = object as Person   // as Person: Java의 (Person) object
        println(person.age)
    }
}
```

```
value is Type: value가 Type이면 true, 아니면 false 반환
value !is Type:  value가 Type이 아니면 true, Type이면 false 반환

value as Type: value가 Type이면 Type으로 타입 캐스팅, 아니면 예외 발생
value as? Type: value가 Type이면 Type으로 타입 캐스팅, value가 null이거나 Type이 아니면 null
```

#
#### (3) Kotlin의 특이한 타입 3가지
`Any, Unit, Nothing`

1. Any
- Java의 Object 역할 (모든 객체의 최상위 타입)
- 모든 Primitive TYpe의 최상의 타입도 Any
- Any 자체로는 null을 포함할 수 없어 null을 포함하고 싶다면 Any?로 표현
- Any에 equals / hasCode / toString 존재

2. Unit
- Java의 void와 동일한 역할
- void와 다르게 Unit은 그 자체로 타입 인자로 사용 가능
- 함수형 프로그래밍에서 Unit은 단 하나의 인스턴스만 갖는 타입을 의미. 즉 코틀린 Unit은 실제 존재하는 타입이라는 것을 표현

3. Nothing
- 함수가 정상적으로 끝나지 않았다는 사실을 표현하는 역할
- 무조건 예외를 반환하는 함수 / 무한 루프 함수 등


#
#### (4) String interpolation / String indexing
```
${변수} 를 사용하면 값이 들어간다. 
혹은 $변수 를 사용할 수도 있다.

TIP: 가독성, 일괄 변환, 정규식 활용 측면을 고려하면 ${변수} 를 사용하는 것이 좋다.
```

문자열의 특정 문자 가져오기

< Java >
```java
String string = "ABCDE"
char ch = string.charAt(1);
```

< Kotlin >
```kotlin
val string = "ABCDE"
val ch = string[1]
```

