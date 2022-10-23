---
title: "예외처리"
date: 2022-06-06T23:04:00+09:00
tags: ['java']
draft: false
---

예외처리
<!--more-->

#
### 프로그램 오류
: 프로그램이 실행 중 어떤 원인에 의해서 오작동을 하거나 비상적으로 종료되는 경우가 있는데, 이러한 결과를 초래하는 원인

```
컴파일 에러: 컴파일 시에 발생하는 에러
런타임 에러: 실행 시에 발생하는 에러
논리적 에러: 실행은 되지만, 의도와 다르게 동작하는 것
```

```
에러(error): 프로그램 코드에 의해서 수습될 수 없는 심각한 오류
예외(exception): 프로그램 코드에 의해서 수습될 수 있는 다소 미약한 오류 
```

<div style="text-align:center">
    <img src="/images/java/basic-7-1.png" alt="exception-1" />
</div>
<div style="text-align:center">
    <img src="/images/java/basic-7-2.png" alt="exception-2" />
</div>

```
Exception클래스들: 사용자의 실수와 같은 외적인 요인에 의해 발생하는 예외
RuntimeException 클래스들: 프로그래머의 실수로 발생하는 예외
```


#
### 예외 처리하기 - try-catch문
```
예외처리 (exception handling)
정의: 프로그램 실행 시 발생할 수 있는 예외의 발생에 대비한 코드를 작성하는 것
목적: 프로그램의 비정상 종료를 막고 정상적인 실행상태를 유지하는 것
```

```java
try {
    // 예외가 발생할 가능성이 있는 문장들을 넣는다.
} catch (Exception1 e1) {
    // Exception1이 발생했을 경우, 이를 처리하기 위한 문장을 적는다.
} catch (Exception2 e2) {
    // Exception2가 발생했을 경우, 이를 처리하기 위한 문장을 적는다.
}
```

```
try 블럭 내에서 예외가 발생한 경우,
1. 발생한 예외와 일치하는 catch 블럭이 있는지 확인한다.
2. 일치하는 catch 블럭을 찾게 되면, 그 catch 블럭 내의 문장들을 수행하고 전체 try-catch문을 빠져나가서 그 다음 문장을 계속해서 수행한다.
   일치하는 catch블럭을 찾지 못하면 예외는 처리되지 못한다.

try블럭 내에서 예외가 발생하지 않은 경우,
catch블럭을 거치지 않고 전체 try-catch문을 빠져나가서 수행을 계속한다.
```


#
### printSTackTrace()와 getMessage()
```
printStackTrace(): 예외발생 당시의 호출스택(Call Stack)에 있었던 메서드의 정보와 예외 메시지를 화면에 출력한다.
getMessage(): 발생한 예외클래스의 인스턴스에 저장된 메시지를 얻을 수 있다.
```


#
### 멀티 catch 블럭
```java
try {
    ....
} catch (ExceptionA | ExceptionB e) {
    e.getMessage();
}
```


#
### 예외 발생시키기
```
1. 연산자 new를 이용해서 발생시키려는 예외 클래스의 객체를 만든 다음
2. 키워드 throw를 이용해서 예외를 발생시킨다.
```


#
### 메서드에 예외 선언하기
```java
void method() throws Exception {
    // 메서드의 내용
}
```


#
### finally블럭
```java
try {
    // 예외가 발생할 가능성이 있는 문장들을 넣는다.
} catch (Exception1 e1) {
    // 예외처리를 위한 문장을 적는다.
} finally {
    // 예외의 발생여부에 관계없이 항상 수행되어야 하는 문장들을 넣는다.
    // finally 블럭은 try-catch문의 맨 마지막에 위치해야 한다.
}
```


#
### 사용자 정의 예외 만들기
```java
class MyException extends Exception {
    MyException(String message) {  // 문자열을 매개변수로 받는 생성자
        super(message);            // 조상인 Exception클래스의 생성자를 호출한다.
    }
}
```