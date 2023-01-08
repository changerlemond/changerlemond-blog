---
title: "자바의 입력 깊게 보기"
date: 2023-01-08T21:39:31+09:00
tags: ['java']
draft: false
---
`Scanner`가 느린 이유, 그래서 `BufferedReader`를 써야 하는 이유!
<!--more-->

#
알고리즘을 풀다 보면 Scanner 로 간단하게 배울 때도 있는데, 더 깊게 하다보면 속도 면에서 Scanner 를 사용하지 말라고 나온다.  
그냥 그렇구나~ 할 수도 있는데, 왜 Scanner 를 사용하면 안되지? 를 생각해볼 법 하다.

<br>

```
백준 알고리즘 15552번 문제 중에서 아래의 내용이 포함되어 있다.
그리고 실제로 Scanner와 System.out.println을 사용할 경우 시간 초과가 발생한다.

Java를 사용하고 있다면, Scanner와 System.out.println 대신 BufferedReader와 BufferedWriter를 사용할 수 있다. 
BufferedWriter.flush는 맨 마지막에 한 번만 하면 된다.
```

<br>

Scanner 와 BufferedReader 가 어떤 성능 차이가 있는지는 자바의 입력을 조금 더 파헤치면 자세히 알 수 있었다.

#
### 자바의 인코딩 과정 먼저 살펴보기
자바의 인코딩을 먼저 살펴볼 필요가 있다.  

자바는 String 을 처리할 때 메모리에서는 UTF-16 BE 인코딩으로 문자열을 저장하고, 송수신에서는 직렬화가 필요할 때 변형된 UTF-8을 사용한다.

문자열을 입출력할 때는 운영체제의 기본 인코딩 값 또는 사용자가 지정한 인코딩 값으로 문자열을 인코딩한다. (**메모리 상의 처리와는 다르다!**)

<br>

UTF-8과 UTF-16의 차이는 Byte 구성 방식의 차이가 있다. UTF-8의 경우 영어는 1Byte, 한글은 3Byte 를 사용하는 반면 UTF-16의 경우 대부분의 문자가 2Byte 로 구성된다.


#
### System.in, 그리고 InputStream
스트림은 단방향 데이터 흐름이라고 생각하면 이해가 빠른 것 같다. 그래서 입력 스트림, 출력 스트림이 별도로 있다.
자바에서 기본이 되는 입력 스트림은 `InputStream`이다.

보통 콘솔에서 입력값을 받을 때 System.in을 자주 사용하는데 여기서 **System.in은 InputStream 타입의 필드라는 점**을 기억해둬야 한다!  
더 자세히 보면 System 클래스의 in 이라는 필드는 InputStream 의 정적 필드임을 볼 수 있다.

<br>

```java
public final class System {
    public static final InputStream in = null;
}
```
한마디로 in 이라는 변수는 InputStream 의 변수!

<br>

*자바에서 입출력 클래스는 java.io 패키지에 있다. 반드시 예외처리도 필수이다!  
try-catch 문으로 예외를 처리해주거나 메소드에 throw IOException 으로 처리가 가능하다.  

(Scanner 나 System.out.print 메소드는 해당 메소드 안에서 자체적으로 예외처리를 해주므로 별도로 해줄 필요가 없다.   
io 패키지는 IOException 예외를 던지므로 이를 처리해주어야 한다.)

<br>

BufferedReader 의 readLine() 구현체
```java
public class BufferedReader extends Reader {
    public String readLine() throws IOException {
        return readLine(false, null);
    }
}
```

<br>

Scanner 의 nextInt(int radix) 구현체
```java
public final class Scanner implements Iterator<String>, Closable {
    public int nextInt(int radix) {
        // Check cached result
        if ((typeCache != null) && (typeCache instanceof Integer)
                && this.radix == radix) {
            int val = ((Integer)typeCache).intValue();
            useTypeCache();
            return val;
        }
        setRadix(radix);
        clearCaches();
        // Search for next int
        try {
            String s = next(integerPattern());
            if (matcher.group(SIMPLE_GROUP_INDEX) == null)
                s = processIntegerToken(s);
            return Integer.parseInt(s, radix);
        } catch (NumberFormatException nfe) {
            position = matcher.start(); // don't skip bad token
            throw new InputMismatchException(nfe.getMessage());
        }
    }
}
```

