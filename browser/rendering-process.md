# 브라우저 렌더링 과정(Browser Rendering Process)

## 전체 흐름

1. **HTML 파싱 -> DOM 생성**
2. **CSS 파싱 -> CSSOM 생성**
3. **DOM + CSSOM -> Render Tree 생성**
4. **Layout(Reflow)**: 각 요소의 크기와 위치 계산
5. **Paint**: 픽셀로 그리기
6. **Composite**: 레이어 합성 후 화면 출력

## 단계별 핵심

### 1. DOM 생성

브라우저가 HTML을 파싱해 노드 트리를 만든다.

### 2. CSSOM 생성

CSS를 파싱해 스타일 트리를 만든다.

- CSS는 기본적으로 렌더링 차단 리소스다.
- CSSOM이 완성되어야 정확한 스타일 계산이 가능하다.

### 3. Render Tree 생성

DOM과 CSSOM을 합쳐 실제로 렌더링할 노드만 구성한다.

- 예: `display: none` 요소는 Render Tree에 포함되지 않는다.

### 4. Layout (Reflow)

각 노드의 실제 위치와 크기를 계산한다.

- 레이아웃에 영향을 주는 속성 변경(`width`, `height`, `margin` 등)은 Reflow를 유발할 수 있다.

### 5. Paint

계산된 정보를 바탕으로 색, 글자, 그림자 등을 픽셀로 그린다.

- 시각 속성 변경(`color`, `background`)은 Paint를 유발할 수 있다.

### 6. Composite

여러 레이어를 GPU가 합성해 최종 화면을 만든다.

- `transform`, `opacity`는 보통 Layout 없이 Composite 단계에서 처리되어 상대적으로 효율적이다.

## JavaScript가 렌더링에 미치는 영향

- JavaScript는 DOM/CSSOM을 변경할 수 있어 렌더링 과정을 다시 발생시킨다.
- 동기 JavaScript는 HTML 파싱을 일시 중단시킬 수 있다.
- 스크립트 로딩 최적화를 위해 `defer`, `async`를 상황에 맞게 사용한다.

## Reflow와 Repaint 차이

- **Reflow(Layout)**: 크기/위치 재계산이 필요한 경우
- **Repaint(Paint)**: 위치 변화 없이 시각만 바뀌는 경우

일반적으로 Reflow가 Repaint보다 비용이 크다.

## 면접 답변용 한 줄 정리

브라우저는 HTML/CSS를 파싱해 DOM/CSSOM을 만들고, 이를 합친 Render Tree로 Layout, Paint, Composite를 거쳐 화면을 그린다. 성능 최적화의 핵심은 불필요한 Reflow/Repaint를 줄이는 것이다.
