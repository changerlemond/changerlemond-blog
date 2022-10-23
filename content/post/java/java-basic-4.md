---
title: "배열"
date: 2022-05-29T19:40:00+09:00
tags: ['java']
draft: false
---

배열
<!--more-->

#
### 배열이란?
같은 타입의 여러 변수를 하나의 묶음으로 다루는 것


#
### 배열 선언

| 선언방법         | 선언 예                           |
|--------------|--------------------------------|
| 타입[ ] 변수이름;  | int [ ] score; String[ ] name; |
| 타입 변수이름 [ ]; | int score[ ]; String name[ ];  |

#
### 배열의 선언
```
타입[ ] 변수 이름;            // 배열을 선언(배열을 다루기 위한 참조 변수 선언)
변수 이름 = new 타입[길이];    // 배열을 생성(실제 저장공간을 생성)
```

#
### 배열의 인덱스
```
인덱스의 범위는 0부터 배열 길이 -1까지
```

#
### 배열의 길이
```
배열이름.length
```

#
### 배열의 출력


1. for문 이용

```java
int [ ] array = { 100, 90, 85, 10, 20};

for(int i=0; i<array.length; i++) {
    System.out.println(array[i]);
}
```


2. Arrays.toString() 이용

```java
int[ ] array = { 100, 90, 85, 10, 20 };
System.out.println(Arrays.toString(array));
```

<br/>

*바로 배열을 출력하는 경우
```java
System.out.println(array);
// [I@14318bb와 같은 형식의 문자열이 출력된다.]
```

#
#### String 배열의 선언과 생성
```java
String[ ] name = new String[3];
```


*변수의 타입 기본값

| 자료형              | 기본값         |
|------------------|-------------|
| boolean          | false       |
| char             | '\u0000'    |
| byte, short, int | 0           |
| long             | 0L          |
| float            | 0.0f        |
| double           | 0.0d 또는 0.0 |
| 참조형              | null        |

```
String 클래스는 char배열에 기능(메서드)을 추가한 것이다.
```

#
#### String 클래스의 주요 메서드

| 메서드                                | 설명                                    |
|------------------------------------|---------------------------------------|
| char charAt(int index)             | 문자열에서 해당 위치에 있는 문자를 반환한다.             |
| int length( )                      | 문자열의 길이를 반환한다.                        |
| String substring(int from, int to) | 문자열에서 해당 범위의 문자열을 반환한다. (to는 포함 X)    |
| boolean equals(Object object)      | 문자열의 내용이 같은지 확인하고 같으면 true, 다르면 false |
| char[ ] toCharArray( )             | 문자열을 문자배열(char[ ] )로 변환해서 반환한다.       |

#
### 2차열 배열의 선언

| 선언 방법            | 선언 예               |
|------------------|--------------------|
| 타입 [ ] [ ] 변수이름; | int [ ] [ ] score; |
| 타입 변수이름 [ ] [ ]; | int score [ ] [ ]; |
| 타입[ ] 변수이름 [ ] ; | int[ ]  score[ ];  |

```
int[ ][ ]score = new int[4][3]; // 4행 3열의 2차원 배열을 생성한다.
배열을 생성하면 배열의 각 요소에는 배열 요소 타입의 기본값이 자동적으로 저장된다.
```

#
### Arrays로 배열 다루기
배열의 비교와 출력 - equals( ), toString( )
- toString( ): 모든 요소를 문자열로 편하게 출력할 수 있다.
일차원 배열에만 사용할 수 있고, 다차원 배열에는 deepToString( )을 사용해야 한다.
- equals( ): 두 배열에 저장된 모든 요소를 비교해서 같으면 true, 다르면 false를 반환
일차원 배열에만 사용할 수 있고, 다차원 배열 비교에는 deepEquals( )를 사용해야 한다.

<br/>

배열의 복사 - copyOf(), copyOfRange( )
- copyOf( ): 배열 전체를 복사해서 새로운 배열을 만들어 반환한다.
- copyOfRange( ): 배열의 일부를 복사해서 새로운 배열을 만들어 반환한다.

<br/>

배열의 정렬 - sort( )