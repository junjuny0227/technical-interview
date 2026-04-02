# Promise와 async/await란?

`Promise`는 비동기 작업의 완료(성공/실패)를 표현하는 객체이고, `async/await`는 Promise를 동기 코드처럼 읽기 쉽게 다루는 문법이다.

## Promise 핵심 개념

Promise는 3가지 상태를 가진다.

- `pending`: 대기
- `fulfilled`: 성공
- `rejected`: 실패

한 번 `fulfilled` 또는 `rejected`가 되면 상태는 다시 바뀌지 않는다.

### 기본 예제

```javascript
const promise = new Promise((resolve, reject) => {
  const success = true;

  if (success) {
    resolve('성공');
  } else {
    reject(new Error('실패'));
  }
});

promise
  .then((result) => {
    console.log(result);
  })
  .catch((error) => {
    console.error(error.message);
  })
  .finally(() => {
    console.log('작업 종료');
  });
```

## Promise 체이닝

`then`은 Promise를 반환하므로 체이닝이 가능하다.

```javascript
Promise.resolve(1)
  .then((num) => num + 1)
  .then((num) => num + 1)
  .then((num) => {
    console.log(num); // 3
  })
  .catch((error) => {
    console.error(error);
  });
```

중간 단계에서 에러가 발생하면 가장 가까운 `catch`로 전파된다.

## async/await 핵심 개념

- `async` 함수는 항상 Promise를 반환한다.
- `await`는 Promise가 처리될 때까지 기다린 뒤 결과를 반환한다.
- `await`는 `async` 함수 내부에서만 사용할 수 있다.

### 기본 예제

```javascript
function fetchUser() {
  return new Promise((resolve) => {
    setTimeout(() => resolve({ id: 1, name: 'Alice' }), 500);
  });
}

async function printUser() {
  try {
    const user = await fetchUser();
    console.log(user.name); // Alice
  } catch (error) {
    console.error(error);
  } finally {
    console.log('완료');
  }
}

printUser();
```

## 병렬 처리 패턴

순차 실행이 필요 없을 때는 `Promise.all`로 병렬 처리하는 것이 효율적이다.

```javascript
async function fetchAll() {
  const [user, posts] = await Promise.all([fetch('/user'), fetch('/posts')]);
  return { user, posts };
}
```

자주 쓰는 메서드:

- `Promise.all`: 하나라도 실패하면 전체 실패
- `Promise.allSettled`: 모두 끝날 때까지 기다리고 각 결과 반환
- `Promise.race`: 가장 먼저 끝난 Promise 결과 반환
- `Promise.any`: 가장 먼저 성공한 Promise 결과 반환(모두 실패하면 에러)

## 자주하는 실수

### 1. async 함수의 에러를 처리하지 않음

```javascript
async function run() {
  const data = await fetchData();
  console.log(data);
}

run(); // 에러 처리 누락 가능
```

해결: `try/catch`를 쓰거나 호출부에서 `.catch()`로 처리한다.

### 2. await를 불필요하게 연속 사용

```javascript
const a = await fetchA();
const b = await fetchB();
```

두 작업이 독립적이면 병렬로 실행하는 것이 좋다.

```javascript
const [a, b] = await Promise.all([fetchA(), fetchB()]);
```

## 정리

- Promise는 비동기 결과를 상태 기반으로 다루는 표준 객체다.
- async/await는 Promise를 더 읽기 쉬운 방식으로 작성하게 해준다.
- 에러 처리는 `catch` 또는 `try/catch`로 명확히 해야 한다.
- 독립적인 비동기 작업은 `Promise.all`로 병렬 처리하면 성능상 유리하다.

## 면접 답변용 한 줄 정리

Promise는 비동기 결과를 상태로 관리하는 객체이고, async/await는 이를 동기식 흐름처럼 작성하는 문법이며, 핵심은 에러 전파 처리와 필요한 곳에서의 병렬 실행이다.
