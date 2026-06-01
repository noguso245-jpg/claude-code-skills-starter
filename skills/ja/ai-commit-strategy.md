---
name: AI開発コミット戦略
description: AIコーディングセッションに最適化したgitコミット戦略。「1タスク=1コミット（ゲームのセーブポイント）」・Conventional Commits形式・git diff注入・危険操作前チェックポイントを組み合わせ、AIセッションの安全な進捗管理と素早い巻き戻しを実現する。
tags: [git, コミット, バージョン管理, セーフポイント, Conventional Commits]
difficulty: 初級
sources:
  - https://addyosmani.com/blog/ai-coding-workflow/
  - https://github.com/awattar/claude-code-best-practices
  - https://addyosmani.com/blog/self-improving-agents/
---

# 目的

AI コーディングセッションでは変更が高速かつ広範囲に及ぶため、人間開発よりもコミット粒度が重要。「後で整理しようとまとめてコミット」は AI セッションでは致命的。細かいセーブポイントが失敗からの即座の回復を可能にする。

# 使用タイミング

- AI コーディングセッション全般（常時適用）
- レガシーコードのリファクタリング
- 大規模タスクを小分けにして進める時
- 実験的な変更を試す前

# 使用しないタイミング

- 1 行の typo 修正（コミットの粒度として細かすぎる）
- WIP（作業中途中）コミット（ただし checkpoint コミットは OK）

# 入力

- 完了したタスクの内容
- 変更されたファイルのリスト
- タスクの種別（feat/fix/refactor/chore/test/docs）

# ワークフロー

## 基本原則：ゲームのセーブポイント思想

```
人間開発：1日の作業 → まとめてコミット
AIコーディング：1タスク完了 → 即コミット → 次のタスク

理由：
- AI は高速に広範囲を変更する
- 問題が発覚した時のコミット単位が小さいほど巻き戻しコストが低い
- セーブポイントがあれば実験的な変更を恐れずに試せる
```

## パターン1：タスク完了即コミット

```bash
# タスク完了ごとに即座にコミット（間隔は30分以内を目安）
git add src/auth/jwt.ts tests/auth/jwt.test.ts
git commit -m "feat(auth): JWT リフレッシュトークンのローテーション実装"

# 次のタスクへ（セーブポイント確立済み）
```

**CLAUDE.md に追記すべきルール：**
```markdown
## コミット規律
- 各タスク完了後に即座にコミットする
- 「後でまとめて」は禁止
- コミット前に npm test を実行する
```

## パターン2：Conventional Commits 形式

AI へのコミットメッセージ生成を一貫させる：

```
形式：<type>(<scope>): <説明>

type:
  feat     - 新機能
  fix      - バグ修正
  refactor - リファクタリング
  test     - テスト追加・変更
  docs     - ドキュメント更新
  chore    - 設定・ビルド・依存関係
  perf     - パフォーマンス改善

例：
feat(auth): JWT リフレッシュトークンのローテーション実装
fix(payments): Stripe Webhook 重複処理バグを修正
refactor(user): UserService を依存性注入パターンに変更
test(api): /users エンドポイントの統合テストを追加
```

**AI へのプロンプト：**
```
変更内容を分析してConventional Commits形式のコミットメッセージを提案してください。
format: <type>(<scope>): <日本語説明>（50文字以内）
```

## パターン3：チェックポイントコミット（危険操作前）

```bash
# リファクタリング・依存関係変更・マイグレーション前の必須手順
git add -A
git commit -m "chore: [checkpoint] リファクタリング前の安定状態"

# 変更実行
# 失敗したら即座に巻き戻せる
git reset --hard HEAD~1
```

**AI への指示：**
```
このタスクを実行する前に：
1. 現在のファイルを git add -A && git commit -m "[checkpoint]..." でコミットしてください
2. その後タスクを実行してください
3. 問題が発生した場合は git reset --hard HEAD~1 で戻ってください
```

## パターン4：git diff 注入（AI のコンテキスト補強）

```bash
# AI セッション開始時に変更履歴を提供する
git diff HEAD~5..HEAD  # 直近 5 コミットの差分
git log --oneline -10  # 直近 10 コミットのログ

# AI へ：
"git diff の出力を確認して、現在の変更方向性を把握してください。
同じパターンで実装を続けてください。"
```

## パターン5：レガシーリファクタリング専用フロー

