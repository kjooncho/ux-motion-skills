---
name: scroll-sequencing
description: 스크롤 구동 내러티브(릴·쇼케이스·케이스 스터디·3D 씬)의 시퀀싱 판단 레시피. 실제 제작 릴(30씬·클립 54)에서 역검증된 규칙 7개 — 가중치 비균등 타임라인, 구간 타입별 이징·스크럽 차등, 스크롤 직결 vs lerp 정착 이원화, 세그먼트 상태 파생, 히스테리시스 스왑, 프로브 내장 — 와 드라이버 독립 코어 스니펫. 스크롤 스토리텔링·스크럽 애니메이션·three.js/R3F 카메라 시퀀스를 설계·구현할 때 활성화. "스크롤 인터랙션 만들어줘", "스크럽이 밋밋해", "스크롤에 씬 전환 붙여줘" 등. Trigger in any language — e.g. "scroll-driven story", "scrub feels floaty", "sync scenes to scroll", "camera moves on scroll", "スクロール連動アニメ", "スクラブが物足りない".
metadata:
  author: Kyoungjoon Cho (kjooncho)
  version: 1.0.0-public
---

# Scroll Sequencing — 스크롤 내러티브의 리듬을 코드 계약으로

## Overview

스크롤 진행도를 주는 도구는 많다(drei `ScrollControls`, lenis, GSAP ScrollTrigger). 곡선 카메라 경로도 있다(drei `MotionPathControls`). **없는 것은 리듬 계층이다** — 어떤 구간이 길고 짧아야 하는지, 구간마다 이징과 스크럽이 어떻게 달라야 하는지, 어떤 값이 스크롤에 직결되고 어떤 값이 느슨하게 정착해야 하는지. 이 레시피는 그 계층의 판단 규칙이다. 값들은 실제 제작된 스크롤 릴(30씬·클립 54·세그먼트 70+)에서 사용자 판정을 거치며 역검증됐다 — "이렇게 하면 좋다"가 아니라 "이렇게 안 했더니 밋밋했고, 이렇게 바꾸니 살았다"의 기록이다.

**적용 지면:** 스크롤 스토리텔링(포트폴리오 릴·제품 쇼케이스·케이스 스터디), three.js/R3F 씬 시퀀스. **오버스펙인 지면:** 단순 섹션 스냅, 단일 패럴랙스, 일반 문서 페이지 — 거기는 CSS scroll-snap이나 단일 IntersectionObserver로 충분하다.

## 판단 규칙 7

### 1. 타임라인은 가중치 비균등으로 선언한다
균등 분할(씬 N개 = 1/N씩)은 길이만 긴 한 동작이 된다. **리듬은 값이 아니라 값의 차이에서 나온다.** 구간마다 가중치를 주고 정규화해서 진행도 구간을 만든다. 실전값: 인트로 0.6 · 단독 체류 1.0 · 챕터 타이틀 0.7 · 챕터 내부 체류 0.55 · 챕터 간 전환 0.5 · 내부 전환 0.32. 스크롤 트랙 총 높이도 이 가중치 합에서 역산한다(구간당 ~155vh).

### 2. 이징은 구간 "타입"에 붙인다
전 구간이 시그니처 커브 하나로 굴러가면 안 된다. 타입별 차등 — 시그니처 `cubic-bezier(0.45, 0, 0.35, 1)`(기본 체류·등장) · 내부 컷 `(0.62, 0, 0.28, 1)`(짧고 타이트 — 늦게 출발해 빨리 붙는다) · 챕터 간 브릿지 `(0.22, 0, 0.12, 1)`(일찍 풀리고 길게 정착). 커브 계보는 `motion-judgment` 원칙 1(지면 판정·비대칭 기본)을 따른다.

