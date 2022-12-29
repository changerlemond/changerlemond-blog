---
title: "자바와 코틀린 차이로 공부하기 (7)"
date: 2022-12-23T23:52:06+09:00
tags: ['kotlin']
draft: false
---

자바와 코틀린 문법 차이 알아보기 (7)
<!--more--> 


### 7. 코틀린에서 예외를 다루는 방법

#

#### (1) try catch finally 구문

**< Java >**

```java
class tryCatchTest {
    private int parseIntOrThrow(@NotNull String string) {
        try {
            return Integer.parseInt(string);
        } catch (NumberFormatException exception) {
            throw new IllegalArgumentException(String.format("주어진 %s는 숫자가 아닙니다.", string));
        }
    }
}
```

**< Kotlin >**

```kotlin
fun parseIntOrThrow(string: String): Int {
    try {
        return string.toInt()
    } catch (exception: NumberFormatException) {
        throw IllegalArgumentException("주어진 ${string}는 숫자가 아닙니다.")
    }
}
```

예외를 던질 때 new 를 사용하지 않는다.

**< Java >**

```java
class tryCatchTest {
    private Integer parseIntOrThrowV2(String string) {
        try {
            return Integer.parseInt(string);
        } catch (NumberFormatException exception) {
            return null;
        }
    }
}
```

**< Kotlin >**

```kotlin
fun parseIntOrThrowV2(string: String): Int? {
    return try {
        string.toInt()
    } catch (exception: NumberFormatException) {
        null
    }
}
```

try - catch 구문 역시 expression 이므로 위와 같이 처리 가능하다.
#

#### (2) Checked Exception 과 Unchecked Exception


**< Java >**

```java
class fileTest {
    public void readFile() throws IOException {
        File currentFile = new File(".");
        File file = new File(currentFile.getAbsolutePath() + "/a.txt");
        BufferedReader reader = new BufferedReader(new FileReader(file));
        System.out.println(reader.readLine());
        reader.close();
    }
}
```

**< Kotlin >**

```kotlin
fun readFile() {
    val currentFile = File(".")
    val file = File(currentFile.absolutePath + "/a.txt")
    val reader = BufferedReader(FileReader(file))
    println(reader.readLine)
    reader.close()
}
```

코틀린에서는 Checked Exception 과 Unchecked Exception 을 구분하지 않는다.  
모두 Unchecked Exception 이다.

#

#### (3) try with resources

```java
class fileTest {
    public void readFile(String path) throws IOException {
        try (BufferedReader reader = new BufferedReader(new FileReader(path))) {
            System.out.println(reader.readLine());
        }
    }
}
```

```kotlin
fun readFile(path: String) {
    BufferedReader(FileReader(path)).use { reader -> {
        println(reader.readLline())
    }}
}
```

코틀린에는 try with resources 구문이 없다. 대신 use 라는 inline 확장함수를 사용해야 한다.