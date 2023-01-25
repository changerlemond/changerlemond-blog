---
title: "Spoke로 깔끔한 테스트 코드 작성하기"
date: 2023-01-25T19:43:03+09:00
draft: false
---

Spoke 첫 입문기 (feat. 테스트 코드와 친해지기)
<!--more--> 

<br>

## Spoke 시작하기
### build.gradle에 의존성 추가하기

```
plugins {
    id 'groovy'
}

dependencies {
    testImplementation('org.spockframework:spock-core:2.0-groovy-3.0')
}
```

> Spock은 Groovy를 사용하며, BDD 프레임워크이다.

<br>

### TDD vs BDD
<div style="text-align:left">
    <img src="/images/test/spoke/tdd-ddd-bdd.png" alt="image" width="600px" height="400px" />
</div>

<br>

**TDD**
<div style="text-align:left">
    <img src="/images/test/spoke/tdd.png" alt="image" width="600px" height="400px" />
</div>

<br>

- Test Driven Development
- 테스트 중심 (개발자)

<br>

**BDD**
<div style="text-align:left">
    <img src="/images/test/spoke/bdd.png" alt="image" width="600px" height="400px" />
</div>

<br>

- Behaviour Driven Development
- 행동 중심 (사용자)

<br>
<br>

## Spock를 이용한 간단 테스트 해보기
Calculator의 로직을 Spock를 이용해 간단하게 테스트를 진행
```kotlin
class Calculator(
    private var _number: Int
) {

    val number: Int
        get() = this._number

    fun add(operand: Int) {
        this._number += operand
    }

    fun minus(operand: Int) {
        this._number -= operand
    }

    fun multiply(operand: Int) {
        this._number *= operand
    }

    fun divide(operand: Int) {
        if (operand == 0) {
            throw IllegalArgumentException("0으로 나눌 수 없습니다")
        }
        this._number /= operand
    }

}
```

<br>

### JUnit vs Spoke
<div style="display: flex; justify-content: space-between">
<div style="width: 49%">

**JUnit**
```kotlin
import org.assertj.core.api.AssertionsForInterfaceTypes.assertThat
import org.junit.jupiter.api.Test

class JunitCalculatorTest {

    @Test
    fun addTest() {

        //given
        val calculator = Calculator(5)

        //when
        calculator.add(3)

        //then
        assertThat(calculator.number).isEqualTo(8)
    }

}
```
</div>

<div style="width: 49%">

**Spoke**

```groovy
import spock.lang.Specification


class CalculatorSpockTest extends Specification {


    def addTest() {

        given:
        Calculator calculator = new Calculator(5)

        when:
        calculator.add(3)

        then:
        8 == calculator.getNumber()
    }

}
```

</div>
</div>

<div style="text-align:left">
    <img src="/images/test/spoke/comparison-to-junit.png" alt="image" />
</div>

<br>

### Spoke의 코드 블럭 (Feature Method)
> Spock는 코드 블럭을 선언해줘야 한다. (given, when, then)   
테스트 메소드 안에 최소한 하나 이상의 블럭이 있어야 한다.

<div style="text-align:left">
    <img src="/images/test/spoke/code-block.png" alt="image" width="600px" height="400px" />
</div>

<br>

- given (or setup): Junit에서 given처럼 테스트에 필요한 환경을 준비하는 과정. 다른 블록보다 상위에 위치해야 한다.
- when: 테스트할 대상 코드를 실행
- then: 테스트할 대상 코드의 결과를 검증
- expect: when + then - 한줄로 표현할 때 주로 사용
- cleanup: 테스트 메소드 종류 후 정리할 때 사용 (ex. 데이터, 파일 등 삭제)
- where:  feature 메소드를 파라미터화해서 실행
- with: then 블럭에서 사용 가능하며 객체를 검사할 때 유용하다. 단 실패한 첫번째 경우만 알려주며 멈춘다.
- verifyAll: with와 같은 기능을 하지만 모든 경우를 다 돌고 실패한 곳을 모두 알려준다.

<br>

### 다양한 조건으로 테스트해보기 (feat. where block 활용하기)

<br>

**Spoke**
```groovy
import spock.lang.Specification

class CalculatorSpockTest extends Specification {

    def addTest() {

        given:
        Calculator calculator = new Calculator(number)

        expect:
        calculator.add(addNumber)
        calculator.number == result

        where:
        number | addNumber | result
        5      | 3         | 10            // fail
        12     | 98        | 112           // fail
        1001   | 533       | 1534          // success

    }

}
```

<div style="text-align:left">
    <img src="/images/test/spoke/spoke-error-message.png" alt="image" />
    <em>테스트가 실패하는 경우 실패한 케이스에 대해 친절하게 자세히 알려준다.</em>
