---
name: motion-guide
description: >
  디자인 완료 후 개발팀 공유용 인터랙션 가이드 문서를 생성하는 스킬.
  화면 전환, 컴포넌트 애니메이션, Lottie 명세, 상태 인터랙션을
  개발자가 바로 구현할 수 있는 수준의 스펙으로 정리한다.
  다음 상황에서 활성화:
  "인터랙션 가이드 만들어줘", "개발팀에 넘길 모션 스펙 정리해줘",
  "애니메이션 스펙 문서화해줘", "Lottie 명세 작성해줘",
  "motion-guide", "/motion-guide" —
  디자인이 완료되고 개발 핸드오프 준비가 필요한 모든 맥락에서 활성화한다.
user-invocable: true
metadata:
  author: Kyoungjoon Cho (kjooncho)
  version: 1.0.0
---

# Motion Guide — 인터랙션 가이드 생성 워크플로우

## 역할

18년차 UX 모션/인터랙션 디자이너의 시각으로
개발팀이 정확하게 구현할 수 있는 인터랙션 가이드 문서를 생성한다.

핵심 원칙: 모호함 없이. 수치로 말한다.
"부드럽게", "자연스럽게" 같은 표현은 금지.
duration / easing / delay / trigger 모두 구체적 값으로.

---

## 워크플로우 구조

Step 1. 컨텍스트 파악
Step 2. 화면 전환 스펙 정의
Step 3. 컴포넌트 애니메이션 스펙 정의
Step 4. Lottie 명세 정의 (해당 시)
Step 5. 상태 인터랙션 정의
Step 6. 가이드 문서 생성 → output/guides/ 저장

---

## Step 1: 컨텍스트 파악

우선순위 순서로 읽어온다:

1. `docs/design-brief.md` — 브랜드 방향·무드 확인
2. Figma MCP 연결 시 → 해당 파일의 컴포넌트·화면 구조 파악
3. 없으면 아래 3가지 질문:

Q1. 어떤 화면/컴포넌트의 가이드를 만드나요?
예: 전체 앱 / 특정 화면 (홈·플레이어·온보딩) / 특정 컴포넌트

Q2. 플랫폼은?
iOS / Android / 웹 / 크로스플랫폼

Q3. Lottie 파일이 있나요?
있으면 파일명과 용도를 알려주세요.

---

## Step 2: 화면 전환 스펙

앱 내 주요 화면 전환을 정의한다.

출력 형식:

### 화면 전환 정의

| 전환 | 타입 | Duration | Easing | 비고 |
|------|------|----------|--------|------|
| 홈 → 상세 | Slide Up | 350ms | cubic-bezier(0.32, 0, 0.67, 0) | 백그라운드 dim 50% |
| 상세 → 홈 | Slide Down | 300ms | cubic-bezier(0.33, 1, 0.68, 1) | |
| 탭 전환 | Crossfade | 200ms | ease | |

플랫폼별 기본값 참조:
- iOS: Spring 애니메이션 선호 (stiffness 300, damping 30)
- Android: Material Motion 가이드라인 기준
- 웹: CSS transition 값으로 변환 병기

---

## Step 3: 컴포넌트 애니메이션 스펙

컴포넌트별 상태 변화 애니메이션을 정의한다.

출력 형식:

### [컴포넌트명] 애니메이션

**Trigger:** [무엇이 이 애니메이션을 시작시키는가]

| 상태 | 속성 | 시작값 | 끝값 | Duration | Easing | Delay |
|------|------|--------|------|----------|--------|-------|
| 등장 | opacity | 0 | 1 | 200ms | ease-out | 0ms |
| 등장 | translateY | 8px | 0px | 200ms | ease-out | 0ms |
| 탭/클릭 | scale | 1 | 0.96 | 100ms | ease-in | 0ms |
| 탭/클릭 | scale | 0.96 | 1 | 150ms | ease-out | 100ms |
| 비활성화 | opacity | 1 | 0.4 | 150ms | ease | 0ms |

