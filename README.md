# ux-motion-skills

**English** | [한국어](./README.ko.md) | [日本語](./README.ja.md)

[![skills.sh](https://skills.sh/b/kjooncho/ux-motion-skills)](https://skills.sh/kjooncho/ux-motion-skills)

**Motion & design judgment skills for Claude Code, distilled from 11 years of production UX motion work.**

Most design skills tell an agent *what to make*. These skills tell it *how to decide* — the values, thresholds, and trade-offs are back-derived from a real production archive: **235 projects, 51,271 duration samples, 61,422 bezier curves**, and 12 rounds of adoption interviews. Every principle survived counter-example review; rejected ones were kept out.

> Related: my implementation notes on motion token standardization in [google-labs-code/design.md#47](https://github.com/google-labs-code/design.md/issues/47#issuecomment-5103717402) — same archive, same numbers.

## Skills

| Skill | What it does | Use it when |
|---|---|---|
| [`motion-judgment`](./skills/motion-judgment) | 20 verified motion decision principles — easing, duration, springs, surface/adoption judgment, process | Any motion decision: "which easing?", "how long?", "spring or ease?" |
| [`motion-guide`](./skills/motion-guide) | Generates developer-ready interaction guide docs — transitions, component animation, Lottie specs, state maps | Design is done, dev handoff needs numbers instead of "make it smooth" |
| [`handoff`](./skills/handoff) | Developer spec generator — 11 edge cases, tokens, analytics events, a justification gate for every key value | Handing a screen to engineers without a follow-up question storm |
| [`design-critique`](./skills/design-critique) | Heuristic critique of screenshots/designs against Nielsen's 10 principles, sorted by severity | You want a structured second opinion, not vibes |
| [`ai-slop-detector`](./skills/ai-slop-detector) | Detects "AI-average" visual signals in a UI and suggests differentiation | Your interface looks like everyone else's AI output |
| [`concept-gate`](./skills/concept-gate) | Problem-fit critique for an idea or design.md before you build | Before committing to building anything |

**Language note:** skill bodies are written in Korean, which is the **canonical** language of this repo — README translations are best-effort mirrors. Claude reads Korean natively and applies the skills in whatever language your conversation uses; every skill's trigger description includes English and Japanese phrases, so activation works in all three languages. Full skill-body translations may follow if there's demand ([open an issue](https://github.com/kjooncho/ux-motion-skills/issues)).

## Install

**Option 1 — skills CLI:**

```bash
npx skills add kjooncho/ux-motion-skills
```

**Option 2 — Claude Code plugin marketplace:**

```
/plugin marketplace add kjooncho/ux-motion-skills
```

**Option 3 — manual copy (any single skill):**

```bash
git clone https://github.com/kjooncho/ux-motion-skills
cp -r ux-motion-skills/skills/motion-judgment ~/.claude/skills/
```

Then just describe your task ("모션 만들어줘" / "which easing should this use?") — skills activate on their trigger contexts, or invoke directly with `/motion-guide` style commands where marked user-invocable.

## Provenance & method

These skills are compiled from a private decision log that records, per principle: the supporting evidence, the verification round it passed, and the counter-examples that modified or killed earlier drafts. The compiled numbers are embedded in the skills; the raw log stays private. If a principle fails in your environment, treat that as a finding — log the counter-example first, then check whether the principle's *reason* still holds, before editing the rule. That review loop is how this set got trustworthy, and it applies to your fork too.

## Author

**Kyoungjoon Cho (조경준)** — UX motion designer, 11 years in production motion/interaction design. GitHub [@kjooncho](https://github.com/kjooncho).

## License

MIT — see [LICENSE](./LICENSE).
