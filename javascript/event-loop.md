# 이벤트 루프(Event Loop)란?

이벤트 루프는 **자바스크립트의 비동기 작업을 처리하기 위한 메커니즘**이다.
자바스크립트는 기본적으로 **싱글 스레드(Single Thread)** 로 동작하기 때문에 한 번에 하나의 작업만 처리할 수 있다.

하지만 실제 애플리케이션에서는 다음과 같은 **비동기 작업**이 많이 발생한다.

- `setTimeout`
- `setInterval`
- `fetch` / `axios` 같은 네트워크 요청
- `Promise`
- DOM 이벤트 (`click`, `scroll` 등)

이벤트 루프는 **콜 스택(Call Stack)과 태스크 큐(Task Queue)를 확인하면서 비동기 작업을 처리하는 역할**을 한다.

## 자바스크립트 실행 구조

자바스크립트의 실행 구조는 크게 다음과 같이 이루어진다.

1. **Call Stack (콜 스택)**
2. **Web APIs**
3. **Task Queue (태스크 큐)**
4. **Event Loop (이벤트 루프)**

동작 과정은 다음과 같다.

1. 자바스크립트 코드가 **Call Stack**에서 실행된다.
2. `setTimeout`, `fetch`, 이벤트 리스너 등의 **비동기 작업은 Web APIs에 전달된다.**
3. 작업이 완료되면 **Task Queue**에 콜백 함수가 들어간다.
4. **Event Loop**는 Call Stack이 비어 있는지 확인한다.
5. Call Stack이 비어 있으면 **Task Queue에 있는 콜백을 Call Stack으로 이동시켜 실행한다.**

## 예제 코드

```javascript
console.log('start');

setTimeout(() => {
  console.log('timeout');
}, 0);

console.log('end');
```

### 실행 결과

```
start
end
timeout
```

### 동작 과정

1. `console.log("start")` 실행
2. `setTimeout`은 **Web APIs로 전달**
3. `console.log("end")` 실행
4. Call Stack이 비어 있음
5. Event Loop가 Task Queue에서 `timeout` 콜백을 가져와 실행

# 매크로 태스크 큐(Macro Task Queue)

매크로 태스크 큐는 **일반적인 비동기 콜백들이 들어가는 큐**이다.

대표적인 매크로 태스크는 다음과 같다.

- `setTimeout`
- `setInterval`
- `setImmediate`
- DOM 이벤트 (`click`, `keydown` 등)
- `requestAnimationFrame`

이러한 작업들은 **매크로 태스크 큐에 저장되었다가 Event Loop에 의해 실행된다.**

## 예제 코드

```javascript
console.log('start');

setTimeout(() => {
  console.log('macro task');
}, 0);

console.log('end');
```

### 실행 결과

```
start
end
macro task
```

`setTimeout`의 콜백은 **매크로 태스크 큐에 들어가기 때문에 현재 실행 중인 코드가 모두 끝난 이후 실행된다.**

# 마이크로 태스크 큐(Micro Task Queue)

마이크로 태스크 큐는 **매크로 태스크보다 우선적으로 실행되는 큐**이다.

대표적인 마이크로 태스크는 다음과 같다.

- `Promise.then`
- `Promise.catch`
- `Promise.finally`
- `queueMicrotask`
- `MutationObserver`

마이크로 태스크는 **현재 실행 중인 작업이 끝난 직후, 다음 매크로 태스크가 실행되기 전에 모두 처리된다.**

## 예제 코드

```javascript
console.log('start');

setTimeout(() => {
  console.log('macro task');
}, 0);

Promise.resolve().then(() => {
  console.log('micro task');
});

console.log('end');
```

### 실행 결과

```
start
end
micro task
macro task
```

### 동작 과정

1. `console.log("start")` 실행
2. `setTimeout` → **매크로 태스크 큐**
3. `Promise.then` → **마이크로 태스크 큐**
4. `console.log("end")` 실행
5. Call Stack이 비어 있음
6. **마이크로 태스크 큐 먼저 실행**
7. 이후 **매크로 태스크 큐 실행**

# 실행 순서 정리

이벤트 루프는 다음 순서로 작업을 처리한다.

1. Call Stack의 작업 실행
2. Call Stack이 비어 있으면 **Micro Task Queue 먼저 실행**
3. Micro Task Queue가 모두 비워질 때까지 실행
4. 이후 **Macro Task Queue에서 하나의 Task 실행**
5. 다시 Micro Task Queue 확인

즉 실행 순서는 다음과 같다.

```
Call Stack
↓
Micro Task Queue
↓
Macro Task Queue
```

# 정리

- **Event Loop**
  → Call Stack이 비어 있는지 확인하고 Task Queue의 작업을 실행하는 메커니즘

- **Macro Task Queue**
  → `setTimeout`, 이벤트, `setInterval` 등 일반적인 비동기 작업이 들어가는 큐

- **Micro Task Queue**
  → `Promise.then`, `queueMicrotask` 등이 들어가는 큐이며 **Macro Task보다 먼저 실행된다**
