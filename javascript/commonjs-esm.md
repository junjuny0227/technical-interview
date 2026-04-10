# CommonJS와 ES Module 차이

CommonJS(CJS)와 ES Module(ESM)은 JavaScript 모듈 시스템이다.

- CommonJS: Node.js에서 오래 사용된 모듈 방식
- ES Module: JavaScript 표준 모듈 방식

## 기본 문법 차이

### CommonJS

```javascript
// export
module.exports = { sum };

// import
const { sum } = require('./math');
```

### ES Module

```javascript
// export
export function sum(a, b) {
  return a + b;
}

// import
import { sum } from './math.js';
```

## 핵심 차이

### 1. 로딩 시점

- CommonJS: 런타임에 동적으로 로드 (`require`)
- ES Module: 정적으로 분석되어 로드 (`import`)

ESM은 정적 분석이 가능해 번들 최적화에 유리하다.

### 2. 내보내기/가져오기 특성

- CommonJS: `module.exports` 하나의 객체를 내보냄
- ES Module: `named export`, `default export`를 지원

```javascript
// ESM default export
export default function log(msg) {
  console.log(msg);
}

// ESM default import
import log from './log.js';
```

### 3. 동기/비동기 특성

- CommonJS: 기본적으로 동기 로딩
- ES Module: 비동기 로딩 구조를 가질 수 있음 (`import()`)

```javascript
const module = await import('./feature.js');
```

### 4. this와 스코프

- CommonJS: 파일이 함수 스코프로 감싸져 동작
- ES Module: 파일 자체가 모듈 스코프, top-level `this`는 `undefined`

### 5. 트리 셰이킹(Tree Shaking)

- CommonJS: 정적 분석이 어려워 트리 셰이킹에 불리
- ES Module: 정적 import/export 기반이라 트리 셰이킹에 유리

## Node.js에서의 사용

Node.js는 둘 다 지원하지만 구분 규칙이 있다.

- `.mjs` 파일: ESM
- `.cjs` 파일: CommonJS
- `package.json`의 `"type": "module"`이면 `.js`를 ESM으로 해석

## 실무 선택 기준

- 프론트엔드 번들링/최적화 중심 프로젝트: ESM 권장
- 기존 Node.js 레거시 프로젝트와 호환성 중심: CommonJS 유지 가능
- 신규 프로젝트: 가능하면 ESM 기준으로 시작

## 정리

- CommonJS는 Node.js 중심의 전통 모듈 시스템이다.
- ES Module은 JavaScript 표준이며 정적 분석과 최적화에 강하다.
- 현재 생태계는 점점 ESM 중심으로 이동 중이다.

## 면접 답변용 한 줄 정리

CommonJS는 런타임 `require` 기반의 전통 Node 모듈 시스템이고, ES Module은 정적 `import/export` 기반의 표준 모듈 시스템으로 트리 셰이킹과 현대 빌드 최적화에 더 유리하다.
