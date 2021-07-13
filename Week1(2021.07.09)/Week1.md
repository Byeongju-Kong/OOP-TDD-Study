# Week1 ( 2021.07.09 )

# TDD의 개념

### ✔️ 테스트가 개발을 이끌어나간다.

### ✔️ 테스트를 먼저 짜고, 이를 통과하는 코드를 작성한다.

머릿 속에 구현을 위한 코드를 먼저 구상 ⇒ 이에 대한 테스트를 위한 코드를 먼저 작성 ⇒ 구현을 위한 코드를 작성

![Week1%20(%202021%2007%2009%20)%20c2ced8bff3324554b11341f2d7b52fcd/_2021-07-10__5.34.52.png](Week1%20(%202021%2007%2009%20)%20c2ced8bff3324554b11341f2d7b52fcd/_2021-07-10__5.34.52.png)

## 개발 순서

1. 어떤 것을 구현할지 생각
2. 실패하는 테스트코드 작성
3. 테스트코드를 성공시키기 위한 실제코드 작성
4. 실제코드와 테스트코드 Refactoring
5. ( 2, 3, 4 )의 과정을 계속 반복한다.

### 💡 실패하는 테스트코드를 작성하는 이유

 운이 좋아서 패스하는 경우를 알아내기 위해서이다. 처음부터 통과하는 경우만을 생각하고 짠다면, 통과하지 못하는 경우에 대한 생각을 하기가 어렵다. 따라서, 통과하지 못하는 테스트를 짜고, 해당 경우에 대한 처리를 해주는 것이다.

# 테스트 주도 개발을 지향하는 이유

## 1. Main에서 해당 클래스가 의도대로 작동하는지 확인하는 것의 불편함

 보통의 사람들 그리고 필자 역시 원하는 의도대로 Class를 설계하였는지 확인하기 위해, Class의 Main에서 인스턴스를 만들고, 이런저런 메서드들을 사용하여 확인을 하였다. 적은 수의 Class로 이루어진 프로젝트의 경우 이렇게 해도 굳이 어렵진 않겠지만, **Class가 몇 백개 있는 프로젝트의 경우엔 Main을 Class의 개수만큼 다 돌려봐야 한다**. 하지만 TDD는 한번의 클릭만으로 전체 프로젝트의 클래스들이 의도대로 작동하는지 확인할 수 있다.

## 2. 좋은 유지보수

### 변화하는 소프트웨어 기술

소프트웨어들의 기술들은 계속해서 발전-변화한다. 따라서 개발자들은 긍정적인 변화를 자신의 프로그램에 잘 적용을 해야한다.

### 낮은 결합도로 연결된 객체들

 필자는 OOP를 공부하는데, 객체지향에선 수 많은 객체들이 다른 수많은 객체들과 연결이 되어있다. 새로운 기술의 개발을 적용하기 위해서 **하나의 객체를 변경한다면, 해당 객체와 연결된 많은 객체들도 영향을 받을 것**이고, 어떠한 문제들이 발생할 것이다. **TDD는 이런 문제들을 빠르게 찾아낼 수가 있고**, 그에 따라 보다 편하게 변화에 대응할 수 있다. 필자는 개인적으로 이 이유가 첫번째 이유보다 더 중요하다고 생각한다.

## 3. 객체지향적인 코드 개발

 내가 구상하는 메서드가 많은 것들을 한다면, 테스트 코드는 상당히 복잡해지기 때문에 작성하기 힘들 것이다.

 반대로 생각하면 간단한 테스트 코드를 짜기 위해선, 구상하는 메서드가 단순하고 명확해야 하고, 하나의 객체가 여러 가지 일을 하도록 구상하면 안된다.

 TDD에 대해 처음 알았을 때, '왜 대체 테스트를 먼저 짜는거지?' 라는 생각이 들었는데, **테스트를 먼저 작성함으로써, 객체의 구현을 좀 더 단순-명확하게 하도록 할 수 있다**고 생각한다.

---

# TDD로 Integer Calculator 만들어보기

### ❗️테스트를 도와주는 가장 기본적인 것은 JUnit 라이브러리인데, 필자는 Assertj를 사용

![Week1%20(%202021%2007%2009%20)%20c2ced8bff3324554b11341f2d7b52fcd/_2021-07-10__5.57.08.png](Week1%20(%202021%2007%2009%20)%20c2ced8bff3324554b11341f2d7b52fcd/_2021-07-10__5.57.08.png)

build.gradle 파일에 아래의 코드를 작성해 의존성을 주입해주면 된다.

```jsx
testCompile("org.assertj:assertj-core:3.11.1")
```

## 1. Calculator 클래스 만들기

```java
//Calculator.java

public class Calculator {
    private int firstNum;
    private int secondNum;

    Calculator(int firstNum, int secondNum){
        this.firstNum = firstNum;
        this.secondNum = secondNum;
    }
    public int getFirstNum() { return firstNum; }
    public int getSecondNum() { return secondNum; }
}

```

두 수의 값을 처리하는 계산기이기 때문에, 두 개의 필드만을 만들었다.

## 2. 실패하는 테스트코드 작성

사칙연산에서 실패하는 경우? → 0으로 나누기 밖에 없다.

