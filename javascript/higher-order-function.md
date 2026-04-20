# 고차함수(Higher-Order Function)란?

고차함수는 **함수를 인자로 받거나, 함수를 반환하는 함수**를 의미한다.

즉, 자바스크립트에서 함수도 값처럼 다룰 수 있기 때문에 가능한 패턴이다.

```javascript
function greet(name) {
  return `안녕하세요, ${name}`;
}

function runWithKim(callback) {
  return callback('김코딩'); // 함수를 인자로 받아 실행
}

console.log(runWithKim(greet)); // 안녕하세요, 김코딩
```

## 왜 중요한가?

고차함수는 다음을 가능하게 한다.

- 중복 로직을 줄이고 재사용성을 높임
- 관심사를 분리해 코드를 읽기 쉽게 만듦
- 선언형 스타일(map, filter, reduce)로 데이터 처리를 단순화함

## 대표 예제 1: 함수를 인자로 받는 경우

### map

`map`은 각 요소를 변환해서 새로운 배열을 만든다.

```javascript
const numbers = [1, 2, 3];
const doubled = numbers.map((n) => n * 2);

console.log(doubled); // [2, 4, 6]
```

### filter

`filter`는 조건을 만족하는 요소만 골라 새로운 배열을 만든다.

```javascript
const users = [
  { id: 1, active: true },
  { id: 2, active: false },
  { id: 3, active: true },
];

const activeUsers = users.filter((user) => user.active);
console.log(activeUsers);
```

### reduce

`reduce`는 배열을 하나의 값으로 누적한다.

```javascript
const prices = [1000, 2000, 3000];
const total = prices.reduce((sum, price) => sum + price, 0);

console.log(total); // 6000
```

## 대표 예제 2: 함수를 반환하는 경우

고차함수는 조건이나 설정값을 먼저 받아서, 실제 실행 함수를 나중에 반환할 수 있다.

```javascript
function makeMultiplier(multiplier) {
  return function (value) {
    return value * multiplier;
  };
}

const double = makeMultiplier(2);
const triple = makeMultiplier(3);

console.log(double(10)); // 20
console.log(triple(10)); // 30
```

이 패턴은 설정값을 캡슐화하는 함수 팩토리(factory)로 자주 사용된다.

## 커링(Currying)과의 관계

커링은 여러 인자를 받는 함수를 인자 1개씩 받는 함수 체인으로 바꾸는 기법이다.

```javascript
function add(a) {
  return function (b) {
    return a + b;
  };
}

const add10 = add(10);
console.log(add10(5)); // 15
```

커링된 함수는 고차함수를 기반으로 동작하며, 부분 적용(Partial Application)에 유리하다.

## 실무에서 자주 쓰는 패턴

### 1) 공통 전처리/후처리 래퍼

```javascript
function withLogging(fn) {
  return function (...args) {
    console.log('실행 전:', fn.name);
    const result = fn(...args);
    console.log('실행 후:', result);
    return result;
  };
}

function add(a, b) {
  return a + b;
}

const addWithLogging = withLogging(add);
addWithLogging(1, 2);
```

### 2) 이벤트 핸들러 생성기

```javascript
function createClickHandler(message) {
  return function () {
    console.log(message);
  };
}

const onSaveClick = createClickHandler('저장 버튼 클릭');
```

## 장점과 주의점

장점:

- 재사용성과 조합성 증가
- 중복 코드 감소
- 테스트 가능한 작은 단위 함수로 분리 가능

주의점:

- 콜백 중첩이 깊어지면 가독성이 떨어질 수 있음
- 익명 함수 남발 시 디버깅이 어려울 수 있음
- `this`를 사용하는 메서드를 그대로 넘길 때는 바인딩 이슈를 주의해야 함

```javascript
const obj = {
  value: 10,
  getValue() {
    return this.value;
  },
};

const extracted = obj.getValue;
console.log(extracted()); // strict mode에서 undefined 가능

const fixed = obj.getValue.bind(obj);
console.log(fixed()); // 10
```

## 정리

- 고차함수는 함수를 인자로 받거나 함수를 반환하는 함수다.
- 자바스크립트의 함수 1급 객체 특성을 활용하는 핵심 패턴이다.
- 선언형 데이터 처리(map/filter/reduce), 함수 팩토리, 커링, 래퍼 패턴 등에 널리 사용된다.

## 면접 답변용 한 줄 정리

고차함수는 함수를 값처럼 전달하거나 반환해 로직을 추상화하는 함수로, 재사용성과 조합성을 높이는 자바스크립트의 핵심 함수형 패턴이다.
