---
title: "자바의 변수"
date: 2022-05-16T00:15:00+09:00
draft: false
---
자바의 변수
<!--more--> 

###
#### print() vs println()
- System.out.print(): 괄호 안의 내용을 출력하고 줄 바꿈을 하지 않는다.

- System.out.println(): 괄호 안의 내용을 출력하고 줄바꿈을 한다.


```
class {
    public static void main(String args[]) {
        System.out.println("Hello, world");
        System.out.print("Hello");
        System.out.print("World");
    } // 결과: Hello, world
      //      Helloworld
}
```

###
#### 덧셈 뺼셈 계산
(+ 덧셈, - 뺄셈, * 곱셈, / 나눗셈)


```
class {
    public static void main(Stirng args[]) {
        System.out.println(5+3); // 8
        System.out.println(5-3); // 2
        System.out.println(5*3); // 15
        System.out.println(5/3); // 1 (정수)
    }
}
```

###
#### 변수의 선언과 저장
- 변수: 하나의 값을 저장할 수 있는 저장공간
- 변수 선언: 변수 타입 변수 이름; (ex. int x;)

변수에 값 저장: x = 5;   
*변수에는 하나의 값만 저장되므로 최신 값이 남는다.


###
#### 변수 타입
숫자 - int, long : 정수(integer)를 저장하기 위한 타입 (20억을 넘을 때는 long)

        - float, double : 실수(floating-point number)를 저장하기 위한 타입. (float는 오차 없이 7자리, double는 15자리)

문자 - char : 문자(character)를 저장하기 위한 타입

        - String : 여러 문자(문자열, string)을 저장하기 위한 타입


###
#### 상수와 리터럴
- 상수: 변수와 마찬가지로 값을 저장할 수 있는 공간이지만 변수와 다르게 한번 값을 저장하면 다른 값으로 변경할 수 없다.
- 상수 선언: final 상수타입 상수 이름; (ex. final int MAX_VALUE = 10;)  
상수 이름은 대문자로 하는 것이 관례, 여러 단어로 이루어져 있을 경우 _로 구분

- 리터럴: 우리가 알고 기존에 알고 있는 상수의 다른 이름 (같은 의미)
- 변수: 하나의 값을 저장하기 위한 공간 (variable)
- 상수: 값을 한번만 저장할 수 있는 공간 (constant)
- 리터럴: 그 자체로 값을 의미하는 것 (literal)

리터럴의 타입과 접미사

- 논리형 - false, true: 없음
- 정수형 - 123, 100L 등 : L
- 실수형 - 1.23, 3.14, 1.3f 등 : f, d
- 문자형 - 'A', '1' 등 : 없음
- 문자열: 'ABC', '123' 등 : 없음
- 문자 리터럴: 'A'처럼 작은따옴표로 문자 하나를 감싼 것. 두 문자 이상은 큰 따옴표로 감싸야하며 문자열 리터럴이라고 한다.   
ex. String string = ""; | char ch = ' '; (문자열은 아무 문자가 없어도 되지만, 문자는 반드시 하나의 문자가 있어야 한다.)


###
#### 문자열 결합
: 문자열을 합칠 때도 덧셈(+)을 사용한다.   
: 어떤 타입의 변수도 문자열과 덧셈연산을 하면 문자열이 된다.


###
#### 기본형과 참조형
- 기본형: 실제 값을 저장 (논리형(boolean), 문자형(char), 정수형(byte, short, int, long), 실수형(float, double))

- 참조형: 어떤 값이 저장되어 있는 주소를 값으로 저장   
: 자바에서는 참조형 변수 간의 연산을 할 수 없으므로 실제 연산에 사용되는 것은 모두 기본형 변수

boolean(false, true): 8bit / 1byte   
char: 16bit / 2byte   
byte: 8bit / 1byte   
short: 16bit / 1byte   
int: 32bit / 4byte   
long: 64bit / 8byte   
float: 32bit / 4byte   
double: 64bit / 8byte   

###
#### printf 이용
```
System.out.printf("age: %d", age);
System.out.printf("age: %d", 14);
System.out.printf("age:14");   

//"age:14"가 화면에 출력
```

###
#### 화면으로부터 입력받는 방법
: Scanner 사용

```
class {
    public static void main(String args[]) {
        Scanner scanner = new Scanner(System.in);

        System.out.print("두자리 정수를 하나 입력해주세요."); // 두자리 정수를 하나 입력해주세요. (입력)
        String input = scanner.nextLine();
        int number = Integer.parseInt(input);
        
        System.out.println("입력내용 : " + input); // 입력내용 : 22
        System.out.println("num=%d%n", number); // number=22
    }
}
```

###
#### 타입 간의 변환방법
- 숫자를 문자로 변환: 숫자에 '0'을 더한다.

- 문자를 숫자로 변환: 문자에서 '0'을 뺀다.

- 숫자를 문자열로 변환: 숫자에 빈 문자열("")을 더한다.

- 문자열을 숫자로 변환: Integer.parseInt() or Double.parseDouble()을 사용한다.

- 문자열을 문자로 변환: charAt(0)을 사용한다.

- 문자를 문자열로 변환: 빈 문자열("")을 더한다.
