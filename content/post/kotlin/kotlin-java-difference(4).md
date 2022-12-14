---
title: "자바와 코틀린 차이로 공부하기 (4)"
date: 2022-12-20T20:30:22+09:00
tags: ['kotlin']
draft: false
---

자바와 코틀린 문법 차이 알아보기 (4)
<!--more--> 

### 4. 코틀린에서 연산자를 다루는 방법
#
#### (1) 단항 연산자 / 산술 연산자

단항 연산자: ++ --  
산술 연산자: + - * / %  
산술대입 연산자: += -= *= /= %=  

**자바와 완전 동일!**

#
#### (2) 비교 연산자와 동등성, 동일성
자바와 사용법은 동일!

단 자바와는 다르게 객체를 비교할 때 비교 연산자를 사용하게 되면 자동으로 compareTo를 호출해준다.

<br>

```
동등성(Equality): 두 객체의 값이 같은가?!
동일성(Identity): 완전히 동일한 객체인가?! 즉 주소가 같은가?!
```

<br>

```
자바에서는 동일성에 ==를 사용, 동등성에 equals를 직접 호출  
코틀린에서는 동일성에 ===를 사용, 동등성에 ==를 호출. ==를 사용하면 간접적으로 equals를 호출해준다.
```
#
#### (3) 논리 연산자와 코틀린에 있는 특이한 연산자

`&& || !`

자바와 완전 동일.  
자바처럼 Lazy 연산을 수행한다.

<br>

**코틀린에만 있는 특이한 연산자**

`in / !in`

컬렉션이나 범위에 포함되어 있다, 포함되어 있지 않다.

`a..b`

a부터 b까지의 범위 객체를 생성한다.

`a[i]`

a에서 특정 Index i로 값을 가져온다.

`a[i] = b`

a의 특정 index i에 b를 넣는다.


#
#### (4) 코틀린에서 연산자를 다루는 방법

코틀리넹서는 객체마다 연산자를 직접 정의할 수 있다.