<br>

하지만 InputStream.read()를 했을 때 **입력한 값이 정상적으로 나오지 않는 것**을 볼 수 있다.  
이건 두가지 특징이 있기 때문이다.

```
1. 입력받은 데이터는 int 형으로 저장되는데 해당 문자의 시스템 또는 운영체제의 인코딩 형식의 10진수로 변수에 저장된다.
2. 1byte만 읽는다.
```

이런 바이트 단위로 주고받는 스트림은 바이트 스트림이라고 한다. 특히 한글은 1Byte 범위를 벗어나기 때문에 정상적으로 인식하지 못한다.


#
### Scanner

Scanner 를 사용할 때 이렇게 보통 사용한다.
```java
public class inputTest {
    public static void main(String[] args) throws IOException {
        Scanner scanner = new Scanner(System.in);
    }
}
```

Scanner 의 구현체를 보면 왜 InputStream 인 System.in 이 필요한지 알 수 있다.

```java
public final class Scanner implements Iterator<String>, Closable {
    public Scanner(InputStream source) {
        this(new InputStreamReader(source), WHITESPACE_PATTERN);
    }
}
```

여기서 이 생성자는 가장 상위에 있는 Scanner 로 보낸다. 이 상위에 있는 Scanner 의 구현체는 아래와 같다.
```java
public final class Scanner implements Iterator<String>, Closable {
    private Scanner(Readable source, Pattern pattern) {
        assert source != null : "source should not be null";
        assert pattern != null : "pattern should not be null";
        this.source = source;
        delimPattern = pattern;
        buf = CharBuffer.allocate(BUFFER_SIZE);
        buf.limit(0);
        matcher = delimPattern.matcher(buf);
        matcher.useTransparentBounds(true);
        matcher.useAnchoringBounds(false);
        useLocale(Locale.getDefault(Locale.Category.FORMAT));
    }
}
```
그 위 구현체를 보면 이 Scanner 를 만들기 위해 인자값으로 new InputStreamReader(source) 를 보내고 있다.  
여기서 등장하는 InputStreamReader 는 InputStream 을 확장시킨 것이다. 한마디로 **바이트로 읽어들이던 형식을 문자단위로 데이터를 변환시키는 중개자 역할**을 한다고 보면 이해가 쉽다.

InputStreamReader 는 문자스트림이라고 한다.
```
An InputStreamReader is a bridge from byte streams to character streams: 
It reads bytes and decodes them into characters using a specified charset. 
The charset that it uses may be specified by name or may be given explicitly, 
or the default charset may be used.

Each invocation of one of an InputStreamReader's read() methods may cause one 
or more bytes to be read from the underlying byte-input stream. 
To enable the efficient conversion of bytes to characters, 
more bytes may be read ahead from the underlying stream than are necessary to satisfy the current read operation.
```

이 InputStreamReader 를 사용하면 InputStream 을 사용했을 때 입력한 값이 정상적으로 나오지 않던 오류 없이 정상적으로 값이 출력되는 것을 볼 수 있다.

<br>

다시 Scanner 로 돌아가서 그럼 Scanner 를 사용했을 때 구현체를 살펴보면 (예시로 nextInt) integerPattern 이라는 메소드가 있다.
```java
public final class Scanner implements Iterator<String>, Closable {
    public int nextInt(int radix) {
        // Check cached result
        if ((typeCache != null) && (typeCache instanceof Integer)
                && this.radix == radix) {
            int val = ((Integer)typeCache).intValue();
            useTypeCache();
            return val;
        }
        setRadix(radix);
        clearCaches();
        // Search for next int
        try {
            String s = next(integerPattern());
            if (matcher.group(SIMPLE_GROUP_INDEX) == null)
                s = processIntegerToken(s);
            return Integer.parseInt(s, radix);
        } catch (NumberFormatException nfe) {
            position = matcher.start(); // don't skip bad token
            throw new InputMismatchException(nfe.getMessage());
        }
    }
}
```

```java
public final class Scanner implements Iterator<String>, Closable {
    private Pattern integerPattern() {
        if (integerPattern == null) {
            integerPattern = patternCache.forName(buildIntegerPatternString());
        }
        return integerPattern;
    }
}
```