### 3. 스크럽 강도도 구간 타입별로 차등한다
스크럽 = 스크롤 목표값을 따라가는 lerp 계수(클수록 즉응 — GSAP `scrub: true` ↔ `scrub: 0.5`의 스펙트럼). 실전값: 인트로 5 · 체류 7 · **전환 10.5**(가장 즉응 — 손맛이 여기서 난다) · 챕터 간 전환 5.5(무게) · 챕터 타이틀 4(가장 느슨 — 쉼표). 스크럽 계수는 "직전 프레임의 구간"으로 고른다 — 구간 판정 자체가 스무딩된 값에 의존하기 때문.

### 4. 전환 값은 스크롤에 직결하고, 체류 값만 lerp로 정착시킨다
이 레시피에서 가장 비싼 교훈. 전환(카메라·오브젝트가 무대를 가로지르는 축)을 lerp로 목표를 뒤따르게 하면 "건너감"이 "따라옴"이 되고, 스크롤을 멈춘 만큼 정확히 멈추지도 않는다 — 밋밋함의 정체가 대부분 이것이다. **전환 축 = 구간 진행도(이징 적용)에서 직접 계산. 체류 축 = 별도 lerp(계수 ~7)로 정착.** 둘을 한 변수로 섞지 말 것.

### 5. 상태는 전부 세그먼트에서 파생시킨다
비디오 재생 게이트, 텍스트 패널 on/out, 조명 방향, 강조색 — 개별 스크롤 리스너를 달지 말고 **현재 세그먼트 하나에서 전부 파생**시킨다. 타임라인이 단일 진실이면 구간을 조정할 때 상태 로직이 공짜로 따라온다.

### 6. 전환 중 콘텐츠 스왑은 히스테리시스로
전환 중간(t=0.5)에서 이전/다음 콘텐츠를 교체하면 경계에서 스크롤 미세 왕복에 깜빡인다. 스왑 임계를 상태에 따라 이원화한다: 아직 안 바꿨으면 t ≥ 0.52에서, 이미 바꿨으면 t ≥ 0.48 유지 — 0.04의 완충이 떨림을 없앤다.

### 7. 프로브를 내장한다
`window.__probe = () => ({ seg, type, t, scrub, ease, … })` 하나가 헤드리스 캡처·상태 검증·성능 회귀를 전부 가능하게 한다. 시퀀스를 만들 때 관측 수단을 함께 만들 것 — "보이는 대로"가 아니라 값으로 검증한다.

### + reduced motion
`prefers-reduced-motion`이면 스크럽 lerp를 즉시 수렴(계수 → 1)시키고 앰비언트(부유·기울임)를 끈다. 정보 전달(어느 씬인가)은 유지된다 — 모션만 빠진다.

## 코어 스니펫 — 드라이버·렌더러 독립 (~90줄)

progress(0~1)를 주는 어떤 드라이버(scrollY, lenis, drei `useScroll().offset`)와도, 어떤 렌더러(three/R3F/DOM)와도 결합된다.