</div>

<br>

**JUnit**
```kotlin
import org.assertj.core.api.AssertionsForInterfaceTypes.assertThat
import org.junit.jupiter.api.Test

class JunitCalculatorTest {

    @Test
    fun addTest() { 
        
        // given
        val calculator = Calculator(5)
        
        // when & then
        calculator.add(3)
        assertThat(calculator.number).isEqualTo(10) // fail

        // when & then
        calculator.add(12)
        assertThat(calculator.number).isEqualTo(8) // fail

        // when & then
        calculator.add(98)
        assertThat(calculator.number).isEqualTo(103) // success

    }

}
```

<div style="text-align:left">
    <img src="/images/test/spoke/junit-error-message.png" alt="image" />
    <em>테스트가 실패한 첫번째 케이스만 알 수 있다.</em>
</div>

<br>

> Spock는 다양한 케이스를 where를 통해 조금 더 가독성 좋게 테스트해볼 수 있고, 모든 테스트 실패 케이스를 알려준다.

<br>

### 예외사항 테스트하기

<br>

<div style="display: flex; justify-content: space-between">
<div style="width: 49%">

**JUnit**
```kotlin
import org.assertj.core.api.AssertionsForInterfaceTypes.assertThat
import org.junit.jupiter.api.Test
import org.junit.jupiter.api.assertThrows

class JunitCalculatorTest {

    fun divideExceptionTest() {

        // given
        val calculator = Calculator(5);

        // when & then
        assertThrows<IllegalArgumentException> {
            calculator.divide(0)
        }.apply {
            assertThat(message).isEqualTo("0으로 나눌 수 없습니다")
        }

    }

}
```
</div>

<div style="width: 49%">

**Spoke**
```groovy
import spock.lang.Specification



class CalculatorSpockTest extends Specification {

	def divideExceptionTest() {

        given:
        Calculator calculator = new Calculator(10)

        when:
        calculator.divide(0)

        then:
        def exception = thrown(IllegalArgumentException.class)
        exception.message == "0으로 나눌 수 없습니다"

    }

}
```

</div>
</div>

<br>

> Spock에서 예외는 thrown() 메서드로 검증할 수 있고, 코드의 흐름에 따라 예외를 확인할 수 있어 코드를 쉽게 이해할 수 있다.

<br>

### Mock 사용해보기
```groovy
def "mock으로 임의의 값 넣어주는 테스트"() {

        given:
        def calculator = GroovyMock(Calculator.class)

        when:
        calculator.getNumber() >> 5

        then:
        5 == calculator.getNumber()

    }
```
가짜 객체의 값은 >> 를 통해 설정할 수 있다.

<br>

> JUnit 테스트는 테스트 메서드의 이름이 늘 고민스럽지만, Spock의 경우 한글로 표현할 수 있어 메서드 작명 고민을 줄여준다.

<br>

## 생각
JUnit의 단점을 해소할 수 있다고 느꼈다.

테스트코드를 작성할 때 어려웠던 점은 쉽게 작성할 수 있어야 한다는 점이었는데, 테스트 코드의 메소드 이름을 영어로 작명해야 하는 점과 여러 케이스를 테스트할 때의 반복적인 코드가 어쩔 수 없이 발생했던 경험이 있다.

Spock을 살펴보니 이런 점들을 많이 해소할 수 있었고, 그래서 테스트코드에 대한 부담감이 줄어드는 느낌이었다.

조금 더 깊이있게 사용해볼 예정이다. 🙂

<br>

> 좋았다고 느낀 점
> - 메소드 이름을 한글로도 작성할 수 있다. 알아보기 힘든 영어 메소드 이름이라면 차라리 한글로 짓고 누가 봐도 어떤 테스트인지 알아볼 수 있는 것이 좋은 테스트 코드라고 생각한다.
> - 여러 가지 테스트가 가능하다. 반복적인 코드를 where 블럭을 통해 해소할 수 있었다.
> - 에러에 대한 메시지를 전부 다 보여준다. 어떤 부분이 잘못되었는지 쉽게 파악할 수 있어 테스트 코드를 고치는 데에 용이하다.
> - 코드 블럭의 명시가 직관적이다.

<br>

## 참고자료
- [Spock 공식 문서](https://spockframework.org/spock/docs/2.3/index.html)
- [Naver D2](https://d2.naver.com/helloworld/568425)
- [우아한형제들 기술블로그](https://techblog.woowahan.com/2560/)
- [향로님 개발블로그](https://jojoldu.tistory.com/228)
- [MobileLive Blog](https://www.mobilelive.ca/blog/value-of-tdd-bdd-ddd)