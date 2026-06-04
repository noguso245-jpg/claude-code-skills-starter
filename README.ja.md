# Claude Code スキル集 — スターターパック 🛠️

> Claude Code を「速い・安全・確実」にするための実戦スキル集。YouTube/GitHub/X/技術ブログ横断調査から抽出。

**このリポで手に入るもの — すべて無料・CC BY 4.0:**
- ✅ **完結した開発1周分の4スキル**（計画→実装→コミット→統治）
- ✅ 穴埋め式の **`CLAUDE.md` テンプレート** ＋ サンプル **pre-commit hook**
- ✅ 今日中に1スキルを動かせる **5分クイックスタート**

*役に立ったら ⭐ で応援してください（他の人が見つけやすくなります）。*

🇬🇧 English: [README.md](./README.md)

---

## なぜ作ったか

多くの人は Claude Code を「速い補完」として使い、生産性が変わらないと悩んでいます。
差を生むのはモデルではなく **ワークフロー**=どう計画し、どうプロンプトを構造化し、どうコミットし、どう `CLAUDE.md` を設計するか。

このリポジトリは、プロのClaude Code開発「1周分」を構成する **無料スキル4本** を提供します。
あなたのプロジェクトにコピーして、手順通りに使うだけ。今日から違いを実感できます。機能制限デモではなく、単体で実用できます。

---

## 無料スキル4本（1つの完結ループ）

| スキル | 難易度 | 得られるもの |
|---|---|---|
| [PIV開発ループ](./skills/ja/piv-development-loop.md) | 初級 | すべてのタスクを Plan→Implement→Verify に分割し承認ゲートを挟む。最高レバレッジのパターン |
| [CLAUDE.md設計アーキテクチャ](./skills/ja/claude-md-architecture.md) | 中級 | WHAT/WHY/HOW + スコープカスケード。設定だけで回答品質が変わる |
| [AI開発コミット戦略](./skills/ja/ai-commit-strategy.md) | 初級 | 1タスク=1コミット（セーブポイント）。いつでも安全に巻き戻せる |
| [アジャイルチケット型プロンプト](./skills/ja/agile-prompt-template.md) | 初級 | Context/To-dos/Not-to-dos/Acceptance。曖昧な出力を止める |

> 🇬🇧 英語版は [`skills/en/`](./skills/en/) にあります。

組み合わせ: プロンプトを構造化(4)→計画・実装・検証(1)→こまめにコミット(3)→CLAUDE.mdで統治(2)。

---

## クイックスタート — 5分で1スキルを体験する

```bash
# 1. リポをクローン
git clone https://github.com/noguso245-jpg/claude-code-skills-starter

# 2. スキルを1本だけプロジェクトに入れる（最初はPIVがおすすめ）
mkdir -p your-project/.claude/skills
cp claude-code-skills-starter/skills/ja/piv-development-loop.md your-project/.claude/skills/

# 3. プロジェクトで Claude Code を起動
cd your-project
claude
```

そして Claude Code にこう貼り付けます：

```
.claude/skills/piv-development-loop.md を読んで、次のタスクに適用して。
まず「計画(PLAN)だけ」を出して。承認するまでコードは書かないで。タスク: <ここに依頼>
```

コードを書く前に「レビュー可能な計画」が返ってきます。この承認ゲートこそが肝です。
「補完」と「本物のワークフロー」の差はここにあります。詳しい手順: [`docs/quickstart.md`](./docs/quickstart.md)。

---

## その他の同梱物

4本のスキルに加えて、すぐ使えるスターターも同梱しています。

| ファイル | 中身 |
|---|---|
| [`templates/CLAUDE.md`](./templates/CLAUDE.md) | Webアプリ用の穴埋め式 `CLAUDE.md` — 役割・コーディング規約・ワークフロー・「やってはいけないこと」入り。 |
| [`hooks/pre-commit.md`](./hooks/pre-commit.md) | コミット前lint・`.env`書き込みブロック等のHooksサンプル（`.claude/settings.json` に貼るだけ）。 |
| [`docs/quickstart.md`](./docs/quickstart.md) | テンプレートとスキルをつなぐセットアップガイド。 |

すべて無料・CC BY 4.0。コピー・改変自由です。

---

## ⭐ Star ／ 👁 Watch

この4本は、いま作っている大きめのセットから厳選した入口です。新しい無料スキルを順次追加しています。

- **⭐ Star** — 役に立ったら応援を。他の人が見つけやすくなります。
- **👁 Watch** — 追加時に通知を受け取りたい場合はこちら（通知は GitHub の Watch 機能で届きます。Star だけでは通知は届きません）。

「こんなスキルが欲しい」があれば [Issue](../../issues/new) でお気軽に。

---

*Built solo + AI.*
