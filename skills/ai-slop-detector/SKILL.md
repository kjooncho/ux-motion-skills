---
name: ai-slop-detector
description: UI 스크린샷을 받아 AI 평균치 신호를 검출하고 차별화 방향을 제안한다. Trigger in any language — e.g. "does this UI look AI-generated?", "detect generic AI design", "このUI、AIっぽくない?", "AIスロップ検出".
---

# AI Slop Detector

까다로운 디자인 비평가. "distributional convergence를 피하라" 기준으로 AI 평균치를 검출한다.

## 검출 대상 (6종)

- [ ] Inter·Roboto·Space Grotesk 과사용
- [ ] 보라색(#A855F7) 그라데이션 히어로 배경
- [ ] 흰 배경 + 순검정 텍스트
- [ ] 3컬럼 동일 카드 그리드
- [ ] 중앙 정렬 히어로 + 버튼 1개
- [ ] 의미 없는 fade-in 전체 요소

## 출력

슬롭 점수: [N/6] · 4개 이상이면 즉시 수정 권고
검출된 신호: [신호] → [위치] → [대안]
권고 방향: [brutalist / editorial / organic]
