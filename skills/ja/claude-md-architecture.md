---
name: CLAUDE.md設計アーキテクチャ
description: CLAUDE.md をチームの AI 同僚オンボーディング文書として設計する。WHAT/WHY/HOW フレームワーク・5スコープカスケード・@imports モジュール化・Compound Engineering ループを使って、セッションをまたいで自己改善する知識ベースを構築する。
tags: [CLAUDE.md, コンテキスト, アーキテクチャ, ドキュメント, チーム]
difficulty: 中級
sources:
  - https://www.obviousworks.ch/en/designing-claude-md-right-the-2026-architecture-that-finally-makes-claude-code-work/
  - https://www.generative.inc/the-complete-claude-code-guide-2026-planning-context-engineering-and-high-leverage-development
---

> **一言でいうと:** `CLAUDE.md` を「AI同僚へのオンボーディング文書」として WHAT/WHY/HOW とスコープ設計で書くと、設定だけで回答品質が上がる。新規プロジェクト開始時や、Claudeが同じミスを繰り返す時に使う。

# 目的

CLAUDE.md は人間向けの README ではなく、AI チームメンバーへのオンボーディング文書。PR のたびに訂正した内容を CLAUDE.md に追記することで「永遠に繰り返さないバグリスト」が自動構築される（Compound Engineering）。

# 使用タイミング

- 新規プロジェクト開始時（最初の30分で設計する）
- CLAUDE.md が150行を超えてきた（リファクタリング時）
- Claude が同じミスを繰り返す（ルール追記）
- チームで統一した AI 行動規範が必要な時

# 使用しないタイミング

- 単発スクリプトや使い捨てプロジェクト
- 既に動いている CLAUDE.md を「なんとなく改善」したい場合（まず `/init` で診断する）

# 入力

- プロジェクト名・目的・技術スタック（バージョン含む）
- 守るべき規約・禁止事項
- ビルド・テスト・デプロイのコマンド
- 過去に Claude が犯したミスのリスト

# ワークフロー

## Step 1 — 5スコープカスケードを設計する

| スコープ | パス | 用途 | Git 管理 |
|---|---|---|---|
| グローバル | `~/.claude/CLAUDE.md` | 個人デフォルト（全プロジェクト） | なし |
| プロジェクトルート | `./CLAUDE.md` | プロジェクト共通ルール | ✅ |
| ローカル秘密 | `./CLAUDE.local.md` | 個人メモ・機密パス | ❌ .gitignore |
| フォルダ | `./src/CLAUDE.md` | モジュール固有ルール（遅延読み込み） | ✅ |
| サブエージェント | `./AGENTS.md` | マルチエージェント用（ツール横断対応） | ✅ |

**後勝ちルール：** より深いスコープが上位を上書きする。

## Step 2 — WHAT/WHY/HOW フレームワークで記述する

```markdown
# [プロジェクト名] CLAUDE.md

## WHAT（何を作っているか）
- プロジェクト：[一文]
- スタック：React 18.3 + TypeScript 5.4 + Vite 5 + Prisma 5.2
- 構造：src/components/, src/api/, src/utils/, tests/
- 重要ファイル：src/middleware/auth.ts（認証。変更前に必ず読む）

## WHY（なぜこう決めたか）
- camelCase for variables, PascalCase for React components
- MUST use TypeScript strict mode. MUST NOT use `any` type
- NEVER commit to main directly. Always create feature branch
- Conventional Commits: feat:, fix:, refactor:, docs:

## HOW（どうやって動かすか）
- Build: `npm run build`
- Test: `npm test` — after every code change
- Lint: `eslint . --fix` — before every commit
- PR: `gh pr create` when work is complete
```

## Step 3 — 精度ルールを適用する

**遵守率：** 具体的ルール 89% vs 曖昧なルール 35%

| ❌ 無視される（曖昧） | ✅ 遵守される（具体的） |
|---|---|
| "クリーンなコードを書く" | "camelCase で変数、PascalCase でコンポーネント" |
| "全てテストする" | "`npm test` を毎変更後に実行。utils/ は80%カバレッジ必須" |
| "TypeScriptを使う" | "strict mode 必須。`any` 型使用禁止" |
| "git に注意する" | "タスクごとに新ブランチ。main への直接コミット禁止" |