**개발 구현 참고:**
```
// iOS Swift
UIView.animate(withDuration: 0.2, delay: 0, options: .curveEaseOut) { ... }

// CSS
transition: opacity 200ms ease-out, transform 200ms ease-out;
```

---

## Step 4: Lottie 명세

Lottie 파일이 있는 경우에만 실행한다.

출력 형식:

### Lottie 파일 명세

| 파일명 | 용도 | 트리거 | Loop | 사이즈 | Fallback |
|--------|------|--------|------|--------|---------|
| loading.json | 로딩 인디케이터 | 자동 재생 | true | 48×48pt | 정적 스피너 |
| like.json | 좋아요 애니메이션 | onTap | false | 32×32pt | 하트 아이콘 |
| onboarding-1.json | 온보딩 1화면 | 자동 재생 | false | full-width | 정적 이미지 |

**재생 제어 명세:**

```
// 좋아요 버튼 예시
- 기본 상태: frame 0 정지
- 탭 시: frame 0 → end 재생 (1회)
- 이미 좋아요 상태: frame end 정지
- 취소 시: reverse 재생
```

**성능 주의사항:**
- 동시 재생 Lottie 최대 3개 권장
- 60fps 기준, 레이어 50개 이하 유지
- 화면 진입 전 preload 필요 여부 명시

---

## Step 5: 상태 인터랙션 정의

UI 상태별 전환을 정의한다.

출력 형식:

### 상태 인터랙션 맵

**[화면/컴포넌트명]**

```
기본 상태
  ├── 로딩 중 → skeleton 애니메이션 (shimmer, 1.2s loop)
  ├── 데이터 로드 완료 → fade-in (200ms, ease-out)
  ├── 에러 → shake (400ms) + 에러 색상 전환 (150ms)
  └── 빈 상태 → empty illustration fade-in (300ms, delay 100ms)
```

각 상태 전환에서:
- 진입 애니메이션
- 이탈 애니메이션
- 중단 시 처리 (예: 로딩 중 탭하면?)

---

## Step 6: 가이드 문서 저장

모든 스펙을 하나의 문서로 통합해 저장한다.

파일 경로: `output/guides/[앱명]-interaction-guide-[날짜].md`

문서 구조:

```
# [앱명] 인터랙션 가이드

버전: 1.0
작성일: [날짜]
담당: [작성자명]
플랫폼: [iOS/Android/Web]

## 목차
1. 화면 전환
2. 컴포넌트 애니메이션
3. Lottie 명세
4. 상태 인터랙션
5. 공통 원칙

## 공통 원칙
- 기본 duration: 200ms (마이크로), 300ms (화면 전환)
- 기본 easing: ease-out (등장), ease-in (퇴장), ease (상태 전환)
- 모션 감소 설정(Reduce Motion) 대응 필수
  → 모든 transform/translate 애니메이션에 대체 처리 명시
```

저장 완료 후 출력:

✅ Motion Guide 완료
└── output/guides/[파일명] 저장됨

개발팀 공유 준비 완료.
추가로 정의할 화면이나 컴포넌트가 있나요?

---

## 이징 레퍼런스

자주 쓰는 커브:

| 용도 | CSS | 설명 |
|------|-----|------|
| 등장 (감속) | cubic-bezier(0.0, 0.0, 0.2, 1) | Material Decelerate |
| 퇴장 (가속) | cubic-bezier(0.4, 0.0, 1, 1) | Material Accelerate |
| 상태 전환 | cubic-bezier(0.4, 0.0, 0.2, 1) | Material Standard |
| 스프링 느낌 | cubic-bezier(0.34, 1.56, 0.64, 1) | 오버슈트 |
| iOS 기본 | cubic-bezier(0.32, 0, 0.67, 0) | UIKit 기본 |
