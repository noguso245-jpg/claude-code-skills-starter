# クイックスタート
## Claude Code Skills Starter

> まず **5分で1スキルを体験** → 効果を感じたら CLAUDE.md テンプレ（後半）まで進む、の2段構成です。

---

## 前提条件

- [ ] Claude Codeがインストール済み（`claude --version` で確認）
- [ ] 適用したいプロジェクトディレクトリがある

Claude Codeが未インストールの場合：
```bash
npm install -g @anthropic-ai/claude-code
```

---

# Part 1：5分で1スキルを体験する（PIV開発ループ）

最初の体験には **PIV開発ループ** が最適です。「コードを書く前に計画を出させ、承認してから実装する」という、最も効果を感じやすいスキルです。

## Step 1：スキルを1本だけコピー（1分）

```bash
# リポをクローン（未取得の場合）
git clone https://github.com/noguso245-jpg/claude-code-skills-starter

# 自分のプロジェクトに skills フォルダを作ってコピー
mkdir -p your-project/.claude/skills
cp claude-code-skills-starter/skills/ja/piv-development-loop.md your-project/.claude/skills/
```

## Step 2：Claude Code を起動（1分）

```bash
cd your-project
claude
```

## Step 3：スキルを適用して「計画だけ」を出させる（3分）

Claude Code に次をそのまま貼り付けます：

```
.claude/skills/piv-development-loop.md を読んで、次のタスクに適用して。
まず「計画(PLAN)だけ」を出して。承認するまでコードは書かないで。
タスク: <あなたの小さな実装タスクを1つ>
```

**何が起きるか：** いきなりコードを書き始めず、変更対象ファイル・手順・検証方法を含む「レビュー可能な計画」が返ってきます。
計画を読んで「OK、実装して」と返すと、承認した範囲だけが実装されます。

> これが体験のゴールです。承認ゲートが入るだけで、的外れな実装・暴走を未然に防げます。
> 英語版を使う場合は `skills/en/piv-development-loop.md` を同じ手順でコピーしてください。

体験できたら、残り3スキル（`ai-commit-strategy` / `agile-prompt-template` / `claude-md-architecture`）も同じ要領で試せます。

---

# Part 2：CLAUDE.md テンプレートでプロジェクト全体を底上げ（任意・約20分）

スキル単体の効果を感じたら、プロジェクト全体に効く `CLAUDE.md` を設定します。

## Step 1：テンプレートをコピー（5分）

```bash
# プロジェクトのルートディレクトリで実行
mkdir -p .claude
cp /path/to/claude-code-skills-starter/templates/CLAUDE.md .claude/CLAUDE.md
```

## Step 2：プロジェクト情報を記入（10分）

`.claude/CLAUDE.md` を開いて、以下を埋めてください：

```markdown
## Project Overview

- **Type**: Web Application
- **Stack**: Next.js + Node.js + PostgreSQL  ← あなたのスタックに変更
- **Stage**: MVP
- **Team Size**: Solo
```

**ポイント：**
- スタックを正確に書くと、Claude Codeが正しいコードを生成する
- 「やってはいけないこと」セクションは特に重要。カスタマイズ推奨

## Step 3：動作確認（5分）

```bash
claude
> このプロジェクトの構造を理解して、最初に何をすべきか教えて
```

設定前と比較して、回答の精度が上がっているはずです。

---

## よくある質問

**Q: CLAUDE.mdはどこに置くべきか？**  
A: プロジェクトルートの `.claude/` フォルダ内。またはプロジェクトルート直下。

**Q: 内容はどれくらい書くべきか？**  
A: 50〜150行が最適。200行を超えると効果が落ちる。

**Q: チームで使う場合は？**  
A: `.claude/CLAUDE.md` をGitにコミットしてチームで共有。個人設定は `.claude/CLAUDE.local.md` に分離。

**Q: モデルが変わったら更新が必要？**  
A: ワークフローの型は変わらない。モデル名などの設定値だけ更新が必要。

---

## 4スキルの早見表

| スキル | ファイル | ひとことで |
|---|---|---|
| PIV開発ループ | `skills/ja/piv-development-loop.md` | Plan → Implement → Verify。計画を承認してから実装 |
| CLAUDE.md設計 | `skills/ja/claude-md-architecture.md` | ルール（設定）の作り方で回答品質を上げる |
| AIコミット戦略 | `skills/ja/ai-commit-strategy.md` | 1タスク=1コミット。安全に巻き戻せる |
| アジャイルプロンプト | `skills/ja/agile-prompt-template.md` | タスクをチケット形式で渡し曖昧出力を防ぐ |

すべて無料・CC BY 4.0。商用利用も改変も自由です。

---

役に立ったら ⭐ で応援してください。問題があれば Issues でお気軽に。
