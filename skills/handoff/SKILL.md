---
name: handoff
description: Figma 또는 디자인 정보를 받아 개발자용 사양서를 생성한다. 엣지케이스 11종 + 토큰 + Analytics 이벤트 포함.
---

# Design Handoff

개발자가 판단·추측하지 않아도 되는 사양서. "이거 어떻게 하면 되나요?" 질문이 없어야 성공.

## 절차

1. **디자인 맥락 1회 수집 → compact brief** — Figma/디자인 정보(스크린샷·variable·메타데이터)를 **한 번만** 읽어 화면 목적·핵심 컴포넌트·토큰·주요 플로우를 짧은 brief로 정리한다. 이후 단계엔 원본이 아니라 이 brief만 전달한다.
   - brief에 **뷰잉 컨텍스트 1줄 필수**: 이 화면이 실행되는 디바이스·시청 거리·주목 시간(예: "폰 잠금화면, 30cm, 2초 스침" vs "데스크톱 대시보드, 상시 주시"). 위계·모션의 1차 인식 요소가 여기서 도출된다 — 없으면 개발자가 30cm 포토샵 기준으로 추측한다.
2. **엣지케이스 11종 병렬 정의 (서브에이전트)** — 11종은 서로 독립이므로 각각 `Agent` 서브에이전트에 위임해 병렬로 정의한다. 메인이 11종을 순차 열거하지 않는다.

   **서브에이전트 프롬프트 템플릿 (엣지케이스당 1회):**
   > 아래 화면 brief를 전제로 `<엣지케이스명>` 상태의 사양을 정의하라. compact JSON 한 줄로만 반환:
   > `{"case":"<엣지케이스명>","ui":"<무엇을 보여주나>","logic":"<트리거·조건>","message":"<카피, 있으면>","fallback":"<복구 동작>"}`
   > (brief 전문은 메인이 STEP 1에서 만든 것을 그대로 붙여 전달)

   컴포넌트가 많으면 컴포넌트 명세도 같은 방식으로 병렬 위임 가능.
3. **메인이 조립** — 수집한 JSON 배열로 아래 출력 구조를 채운다(엣지케이스 원문 분석은 서브에이전트에서 소비·폐기, 메인 context엔 compact 결과만).
4. **정당화 게이트 (출력 직전)** — **핵심 값 우선**(사용자가 처음 보는 요소·인터랙션 파라미터·토큰 이탈값), 나머지는 토큰 준수 확인으로 충분. 각 핵심 값에 "왜 이 값인가"를 1줄로 답할 수 있는지 점검한다. 답 못 하는 값 = 임의값(취향)일 가능성 — 토큰으로 치환하거나 사유를 "개발자 질문 방지 메모"에 적는다. 정당화 안 되는 값을 그대로 내보내면 개발자 질문이 거기서 나온다.

## 출력 구조

**컴포넌트 명세**
- 컴포넌트명 (PascalCase) / Props·변형 목록
- 상태: default · hover · active · disabled · loading

**디자인 토큰**
- spacing: Npx (8px 배수) · border-radius: Npx · 색상: hex + 용도
- 모션: duration + easing

**엣지케이스 11종** (전부 정의)
Empty · Loading · Error · Success · Offline · Permission Denied · First-time · Returning · Interrupted · Concurrent · Degraded

**Analytics 이벤트**
- 이벤트명: snake_case 동사_명사 (예: button_tapped, screen_viewed)
- 공통 속성: user_id · session_id · platform

**개발자 질문 방지 메모**: 판단 여지가 있는 사항 명시