```bash
# Step 1：フィーチャーブランチ作成
git checkout -b refactor/legacy-auth

# Step 2：分析コミット（コードは変更しない）
claude -p "src/auth/ の構造を分析して analysis.md を生成してください。
コードは変更しないこと。分析ドキュメントのみ作成。"
git add .claude/analysis.md && git commit -m "docs: auth モジュール分析"

# Step 3：1モジュールずつ抽出（1懸念点 = 1コミット）
# 「一度に全部リファクタリング」は禁止
claude -p "analysis.md に基づいて、JwtService のみを抽出してください。
他のモジュールに触れないこと。"
git add src/auth/jwt.service.ts && git commit -m "refactor(auth): JwtService を分離"

# Step 4：次のモジュール
# ...繰り返す
```

## AI へのスマートコミットコマンド

```bash
# .claude/commands/smart-commit.md として登録
# /smart-commit で実行できる
```

```markdown
---
name: スマートコミット
description: git diff を分析してConventional Commits形式のコミットを作成
---

以下の手順でコミットを作成してください：

1. `git diff` を実行して変更内容を確認する
2. `git status` で変更ファイルを確認する
3. 変更内容に基づいて Conventional Commits 形式のメッセージを決める
4. テストファイルと実装ファイルを分けて、適切なスコープを特定する
5. `git add [変更したファイル]` で関連ファイルのみをステージングする
6. コミットを実行する

ルール：
- `git add -A` や `git add .` は使わない（関係ないファイルを含める恐れ）
- .env ファイルは絶対にコミットしない
- コミットメッセージは日本語 50 文字以内
```

# 検証ステップ

```
コミット前に確認：
□ npm test が通過しているか
□ コミットに関係ないファイルが含まれていないか（git status 確認）
□ .env, secrets/ がステージングされていないか
□ Conventional Commits 形式になっているか
□ チェックポイントコミットが必要な操作でないか
```

# 成功基準

- git log が読み取り可能な進行状況の記録になっている
- 任意のコミットに 5 秒以内に巻き戻せる
- コミット単位が「1 つの変更理由」に対応している

# 失敗パターン

- **「後でまとめて」コミット**：問題発覚時の巻き戻し単位が大きすぎる
- **`git add -A` の乱用**：テスト生成ファイルや一時ファイルが混入する
- **チェックポイントなしの危険操作**：大規模リファクタリングで方向を失う
- **コミットメッセージ「WIP」「fix」だけ**：git log が情報のない記録になる
- **1 セッション 1 コミット**：AI の高速変更に対して粒度が粗すぎる

# ベストプラクティス

- タスク完了 = コミット（時間ではなくタスク単位でコミット）
- git log は「このプロジェクトで何が起きたか」の物語。AI が読んでも分かる粒度にする
- `git diff HEAD~5` を AI セッション開始時に提供することで、AI が過去の変更方向を把握できる
- 「スマートコミット」コマンドを作成して /smart-commit で呼び出せるようにする

# アンチパターン

- AI に「全部変更してから最後にまとめてコミット」を指示する
- git の設定（.gitconfig）を AI に変更させる
- マージコミットを単体コミットとして扱う

# プロンプト例

```
タスクを完了したら、以下の手順でコミットしてください：
1. npm test を実行してテストが通過することを確認する
2. 変更内容を分析して Conventional Commits 形式のメッセージを決める
3. 関連ファイルのみを git add する（.env や node_modules は除外）
4. コミットを実行する

コミットメッセージ形式：
<type>(<scope>): <日本語説明（50文字以内）>
```

# コマンド例

```bash
# 変更の確認
git status
git diff --stat

# スコープ付きコミット
git add src/auth/ tests/auth/
git commit -m "feat(auth): JWT 有効期限を 1h から 15m に短縮"

# チェックポイントコミット
git add -A && git commit -m "chore: [checkpoint] DB マイグレーション前の安定状態"

# 巻き戻し（チェックポイントへ）
git reset --hard HEAD~1

# AI へのコンテキスト提供
git log --oneline -10
git diff HEAD~3..HEAD
```

# 関連スキル

- [[PIV開発ループ]] — 各フェーズ完了でコミットするタイミング
- [[セッションスコープ規律]] — セッション終了前のコミット確認
- [[Gitウォークツリー並行開発]] — ウォークツリーでのコミット管理
- [[複雑度ベース実装ルーティング]] — Large タスクでのフェーズごとコミット