**強制語彙：** `MUST`・`NEVER`・`ALWAYS` を使うと遵守率が上がる。

## Step 4 — @imports でモジュール化する

```markdown
# CLAUDE.md（ルート）
@.claude/rules/git-conventions.md
@.claude/rules/security-rules.md
@docs/architecture.md
```

- ファイルサイズを分散してルートをスリムに保つ
- ルールごとにファイルを分けてプロジェクト横断で再利用
- `./src/CLAUDE.md` に API 固有ルールを置いて遅延読み込みにする

## Step 5 — Compound Engineering ループを設定する

```
PR レビューで Claude のミスを発見
 ↓
そのミスを具体的ルールとして CLAUDE.md に追記
 ↓
同じミスが二度と繰り返されない
 ↓
1ヶ月後：よくあるミスが消える
3ヶ月後：CLAUDE.md = チームの暗黙知の文書化
```

**ルール：** ミスを見つけたら感情的にならず、CLAUDE.md に1行追加する。

## Step 6 — サイズガイドラインを守る

| 指標 | 推奨値 | 超えたら |
|---|---|---|
| 行数 | ≤200行 | @imports で分割 |
| トークン | ≤2,000 | 優先度低いルールを削除 |
| セクション数 | 3〜5 | WHAT/WHY/HOW に絞る |

**Boris Cherny（Claude Code 作者）の CLAUDE.md：** 約2,500トークン（100行）

# 検証ステップ

```
/init を実行して Claude の CLAUDE.md 改善提案を確認する
→ 提案が的確なら構造が正しい
→ 「全て問題ない」と返ってきたら過剰に書きすぎ
```

# 成功基準

- Claude が同じミスを2回以上繰り返さない
- 新しいチームメンバーが CLAUDE.md を読んで迷わず作業できる
- CLAUDE.md が200行以下に収まっている
- `/init` 実行後に「改善提案なし」が返ってくる

# 失敗パターン

- **肥大化CLAUDE.md**：500行を超えると重要ルールが埋もれてスキップされる
- **曖昧な指示**：「良いコードを書く」は遵守率35%。具体的に書く
- **ルール重複**：CLAUDE.md に書いてあることをプロンプトにも書く
- **更新忘れ**：PR で修正したミスを CLAUDE.md に追記しない → 翌週同じミスが起きる
- **README と混同**：CLAUDE.md は人間ではなく AI へのオンボーディング文書

# ベストプラクティス

- まず `/init` を実行してベースラインを生成し、そこから削ぎ落とす
- `CLAUDE.local.md` に WIP メモや機密パスを書く（.gitignore 登録必須）
- フォルダレベルの `CLAUDE.md` でモジュール固有ルールをスコープ制限する
- 月次でレビューして古くなったルールを削除する（腐ったルールは従わない）
- `/dx:review-claudemd` で会話履歴から改善提案を自動抽出する

# アンチパターン

- 人間向け README の内容をそのままコピーする
- 「なんとなく良さそう」な規約を詰め込んで200行を超える
- バージョン番号を書かない（「React を使う」ではなく「React 18.3」と書く）
- グローバル CLAUDE.md にプロジェクト固有ルールを書く（スコープを使い分ける）

# プロンプト例

```
/init を実行して現在のプロジェクトの CLAUDE.md を生成してください。
生成後、WHAT/WHY/HOW フレームワークで再構成して200行以下に収めてください。
過去にエラーになったパターンがあれば教えてください（CLAUDE.md に追記します）。
```

# コマンド例

```bash
# CLAUDE.md の初回生成
/init

# CLAUDE.md の改善提案を取得
/dx:review-claudemd

# フォルダ固有ルールを追加（src/ 配下でのみ読まれる）
touch src/CLAUDE.md
```

# 関連スキル

- [[ワークスペース初期化]] — CLAUDE.md を含むワークスペース全体のセットアップ
- [[コンテキスト管理戦略]] — CLAUDE.md のトークンコスト管理
- [[フックシステム設計]] — CLAUDE.md で書けない「強制ルール」をフックで補完
