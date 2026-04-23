# CORS란?

CORS(Cross-Origin Resource Sharing)는 **다른 출처(Origin) 간의 HTTP 요청을 브라우저가 어떻게 허용할지 정하는 보안 정책**이다.

기본적으로 브라우저는 SOP(Same-Origin Policy)에 따라 다른 출처의 리소스 접근을 제한한다. CORS는 이 제한을 **서버가 응답 헤더로 예외 허용**하는 방식이다.

## Origin(출처)이란?

출처는 다음 3가지 조합으로 결정된다.

- 프로토콜(`http`, `https`)
- 호스트(`example.com`)
- 포트(`:3000`)

이 중 하나라도 다르면 다른 출처로 본다.

예:

- `https://example.com` 과 `https://example.com:3000` -> 다른 출처
- `http://example.com` 과 `https://example.com` -> 다른 출처

## 왜 필요한가?

사용자가 로그인된 상태에서 악성 사이트가 임의로 다른 사이트에 요청을 보내고 응답까지 읽을 수 있다면 큰 보안 문제가 생긴다.

브라우저는 이를 막기 위해 기본적으로 교차 출처 응답 읽기를 제한하고, 서버가 허용한 경우에만 응답을 노출한다.

## 동작 방식

### 단순 요청(Simple Request)

조건을 만족하는 일부 요청은 바로 전송되고, 서버 응답의 CORS 헤더를 보고 브라우저가 응답 접근 가능 여부를 판단한다.

대표 응답 헤더:

- `Access-Control-Allow-Origin`
- `Access-Control-Allow-Credentials`

## 프리플라이트 요청(Preflight)

다음과 같은 경우에는 실제 요청 전에 브라우저가 `OPTIONS` 요청을 먼저 보낸다.

- `PUT`, `DELETE` 같은 메서드 사용
- 커스텀 헤더 사용
- 특정 `Content-Type` 이외의 타입 사용

서버는 다음 헤더 등으로 허용 범위를 알려준다.

- `Access-Control-Allow-Origin`
- `Access-Control-Allow-Methods`
- `Access-Control-Allow-Headers`
- `Access-Control-Max-Age`

프리플라이트가 통과해야 실제 요청이 전송된다.

## 자주 하는 실수

### 1. CORS를 프론트 문제라고만 생각함

CORS는 브라우저가 검사하지만, 허용 여부는 **서버 응답 헤더**로 결정된다.

### 2. `Access-Control-Allow-Origin: *` 와 credentials를 같이 쓰려 함

쿠키나 인증 정보를 포함하는 요청(`credentials: 'include'`)에서는 와일드카드(`*`)를 사용할 수 없다.

### 3. 서버는 정상인데 브라우저에서만 막히는 이유를 모름

Postman이나 서버 간 통신은 CORS 영향을 거의 받지 않지만, **브라우저 환경에서는 SOP/CORS 정책이 적용**된다.

## 정리

- CORS는 다른 출처 요청 자체를 막는 기술이 아니라, **응답을 브라우저가 읽을 수 있는지 제어하는 정책**이다.
- 브라우저가 SOP를 기본으로 적용하고, 서버가 CORS 헤더로 예외를 허용한다.
- 복잡한 요청은 프리플라이트(`OPTIONS`)를 먼저 보낸다.

## 면접 답변용 한 줄 정리

CORS는 브라우저의 동일 출처 정책을 전제로, 서버가 응답 헤더로 다른 출처의 응답 접근을 허용할지 결정하는 메커니즘이다.
