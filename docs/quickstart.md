# 30分セットアップガイド
## Claude Code Skills Starter

---

## 前提条件

- [ ] Claude Codeがインストール済み（`claude --version` で確認）
- [ ] プロジェクトディレクトリが存在する

Claude Codeが未インストールの場合：
```bash
npm install -g @anthropic-ai/claude-code
```

---

## Step 1：テンプレートをコピー（5分）

```bash
# プロジェクトのルートディレクトリで実行
mkdir -p .claude

# このリポジトリのテンプレートをコピー
cp /path/to/claude-code-skills-starter/templates/CLAUDE.md .claude/CLAUDE.md
```

---

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

---

## Step 3：動作確認（5分）

```bash
# Claude Codeを起動
claude

# 最初のコマンドで確認
> /overview
```

CLAUDE.mdの内容がClaudeに読み込まれていることを確認してください。

---

## Step 4：最初のタスクを試す（10分）

```
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

## 次のステップ

このテンプレートで効果を感じたら、同梱の4スキル（`skills/ja/` ・ `skills/en/`）も試してください。

- **CLAUDE.md設計**（`claude-md-architecture`）— ルールの作り方
- **計画ファースト開発 PIV**（`piv-development-loop`）— Plan → Implement → Verify
- **AIコミット戦略**（`ai-commit-strategy`）— コミットの型
- **アジャイルなプロンプト設計**（`agile-prompt-template`）— タスクの渡し方

すべて無料・CC BY 4.0。商用利用も改変も自由です。

---

問題があれば Issues でお気軽に。
