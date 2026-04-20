# 쿠키, 로컬스토리지, 세션스토리지

브라우저 저장 메커니즘은 인증, 사용자 설정, 임시 상태 유지에서 자주 비교되는 주제다.

핵심은 다음 3가지다.

- 쿠키(Cookie): 서버와 자동으로 주고받는 작은 데이터
- 로컬스토리지(LocalStorage): 브라우저에 오래 남는 클라이언트 저장소
- 세션스토리지(SessionStorage): 탭(세션) 단위로 유지되는 클라이언트 저장소

## 한눈에 비교

| 항목       | 쿠키                         | 로컬스토리지               | 세션스토리지                          |
| ---------- | ---------------------------- | -------------------------- | ------------------------------------- |
| 저장 위치  | 브라우저                     | 브라우저                   | 브라우저                              |
| 전송 여부  | HTTP 요청마다 자동 전송 가능 | 자동 전송 안 됨            | 자동 전송 안 됨                       |
| 만료 시점  | 직접 설정(Expires/Max-Age)   | 직접 삭제 전까지 유지      | 탭/창 종료 시 삭제                    |
| 용량(대략) | 약 4KB                       | 약 5MB 내외                | 약 5MB 내외                           |
| 접근 범위  | 도메인/경로 기준             | 같은 출처(Origin)          | 같은 출처 + 같은 탭                   |
| 주요 용도  | 인증 세션, 서버 연동 상태    | 사용자 설정, 캐시성 데이터 | 일회성 입력 상태, 탭 단위 임시 데이터 |

## 1) 쿠키(Cookie)

쿠키는 서버가 `Set-Cookie` 헤더로 내려주고, 브라우저가 조건에 맞는 요청에 자동으로 붙여 보내는 데이터다.

### 주요 속성

- `Expires` / `Max-Age`: 만료 시간
- `Domain`, `Path`: 전송 범위
- `Secure`: HTTPS에서만 전송
- `HttpOnly`: JavaScript 접근 차단(document.cookie로 읽기 불가)
- `SameSite`: 크로스 사이트 요청에서 쿠키 전송 제어

### 장점

- 서버 인증 흐름과 자연스럽게 연동됨
- `HttpOnly`로 XSS 상황에서 토큰 탈취 위험을 줄일 수 있음

### 주의점

- 요청마다 자동 전송되어 트래픽이 늘어날 수 있음
- 설정이 잘못되면 CSRF 위험이 커질 수 있음

## 2) 로컬스토리지(LocalStorage)

로컬스토리지는 같은 출처 내에서 브라우저에 비교적 오래 남는 키-값 저장소다.

```javascript
localStorage.setItem('theme', 'dark');
const theme = localStorage.getItem('theme');
localStorage.removeItem('theme');
```

### 특징

- 브라우저를 닫아도 유지됨
- 문자열 기반 저장소(객체는 `JSON.stringify`/`JSON.parse` 필요)
- JavaScript에서만 접근 가능(요청 자동 전송 없음)

### 주의점

- XSS가 발생하면 스크립트로 읽힐 수 있음
- 민감한 인증 토큰 저장소로는 신중해야 함

## 3) 세션스토리지(SessionStorage)

세션스토리지는 탭 단위로 유지되는 키-값 저장소다.

```javascript
sessionStorage.setItem('draft', '작성중인 글');
const draft = sessionStorage.getItem('draft');
sessionStorage.removeItem('draft');
```

### 특징

- 탭/창을 닫으면 데이터가 사라짐
- 같은 사이트라도 탭이 다르면 저장소도 분리됨
- 문자열 기반 저장소

### 적합한 사례

- 결제/가입 폼의 임시 입력값
- 탭 단위 플로우 상태 유지

## 보안 관점 요약

### XSS

- 로컬스토리지/세션스토리지는 JavaScript로 접근 가능하므로 XSS에 취약할 수 있음
- 민감 정보는 가능하면 `HttpOnly` 쿠키 사용을 우선 검토

### CSRF

- 쿠키는 자동 전송 특성 때문에 CSRF 대응이 필요함
- `SameSite`, CSRF 토큰, Origin/Referer 검증을 함께 사용

## 언제 무엇을 쓰면 좋나?

- 서버 인증 세션: 쿠키(특히 `HttpOnly`, `Secure`, 적절한 `SameSite`)
- 장기 사용자 설정(테마, 언어): 로컬스토리지
- 탭 생명주기와 같이 사라져야 하는 임시 상태: 세션스토리지

## 자주 하는 실수

1. 로컬스토리지에 JWT/민감정보를 무조건 저장하는 것
2. 쿠키 보안 속성(`Secure`, `HttpOnly`, `SameSite`)을 빠뜨리는 것
3. 저장소 용량 제한/만료 전략 없이 데이터가 계속 쌓이게 두는 것
4. 객체를 그대로 저장하려고 해서 `[object Object]`가 들어가는 것

```javascript
const user = { id: 1, name: 'Alice' };
localStorage.setItem('user', JSON.stringify(user));

const restoredUser = JSON.parse(localStorage.getItem('user') || '{}');
console.log(restoredUser.name);
```

## 정리

- 쿠키는 서버 통신과 인증 중심, 자동 전송되는 저장 메커니즘이다.
- 로컬스토리지는 장기 클라이언트 저장, 세션스토리지는 탭 단위 임시 저장에 적합하다.
- 보안은 저장소 선택보다 XSS/CSRF 대응 전략과 속성 설정이 핵심이다.

## 면접 답변용 한 줄 정리

쿠키는 서버와 자동 전송되는 인증 친화 저장소이고, 로컬스토리지는 장기 클라이언트 저장, 세션스토리지는 탭 단위 임시 저장에 쓰며, 실제 설계의 핵심은 XSS·CSRF를 고려한 보안 설정이다.
