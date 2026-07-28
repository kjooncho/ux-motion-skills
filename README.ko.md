# ux-motion-skills

[English](./README.md) | **한국어** | [日本語](./README.ja.md)

[![skills.sh](https://skills.sh/b/kjooncho/ux-motion-skills)](https://skills.sh/kjooncho/ux-motion-skills)

**18년 경력 중 최근 11년의 실무 UX 모션 작업에서 증류한, Claude Code용 모션·디자인 판단 스킬.**

대부분의 디자인 스킬은 에이전트에게 *무엇을 만들지*를 알려줍니다. 이 스킬들은 *어떻게 결정할지*를 알려줍니다 — 값·임계치·트레이드오프가 실제 프로덕션 아카이브에서 역산됐습니다: **프로젝트 235개, duration 표본 51,271건, 베지어 커브 61,422건**, 채택 인터뷰 12라운드. 모든 원칙은 반례 검증을 통과한 것만 남겼고, 기각된 것은 싣지 않았습니다.

> 관련: 같은 아카이브·같은 수치로 작성한 모션 토큰 표준화 구현 노트 — [google-labs-code/design.md#47](https://github.com/google-labs-code/design.md/issues/47#issuecomment-5103717402)

## 스킬

| 스킬 | 하는 일 | 쓰는 순간 |
|---|---|---|
| [`motion-judgment`](./skills/motion-judgment) | 역검증된 모션 판단 원칙 20개 — 이징·duration·스프링·지면/채택 판단·프로세스 | 모든 모션 결정: "이징 뭐 쓸까", "얼마나 길게?", "스프링? 이즈?" |
| [`motion-guide`](./skills/motion-guide) | 개발팀 인터랙션 가이드 문서 생성 — 전환·컴포넌트 애니메이션·Lottie 명세·상태 맵 | 디자인 끝, 핸드오프에 "부드럽게" 대신 수치가 필요할 때 |
| [`handoff`](./skills/handoff) | 개발자 사양서 생성 — 엣지케이스 11종·토큰·Analytics 이벤트·핵심 값 정당화 게이트 | 후속 질문 폭탄 없이 화면을 개발에 넘길 때 |
| [`design-critique`](./skills/design-critique) | Nielsen 휴리스틱 10원칙 기반 스크린샷/디자인 비평, 심각도 분류 | 감이 아니라 구조화된 2차 의견이 필요할 때 |
| [`ai-slop-detector`](./skills/ai-slop-detector) | UI의 "AI 평균치" 신호를 검출하고 차별화 방향 제안 | 내 화면이 남들 AI 산출물과 똑같아 보일 때 |
| [`concept-gate`](./skills/concept-gate) | 아이디어/design.md의 문제 적합성·빠진 사용자 비평 | 무엇이든 만들기 전에 |

**언어 안내:** 스킬 본문은 한국어이며, 이 리포의 **정본(canonical)** 언어입니다 — README 번역본은 최선 노력 미러입니다. Claude는 한국어 스킬을 그대로 읽고 대화 언어가 무엇이든 적용하며, 모든 스킬의 트리거 설명에 영어·일본어 문구가 포함되어 3개 언어 모두에서 활성화됩니다. 스킬 본문 번역은 수요가 있으면 진행합니다([이슈로 요청](https://github.com/kjooncho/ux-motion-skills/issues)).

## 설치

**방법 1 — skills CLI:**

```bash
npx skills add kjooncho/ux-motion-skills
```

**방법 2 — Claude Code 플러그인 마켓플레이스:**

```
/plugin marketplace add kjooncho/ux-motion-skills
```

**방법 3 — 수동 복사 (스킬 하나만):**

```bash
git clone https://github.com/kjooncho/ux-motion-skills
cp -r ux-motion-skills/skills/motion-judgment ~/.claude/skills/
```

설치 후에는 그냥 작업을 설명하면 됩니다("모션 만들어줘" / "이징 뭐 쓸까") — 트리거 맥락에서 자동 활성화되고, user-invocable 표시된 스킬은 `/motion-guide`처럼 직접 호출할 수 있습니다.

## 출처와 방법

이 스킬들은 비공개 결정 로그의 컴파일본입니다. 로그에는 원칙별 근거·통과한 검증 라운드·초안을 수정/기각시킨 반례가 기록되어 있고, 컴파일된 수치는 스킬 안에 내재화했습니다(원본 로그는 비공개). 자신의 환경에서 원칙이 깨지면 그것을 발견으로 취급하세요 — 반례를 먼저 기록하고, 원칙의 *이유*가 여전히 성립하는지 검증한 뒤에 규칙을 고치는 것. 이 검증 루프가 이 세트를 신뢰할 수 있게 만든 방법이고, 당신의 포크에도 똑같이 적용됩니다.

## 저자

**조경준 (Kyoungjoon Cho)** — UX 모션 디자이너 — 경력 18년, 최근 11년은 프로덕션 모션/인터랙션 디자인. GitHub [@kjooncho](https://github.com/kjooncho).

## 라이선스

MIT — [LICENSE](./LICENSE) 참조.
