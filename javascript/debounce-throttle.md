# 디바운스(Debounce)와 쓰로틀(Throttle)이란?

디바운스와 쓰로틀은 짧은 시간에 이벤트가 과도하게 발생할 때 호출 횟수를 제어해 성능을 개선하는 기법이다.

## 디바운스(Debounce)

디바운스는 **연속 호출이 멈춘 뒤 일정 시간이 지나면 함수가 1번 실행**되도록 만든다.

- 입력창 자동완성
- 검색 API 호출
- resize 이후 최종 레이아웃 계산

### 동작 예시

사용자가 300ms 안에 계속 타이핑하면 함수 실행이 계속 미뤄지고, 타이핑이 멈춘 시점에서 한 번만 실행된다.

```javascript
function debounce(callback, delay) {
  let timerId;

  return function (...args) {
    clearTimeout(timerId);
    timerId = setTimeout(() => {
      callback.apply(this, args);
    }, delay);
  };
}

const onSearch = debounce((keyword) => {
  console.log('API 요청:', keyword);
}, 300);
```

## 쓰로틀(Throttle)

쓰로틀은 **일정 시간 간격마다 함수가 최대 1번만 실행**되도록 만든다.

- scroll 위치 계산
- mousemove 좌표 추적
- 무한 스크롤 트리거

### 동작 예시

이벤트가 계속 발생해도 200ms마다 한 번씩만 실행된다.

```javascript
function throttle(callback, interval) {
  let lastExecuted = 0;

  return function (...args) {
    const now = Date.now();

    if (now - lastExecuted >= interval) {
      lastExecuted = now;
      callback.apply(this, args);
    }
  };
}

const onScroll = throttle(() => {
  console.log('스크롤 처리');
}, 200);
```

## 차이점 정리

- 디바운스: 마지막 이벤트 이후 1번 실행
- 쓰로틀: 이벤트가 이어져도 주기적으로 실행

즉, **"마지막 1번"이 필요하면 디바운스, "중간중간 진행 상황"이 필요하면 쓰로틀**을 쓴다.

## 자주하는 실수

### 1. 디바운스를 scroll에 무조건 사용

scroll 진행 중 상태를 계속 반영해야 하는데 디바운스를 쓰면 마지막에만 실행되어 UX가 어색할 수 있다.

### 2. this/인자 전달 누락

래퍼 함수에서 `callback(...args)`만 쓰면 컨텍스트가 깨질 수 있다. `apply(this, args)`로 전달하는 패턴이 안전하다.

## 정리

- 디바운스는 연속 이벤트가 끝난 뒤 한 번 실행한다.
- 쓰로틀은 일정 주기로 실행 횟수를 제한한다.
- 둘 다 불필요한 연산과 API 호출을 줄여 성능을 개선한다.

## 면접 답변용 한 줄 정리

디바운스는 이벤트가 멈춘 뒤 마지막 한 번만 실행하고, 쓰로틀은 이벤트가 계속 발생해도 일정 주기마다 한 번만 실행해 과도한 호출을 제어한다.