```js
function cubicBezier(p1x, p1y, p2x, p2y) {
  const cx = 3 * p1x, bx = 3 * (p2x - p1x) - cx, ax = 1 - cx - bx;
  const cy = 3 * p1y, by = 3 * (p2y - p1y) - cy, ay = 1 - cy - by;
  const sx = t => ((ax * t + bx) * t + cx) * t;
  const dx = t => (3 * ax * t + 2 * bx) * t + cx;
  return x => {
    let t = x;
    for (let i = 0; i < 5; i++) { const d = dx(t); if (Math.abs(d) < 1e-6) break; t -= (sx(t) - x) / d; }
    return ((ay * t + by) * t + cy) * t;
  };
}
const clamp01 = v => Math.min(1, Math.max(0, v));

/**
 * units: [{ type, id?, w, ease?, scrub? }]  — 순서 = 타임라인
 * eases: { [name]: [p1x,p1y,p2x,p2y] }      — 타입 기본 이징은 easeOf로 매핑
 * scrub: { [type]: number }                  — 타입별 lerp 계수 (규칙 3)
 */
function defineSequence({ units, eases, scrub, easeOf }) {
  const fns = Object.fromEntries(Object.entries(eases).map(([k, v]) => [k, cubicBezier(...v)]));
  const totalW = units.reduce((a, u) => a + u.w, 0);
  let acc = 0;
  const stops = units.map(u => ({ ...u, end: (acc += u.w) / totalW }));
  const segOf = p => { const i = stops.findIndex(s => p < s.end); return i < 0 ? stops.length - 1 : i; };

  let smooth = 0, swap = false, lastSeg = -1;
  const subs = { enter: [], exit: [] };

  return {
    stops, totalW,
    on: (ev, fn) => subs[ev].push(fn),
    trackHeightVh: perUnitVh => Math.round(totalW * perUnitVh),   // 규칙 1 — 트랙 높이 역산
    sample(target, dt, { reduced = false } = {}) {
      // 규칙 3 — 스크럽 계수는 직전 프레임 구간으로
      const prev = stops[segOf(smooth)];
      const k = prev.scrub ?? scrub[prev.type] ?? 7;
      smooth += (target - smooth) * (reduced ? 1 : Math.min(1, dt * k));

      const i = segOf(smooth), stop = stops[i];
      const start = i === 0 ? 0 : stops[i - 1].end;
      const t = clamp01((smooth - start) / (stop.end - start));
      const easeName = stop.ease ?? (easeOf ? easeOf(stop) : 'sig');
      const eased = (fns[easeName] ?? (x => x))(t);

      // 규칙 6 — 히스테리시스 스왑 (전환 구간에서만 의미)
      const swapOn = t >= (swap ? .48 : .52);
      swap = stop.type === 'tr' ? swapOn : false;

      if (i !== lastSeg) {   // 규칙 5 — 상태 파생은 여기 하나로
        if (lastSeg >= 0) subs.exit.forEach(f => f(stops[lastSeg], i));
        subs.enter.forEach(f => f(stop, i));
        lastSeg = i;
      }
      // eased = 전환 직결 축에 그대로 (규칙 4 전반). 체류 정착 축은 이 값과 별도로
      // 소비자가 lerp(계수 ~7)를 건다 (규칙 4 후반) — 한 변수로 섞지 말 것.
      return { index: i, type: stop.type, id: stop.id, t, eased, progress: smooth, swap };
    },
  };
}
```

사용 예 (three.js든 R3F `useFrame`이든 동일):

```js
const seq = defineSequence({
  eases: { sig: [.45, 0, .35, 1], inner: [.62, 0, .28, 1], bridge: [.22, 0, .12, 1] },
  scrub: { intro: 5, hold: 7, tr: 10.5, arc: 4 },
  easeOf: stop => stop.type === 'tr' ? (stop.bridge ? 'bridge' : 'inner') : 'sig',
  units: [
    { type: 'intro', w: .6 },
    { type: 'hold', id: 's0', w: 1 },
    { type: 'tr', id: 's0>s1', w: .5, bridge: true, scrub: 5.5 },
    { type: 'hold', id: 's1', w: .55 },
  ],
});
seq.on('enter', stop => { /* 재생 게이트·패널·조명 파생 (규칙 5) */ });

// 프레임 루프:
const s = seq.sample(rawScrollProgress, dt, { reduced });
if (s.type === 'tr') rig.position.x = lerpPath(from, to, s.eased);   // 직결 (규칙 4)
else rigX += (targetX - rigX) * Math.min(1, dt * 7);                 // 정착 (규칙 4)
```

## Provenance

규칙과 수치는 2026년 제작 스크롤 릴(30씬·클립 54)의 저작 과정에서 나왔다 — 특히 규칙 4는 "전환이 밋밋하다"의 원인을 사용자 판정으로 추적해 얻은 것이고, 규칙 1·3의 수치는 그 릴에서 리듬이 성립할 때까지 조정된 최종값이다. 커브 자체의 계보는 [`motion-judgment`](../motion-judgment)의 아카이브(duration 표본 51,271건·베지어 61,422건)와 같다. 이 레시피가 당신 지면에서 실패하면 반례를 먼저 기록하고, 규칙의 *이유*가 여전히 성립하는지 본 다음 수치를 고칠 것.