여기서 buildIntegerPatternString()의 구현체를 타고 들어가면 이렇게 생겼다.
```java
public final class Scanner implements Iterator<String>, Closable {
    private String buildIntegerPatternString() {
        String radixDigits = digits.substring(0, radix);
        // \\p{javaDigit} is not guaranteed to be appropriate
        // here but what can we do? The final authority will be
        // whatever parse method is invoked, so ultimately the
        // Scanner will do the right thing
        String digit = "((?i)["+radixDigits+"\\p{javaDigit}])";
        String groupedNumeral = "("+non0Digit+digit+"?"+digit+"?("+
                groupSeparator+digit+digit+digit+")+)";
        // digit++ is the possessive form which is necessary for reducing
        // backtracking that would otherwise cause unacceptable performance
        String numeral = "(("+ digit+"++)|"+groupedNumeral+")";
        String javaStyleInteger = "([-+]?(" + numeral + "))";
        String negativeInteger = negativePrefix + numeral + negativeSuffix;
        String positiveInteger = positivePrefix + numeral + positiveSuffix;
        return "("+ javaStyleInteger + ")|(" +
                positiveInteger + ")|(" +
                negativeInteger + ")";
    }
}
```

Scanner 가 속도가 느린 이유가 여기에 있다. 위와 같은 **정규식 검사를 정말 많이 하기 떄문**이다.  
물론 장점은 강력하게 정규식 검사를 하고 있기 때문에 타입 변환의 안정성이 매우 뛰어나다는 점이 있다.

이 수많은 정규식을 통과하고 나면 patternCache.forName()으로 보내서 이 메소드에서 Pattern 이라는 타입으로 compile 을 호출하여 String 정규식 문자열을 Pattern 이라는 객체로 반환시켜준다.
```java
public final class Scanner implements Iterator<String>, Closable {
    Pattern forName(String name) {
        if (oa == null) {
            Pattern[] temp = new Pattern[size];
            oa = temp;
        } else {
            for (int i = 0; i < oa.length; i++) {
                Pattern ob = oa[i];
                if (ob == null)
                    continue;
                if (hasName(ob, name)) {
                    if (i > 0)
                        moveToFront(oa, i);
                    return ob;
                }
            }
        }

        // Create a new object
        Pattern ob = Pattern.compile(name);
        oa[oa.length - 1] = ob;
        moveToFront(oa, oa.length - 1);
        return ob;
    }
}
```
이 과정을 거쳐 결국 pattern 객체를 string 객체로 변환, 그리고 return 시 Integer.parseInt 로 int 형으로 리턴되는 과정이다.


#
### BufferedReader
BufferedReader 는 아래와 같이 주로 사용하고 있다.
```java
public class InputTest {
    public static void main(String[] args) {
        BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(System.in));
    }
}
```

처리과정은 Scanner 와 거의 흡사한데, BufferedReader 를 사용하는 이유는 문자열처럼 보내버리기 위해서이다.  
Scanner 에서는 내부에서 임시 배열을 두어 문자열처럼 사용하는 구조인데, Buffer 를 통해 입력 받은 문자를 쌓아둔 뒤 한번에 문자열처럼 보내버리는 것이다.

BufferedReader 를 사용할 때 readLine()을 많이 사용하는데 이 메소드는 한줄 전체를 읽기 때문에 char 배열을 하나하나 생성하지 않아도 String 으로 리턴하여 받을 수 있다는 장점이 있다.
이를 사용하면 좋은 점은 **버퍼가 있는 스트림**이고 **별다른 정규식을 검사하지 않기 때문**에 Scanner 에 비해 성능이 좋을 수 밖에 없다.


#
### 마지막으로
정리하면 InputStream 은 바이트 단위, InputStreamReader 는 문자 단위, BufferedReader 는 스트림에 버퍼를 두어 문자를 한번에 보내는 것이라고 생각할 수 있을 것 같다.  
그리고 Scanner 는 많은 정규식을 거치기 때문에 성능면에서 좋지 않고, BufferedReader 는 정규식을 검사하지 않기 때문에 속도가 더욱 빠르다.

기본적인 알고리즘을 풀 때는 관계 없겠지만, 그래도 되도록 Scanner 보다는 BufferedReader 를 사용하는 것이 성능 면에서 좋다는 점은 기억해두자!