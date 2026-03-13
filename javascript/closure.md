# 클로저(Closure)란?

클로저는 **내부 함수가 자신이 선언될 당시의 환경(Lexical Environment), 즉 스코프를 기억하여 외부 함수의 실행이 끝난 이후에도 해당 스코프에 접근할 수 있는 함수**를 의미한다.

```javascript
function outerFunction() {
  let num = 10; // 외부 함수의 지역 변수

  function innerFunction() {
    console.log(num); // 내부 함수에서 외부 함수의 변수에 접근
  }

  innerFunction(); // 내부 함수 호출
}

outerFunction(); // 10이 출력됨
```

### 코드 설명

1. `outerFunction` 내부에서 `innerFunction`이라는 내부 함수가 선언되고 호출된다.
2. 내부 함수 `innerFunction`은 자신을 포함하고 있는 외부 함수 `outerFunction`의 지역 변수 `num`에 접근할 수 있다.
3. 이는 `innerFunction`이 **자신이 선언된 환경(outerFunction 내부의 스코프)** 을 기억하고 있기 때문이다.

자바스크립트에서 **스코프는 함수를 호출하는 시점이 아니라, 함수를 어디에서 선언했는지에 따라 결정된다.**

이를 **렉시컬 스코핑(Lexical Scoping)** 이라고 한다.

## 내부 함수가 반환되는 경우

이번에는 내부 함수 `innerFunction`이 `outerFunction`에서 **반환되는 경우**를 살펴보자.

```javascript
function outerFunction() {
  let num = 10; // 외부 함수의 지역 변수

  function innerFunction() {
    console.log(num); // 내부 함수에서 외부 함수의 변수에 접근
  }

  return innerFunction; // 내부 함수 반환
}

const closureFunction = outerFunction(); // outerFunction 실행 후 innerFunction 반환
closureFunction(); // 10이 출력됨
```

### 동작 과정

1. `outerFunction`이 실행되면서 지역 변수 `num`이 생성된다.
2. `innerFunction`이 선언되고, `outerFunction`은 `innerFunction`을 반환한다.
3. `outerFunction`의 실행이 끝나면 콜 스택에서 제거된다.
4. 일반적으로 함수 실행이 끝나면 해당 함수의 지역 변수도 접근할 수 없게 된다.

하지만 위 코드에서는 `closureFunction()`을 호출했을 때 **여전히 `num`에 접근할 수 있고 `10`이 출력된다.**

그 이유는 **`innerFunction`이 자신이 선언될 당시의 스코프(`outerFunction`의 렉시컬 환경)를 기억하고 있기 때문**이다.

## 정리

**내부 함수가 외부 함수보다 오래 살아남아 외부 함수의 스코프에 접근할 수 있는 현상을 클로저(Closure)라고 한다.**

즉, **클로저는 내부 함수가 외부 함수의 렉시컬 환경을 기억하고 이를 계속 참조할 수 있는 구조**이다.
