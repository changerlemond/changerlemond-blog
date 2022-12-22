---
title: "자바와 코틀린 차이로 공부하기 (5)"
date: 2022-12-21T22:41:35+09:00
tags: ['kotlin']
draft: false
---

자바와 코틀린 문법 차이 알아보기 (5)
<!--more--> 

### 5. 코틀린에서 조건문을 다루는 방법

#

#### (1) if문

**< Java >**

```java
class ifTest() {
    private void validateScoreIsNotNegative(int score) {
        if (score < 0) {
            throw new IllegalArgumentException(String.format("%score는 0보다 작을 수 없습니다.", score));
        }
    }
}
```

**< Kotlin >**

```kotlin
fun validateScoreIsNotNegative(score: Int) {
    if (score < 0) {
        throw IllegalArgumentException("${score}는 0보다 작을 수 없습니다")
    }
}
```

- 함수에서 unit(void) 생략 가능
- new를 사용하지 않고 예외를 바로 throw

<br>

**< Java >**

```java
class ifTest() {
    private String getPassOrFail(int score) {
        if (score >= 50) {
            return "P";
        } else {
            return "F";
        }
    }
}
```

**< Kotlin >**

```kotlin
fun getPassOrFail(score: Int): String {
    return if (score >= 50) {
        "P"
    } else {
        "F"
    }
}
```

#

#### (2) Expression & Statement

```
자바에서 if-else는 Statement, 코틀린에서는 Expression

Statement: 프로그램의 문장, 하나의 값으로 도출되지 않는다.
Expression: 히나의 값으로 도출되는 문장
```

코틀린에서는 if-else를 expression으로 사용할 수 있기 때문에 3항 연산자가 없다.

**< Java >**
```java
class ifTest() {
    private String getGrade(int score) {
        if (score >= 90) {
            return "A";
        } else if (score >= 80) {
            return "B";
        } else if (score >= 70) {
            return "C";
        } else {
            return "D";
        }
    }
}
```

**< Kotlin >**
```kotlin
fun getGrade(score: Int): String {
    return if (score >= 90) {
        "A"
    } else if (score >= 80) {
        "B"
    } else if (score >= 70) {
        "C"
    } else {
        "D"
    }
}
```

<br>

코틀린에서 어떤 값이 특정 범위에 포함되어 있는지, 아닌지 체크할 때는
```kotlin
if (0 <= score && score <= 100) {
    return score
}
```

```kotlin
if (score in 0..100) {
    return score
}
```

#

#### (3) switch와 when

**< Java >**
```java
class switchTest {
    private String getGradeWithSwitch(int score) {
        switch (score / 10) {
            case 9:
                return "A";
            case 8:
                return "B";
            case 7:
                return "C";
            default:
                return "D";
        }
    }   
}
```

*단 자바 14 버전부터 코틀린처럼 Expression 형식으로 쓸 수 있다.

**< Kotlin >**
```kotlin
fun getGradeWithSwitch(score: Int, ): String {
    return when (score / 10) {
        9 -> "A"
        8 -> "B"
        7 -> "C"
        else -> "D"
    }
}
```

```kotlin
fun getGradeWithSwitchV2(score: Int): String {
    return when (score) {
        in 90..99 -> "A"
        in 80..89 -> "B"
        in 70..79 -> "C"
        else -> "D"
    }
}
```

코틀린의 when은 Enum Class or Sealed Class와 함께 사용할 경우 더욱더 진가를 발휘한다.

