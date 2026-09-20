# R3F 씬 타임라인 — drei 위에 얹는 시퀀싱 레이어 (초안)

**성격:** [`SKILL.md`](./SKILL.md) 판단 규칙 7종·코어 스니펫의 R3F/drei 결합부 레시피. 규칙과 수치 자체는 SKILL.md가 정본 — 이 문서는 반복하지 않고 결합 지점만 다룬다.
**검증 상태:** N=1 (reel30 해부 1건, 그것도 R3F가 아니라 바닐라 three.js). 아래 drei 결합은 스케치이지 실증이 아니다 — 표기는 문서 끝 "미검증" 절 참조.

## 전제 (정정판)

구 전제 — "theatre-js 계열 생태계 전멸, 자작이 유일 경로" — 는 틀렸다. `pmndrs/drei`(★9,798·활발)에 `MotionPathControls`·`ScrollControls`·`CameraControls`·`CameraShake`가 실재한다.

죽은 것은 **기능이 아니라 두 형태**다 — GUI 타임라인 에디터(theatre.js)와 DOM↔3D 래퍼(r3f-scroll-rig·three-story-controls). 정정 후에도 남는 공백은 "씬 단위 리듬을 코드로 선언하는 저작 계층"이고, 이건 이미 SKILL.md의 규칙 7종·`defineSequence` 코어가 채운다(가중치 비균등 타임라인·구간별 이징/스크럽 차등·직결/정착 이원화·세그먼트 상태 파생·히스테리시스·프로브). 이 레시피의 범위는 **그 코어를 drei 프리미티브에 배선하는 지점**뿐이다.

## drei 결합 지점

| drei 프리미티브 | 역할 | `seq`와의 결합 |
|---|---|---|
| `ScrollControls` / `useScroll().offset` | progress 공급 (0~1) | `seq.sample(offset, dt)`의 target 인자로 그대로 투입 — SKILL.md 코어는 progress 소스에 무관 |
| `MotionPathControls` | 곡선 카메라 경로 (focus·offset·damping) | 전환 구간에서 `s.eased`(직결 축, 규칙 4)를 경로 진행률로 공급 — 정착 축은 별도 lerp이므로 이 컨트롤에 넣지 않는다 |
| `CameraControls` | 직접 카메라 조작(lookAt 등) | 세그먼트 `enter` 콜백에서 목표값 세팅(규칙 5 — 상태는 세그먼트에서 파생) |
| `CameraShake` | 앰비언트 흔들림 | `prefers-reduced-motion`에서 끄는 대상 — SKILL.md "+ reduced motion" 항목과 동일 취급 |

## 결합 스케치 (요약 — 전체 구현 아님)

```jsx
function SceneRig() {
  const offset = useScroll().offset;           // drei ScrollControls가 공급하는 progress
  const path = useRef();
  useFrame((_, dt) => {
    const s = seq.sample(offset, dt);            // SKILL.md 코어, 그대로
    if (s.type === 'tr') path.current?.moveAlongPath(s.eased);   // 직결 축 → MotionPathControls
    else settleX.current += (targetX(s) - settleX.current) * Math.min(1, dt * 7); // 정착 축은 별도 lerp
  });
  return <MotionPathControls ref={path} curves={curves} />;
}
```

## 미검증 표기 (N=1 — 게이트 4)

- 근거는 reel30 1건뿐이고, 그 reel30조차 R3F가 아니라 바닐라 three.js 단일 파일(1,091줄) 해부다.
- **R3F 실구동 0회 · drei `useScroll` 결합 실증 0회 · reel30 밖 두 번째 사용처 0개.**
- 위 결합 표·스케치는 drei 공식 API 시그니처 기준으로 구성한 것이지, 실제로 돌려서 확인한 결과가 아니다.
- 채택 전 최소 R3F 데모 1개에 태워 확인할 것. 반례가 나오면 이 문서를 먼저 고치고 SKILL.md 규칙 자체는 건드리지 않는다(결합부 문제와 코어 규칙 문제를 분리).

## 관련

- 판단 규칙 7종·수치·코어 스니펫(드라이버·렌더러 독립): [`SKILL.md`](./SKILL.md)
- 스파이크 원본: `claude_laboratory/_research/r3f-scroll-sequencing-spike-2026-08.md`