```java
//CalculatorTest.java

@Test
@DisplayName("0으로 나누기 예외처리")
void divZeroException() {
    calculator = new Calculator(1, 0);
    assertThatThrownBy(() -> calculator.div()).
            isInstanceOf(ArithmeticException.class).
            hasMessage("0으로 나눌 수 없습니다.");
}
```

1을 0으로 나누려는 실패하는 테스트코드를 작성했다. 

## 3. 통과하기 위한 실제 코드 작성

```java
//Calculator.java
public class Calculator {
    private int firstNum;
    private int secondNum;

    Calculator(int firstNum, int secondNum){ //생성자는 실패할 것도 없기 때문에 먼저 작성했다.
        this.firstNum = firstNum;            //물론 생성자에 대한 테스트 코드를 먼저 작성했다.
        this.secondNum = secondNum;
    }
    public int div(){
        if(secondNum == 0){
            throw new ArithmeticException("0으로 나눌 수 없습니다.");
        }
       
    }

}

```

다음과 같이 작성을 하면 위의 실패하는 경우에 대한 처리를 할 수 있다.

## 4. Refactoring

```java
//CalculatorTest.java
@Test
@DisplayName("두 수의 나누기")
void div() {
    Calculator calculator = new Calculator(1, 0);
    int result = calculator.div();
    assertThat(result).isEqualTo(2);
}

@Test
@DisplayName("0으로 나누기 예외처리")
void divZeroException() {
    calculator = new Calculator(2, 0);
    assertThatThrownBy(() -> calculator.div()).
            isInstanceOf(ArithmeticException.class).
            hasMessage("0으로 나눌 수 없습니다.");
}
```

```java
//Calculator.java
public class Calculator {
    private int firstNum;
    private int secondNum;

    Calculator(int firstNum, int secondNum){
        this.firstNum = firstNum;
        this.secondNum = secondNum;
    }
    public int getFirstNum() { return firstNum; }
    public int getSecondNum() { return secondNum; }

    public int div() {
        if(secondNum == 0){
            throw new ArithmeticException("0으로 나눌 수 없습니다.");
        }
        return firstNum / secondNum;
    }
}
```

0으로 나눌 시에 발생하는 경우와 나누기가 잘 되는 경우를 위와 같이 작성할 수 있다. 물론 테스트코드를 먼저 작성해야한다.

**❗️throw하면서 던지는 메시지와 테스트코드의 hasMessage의 인자가 같아야 한다.**

## 5. 위의 과정 반복

```java
//CalculatorTest.java

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import static org.assertj.core.api.Assertions.assertThat;
import static org.assertj.core.api.Assertions.assertThatThrownBy;
import static org.junit.jupiter.api.Assertions.*;

class CalculatorTest {
    Calculator calculator;
    @BeforeEach   //테스트 메서드들이 실행되기 전에 실행하는 메서드에 대한 Annotation
    void setUp(){
        calculator  = new Calculator(2, 1);
    }

    @Test
    @DisplayName("생성자로 객체의 초기값 설정") //아래의 테스트가 테스트시에 보여지는 문자열
    void create() {

        assertAll(
                () -> assertThat(calculator.getFirstNum()).isEqualTo(2),
                () -> assertThat(calculator.getSecondNum()).isEqualTo(1)
        );
    }
    @Test
    @DisplayName("두 수의 더하기")
    void add() {
        int result = calculator.add();
        assertThat(result).isEqualTo(3);
    }

    @Test
    @DisplayName("두 수의 빼기")
    void sub() {
        int result = calculator.sub();
        assertThat(result).isEqualTo(1);
    }
    @Test
    @DisplayName("두 수의 곱하기")
    void mul() {
        int result = calculator.mul();
        assertThat(result).isEqualTo(2);
    }

    @Test
    @DisplayName("두 수의 나누기")
    void div() {
        Calculator calculator = new Calculator(2, 1);
        int result = calculator.div();
        assertThat(result).isEqualTo(2);
    }

    @Test
    @DisplayName("0으로 나누기 예외처리")
    void divZeroException() {
        calculator = new Calculator(2, 0);
        assertThatThrownBy(() -> calculator.div()).
                isInstanceOf(ArithmeticException.class).
                hasMessage("0으로 나눌 수 없습니다.");
    }
}

```

```java
//Calculator.java

public class Calculator {
    private int firstNum;
    private int secondNum;

    Calculator(int firstNum, int secondNum){
        this.firstNum = firstNum;
        this.secondNum = secondNum;
    }
    public int getFirstNum() { return firstNum; }
    public int getSecondNum() { return secondNum; }

    public int add() {
        return firstNum + secondNum;
    }

    public int sub() {
        return firstNum - secondNum;
    }

    public int div() {
        if(secondNum == 0){
            throw new ArithmeticException("0으로 나눌 수 없습니다.");
        }
        return firstNum / secondNum;
    }

    public int mul() {

        return firstNum * secondNum;
    }
}
```

테스트 코드를 작성하고 메서드를 구현하고 하는 방식을 반복하면 된다.

![Week1%20(%202021%2007%2009%20)%20c2ced8bff3324554b11341f2d7b52fcd/_2021-07-10__6.24.34.png](Week1%20(%202021%2007%2009%20)%20c2ced8bff3324554b11341f2d7b52fcd/_2021-07-10__6.24.34.png)

Test 완료