# DOM이란?

DOM은 Document Object Model의 약자로, HTML 문서를 구조화하여 표현한 것을 의미한다. 또한 프로그래밍 언어가 DOM 구조에 접근할 수 있는 방법을 제공하여 문서의 구조, 스타일, 내용 등을 변경할 수 있게 해주는 인터페이스 역할을 한다.

브라우저는 HTML 문자열 자체를 직접 다루는 것이 아니라, 이를 파싱하여 만든 **트리 형태의 객체 모델(DOM Tree)** 을 다룬다.

## DOM의 핵심 구성

- `document`: 문서 전체를 나타내는 최상위 객체
- `Element`: 태그 노드 (`div`, `p` 등)
- `Text`: 텍스트 노드

즉, DOM은 노드(Node)들의 트리 구조로 이루어져 있다.

## 자주 사용하는 DOM 조작 API

- `document.querySelector()` / `querySelectorAll()`
- `document.createElement()`
- `appendChild()` / `removeChild()`
- `element.textContent`, `element.innerHTML`

# DOM 이벤트와 Event Loop

DOM 이벤트(`click`, `keydown` 등)의 콜백은 태스크 큐를 통해 Event Loop에 의해 실행된다.

`requestAnimationFrame`은 일반적인 매크로 태스크와 동일하게 취급되기보다는, **브라우저의 다음 렌더링 직전에 실행되는 렌더링 타이밍 콜백**으로 이해하는 것이 더 정확하다.

또한 `Promise.then`은 **마이크로태스크 큐**에서 처리되며, 다음 태스크보다 먼저 실행된다.

## 정리

DOM은 브라우저가 HTML을 파싱해 만든 트리 구조의 객체 모델이며, JavaScript가 이를 통해 문서의 구조와 스타일, 내용을 동적으로 조작할 수 있게 해주는 인터페이스다.
