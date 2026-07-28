# ux-motion-skills

[English](./README.md) | [한국어](./README.ko.md) | **日本語**

[![skills.sh](https://skills.sh/b/kjooncho/ux-motion-skills)](https://skills.sh/kjooncho/ux-motion-skills)

**18年のキャリアのうち直近11年のUXモーション実務から蒸留した、Claude Code向けモーション&デザイン判断スキル。**

多くのデザインスキルはエージェントに*何を作るか*を教えます。このスキルは*どう決めるか*を教えます — 値・しきい値・トレードオフは、実際のプロダクションアーカイブから逆算されたものです: **プロジェクト235件、duration サンプル51,271件、ベジェカーブ61,422件**、採用インタビュー12ラウンド。すべての原則は反例検証を通過したものだけを残し、棄却されたものは載せていません。

> 関連: 同じアーカイブ・同じ数値によるモーショントークン標準化の実装ノート — [google-labs-code/design.md#47](https://github.com/google-labs-code/design.md/issues/47#issuecomment-5103717402)

## スキル一覧

| スキル | 何をするか | 使う場面 |
|---|---|---|
| [`motion-judgment`](./skills/motion-judgment) | 検証済みモーション判断原則20 — イージング・duration・スプリング・採用判断・プロセス | あらゆるモーション判断:「イージングどれにする?」「どのくらいの長さ?」 |
| [`motion-guide`](./skills/motion-guide) | 開発チーム向けインタラクションガイド生成 — 画面遷移・コンポーネントアニメーション・Lottie仕様・状態マップ | デザイン完了後、「なめらかに」ではなく数値でハンドオフしたいとき |
| [`handoff`](./skills/handoff) | 開発者向け仕様書生成 — エッジケース11種・トークン・Analyticsイベント・キー値の正当化ゲート | 後追い質問の嵐なしで画面をエンジニアに渡すとき |
| [`design-critique`](./skills/design-critique) | Nielsenヒューリスティック10原則によるデザイン批評、深刻度分類 | 感覚ではなく構造化されたセカンドオピニオンが欲しいとき |
| [`ai-slop-detector`](./skills/ai-slop-detector) | UIの「AI平均値」シグナルを検出し差別化方向を提案 | 自分の画面が他のAI出力と同じに見えるとき |
| [`concept-gate`](./skills/concept-gate) | アイデア/design.mdの課題適合性・見落としユーザーを批評 | 何かを作り始める前に |

**言語について:** スキル本文は韓国語で書かれており、このリポジトリの**正本(canonical)**言語です — README翻訳はベストエフォートのミラーです。Claudeは韓国語スキルをそのまま読み、会話がどの言語でも適用します。各スキルのトリガー説明には英語・日本語のフレーズが含まれているため、3言語すべてで発動します。スキル本文の翻訳は需要があれば進めます([Issueでリクエスト](https://github.com/kjooncho/ux-motion-skills/issues))。

## インストール

**方法 1 — skills CLI:**

```bash
npx skills add kjooncho/ux-motion-skills
```

**方法 2 — Claude Code プラグインマーケットプレイス:**

```
/plugin marketplace add kjooncho/ux-motion-skills
```

**方法 3 — 手動コピー (単一スキル):**

```bash
git clone https://github.com/kjooncho/ux-motion-skills
cp -r ux-motion-skills/skills/motion-judgment ~/.claude/skills/
```

インストール後はタスクを普通に説明するだけです(「モーション作って」「イージングどれにする?」)— トリガー文脈で自動発動し、user-invocable のスキルは `/motion-guide` のように直接呼び出せます。

## 出典と方法論

これらのスキルは非公開の意思決定ログのコンパイル版です。ログには原則ごとの根拠・通過した検証ラウンド・初期案を修正/棄却させた反例が記録されており、コンパイルされた数値はスキル内に埋め込みました(元ログは非公開)。自分の環境で原則が破れたら、それを発見として扱ってください — まず反例を記録し、原則の*理由*がまだ成立するかを検証してからルールを直す。この検証ループがこのセットを信頼できるものにした方法であり、あなたのフォークにも同じく適用されます。

## 作者

**チョ・ギョンジュン (Kyoungjoon Cho / 조경준)** — UXモーションデザイナー — キャリア18年、直近11年はプロダクションモーション/インタラクションデザイン。GitHub [@kjooncho](https://github.com/kjooncho)。

## ライセンス

MIT — [LICENSE](./LICENSE) を参照。
