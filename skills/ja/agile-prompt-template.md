---
name: アジャイルチケット型プロンプトテンプレート
description: アジャイル開発のチケット構造（Context/To-dos/Not-to-dos/Acceptance Criteria）をAIプロンプトに適用するパターン。Role→Goal→Constraintsフレームワークと組み合わせ、「曖昧なプロンプトが曖昧な実装を生む」問題を解決する。チームで共有できる再利用可能プロンプトライブラリの設計方法も含む。
tags: [プロンプトエンジニアリング, テンプレート, アジャイル, 品質, 再利用]
difficulty: 初級
sources:
  - https://medium.com/google-cloud/taming-vibe-coding-the-engineers-guide-fff70b6d807a
  - https://www.augmentcode.com/guides/master-prompt-engineering-techniques-for-ai-coding
  - https://dev.to/raghavyuva/the-art-of-vibe-coding-with-actual-discipline-lo
---

# 目的

「プロンプトの品質 = 実装の品質の上限」という原則に基づき、曖昧なプロンプトを構造化された形式に変換する。一度書いた良いプロンプトをテンプレート化してチームで再利用する。

# 使用タイミング

- 新しい機能・コンポーネントの実装を依頼する時
- バグ修正・リファクタリングを依頼する時
- チームで AI 開発のプロンプト品質を統一したい時
- 同じ種類の作業を繰り返す時（テスト追加・ドキュメント生成等）

# 使用しないタイミング

- 探索的な質問・調査依頼（構造化不要）
- 1 行の変更や自明なタスク

# 入力

- タスクの目標
- 関連する制約・規約
- 成功基準

# ワークフロー

## フレームワーク1：アジャイルチケット型プロンプト

アジャイルのチケット形式をプロンプトに適用する：

```markdown
## Context（背景）
[なぜこのタスクが必要か・関連する制約・アーキテクチャの文脈]

## To-dos（実施すること）
- [具体的なタスク1]
- [具体的なタスク2]
- [具体的なタスク3]

## Not-to-dos（実施しないこと）
- [スコープ外の変更]
- [使ってはいけないアプローチ]
- [触れてはいけないファイル]

## Acceptance Criteria（受け入れ基準）
- [ ] [検証可能な条件1]
- [ ] [検証可能な条件2]
- [ ] [テストコマンドが通る]
```

**具体例（認証機能の実装）：**
```markdown
## Context
JWT リフレッシュトークンを実装する。src/auth/session.ts に既存の
セッション管理があり、そのパターンに従うこと。
RedisCache 経由でトークンを管理すること（直接 DB 書き込み禁止）。

## To-dos
- JWT リフレッシュトークンエンドポイントを POST /auth/refresh に追加
- トークンの有効期限を 15 分に設定（セキュリティ要件）
- リフレッシュトークンは 7 日間有効

## Not-to-dos
- src/auth/oauth.ts は変更しない（別の PR で対応予定）
- 独自の暗号化実装を書かない（jsonwebtoken ライブラリを使う）
- DB に直接書かない（必ず RedisCache 経由）

## Acceptance Criteria
- [ ] npm test -- --grep "jwt refresh" が通る
- [ ] 有効期限切れトークンが 401 を返す
- [ ] 正常なリフレッシュが新しいトークンペアを返す
- [ ] TypeScript 型エラーがゼロ
```

## フレームワーク2：Role→Goal→Constraints

AI に役割・目標・制約を明示する：

```markdown
# Role（役割）
あなたは [専門知識] を持つシニアエンジニアです。
[チームの規約] に精通しています。

# Goal（目標）
[具体的な成果物] を作成してください。

# Constraints（制約）
- 使用言語/フレームワーク：[指定]
- テスト要件：[指定]
- スタイルガイド：[指定]
- パフォーマンス要件：[指定]
```

**具体例（API エンドポイント実装）：**
```markdown
# Role
あなたは TypeScript と Node.js を専門とするシニアバックエンドエンジニアです。
このチームは Express.js + Prisma + Jest を使用しています。

# Goal
ユーザーのプロフィール更新 API エンドポイント（PUT /users/:id）を実装してください。

# Constraints
- Zod でリクエストボディを検証する
- 認証ミドルウェアを必ず通す（src/middleware/auth.ts を参照）
- 自分のプロフィールのみ更新可能（他ユーザーは 403）
- Jest テストをエンドポイントと同時に作成する
- 入力値は全て sanitize する（XSS 対策）
```

## フレームワーク3：フューショット（例示型）

同じパターンの変換に使う：

```markdown
# 指示
以下の例に従って [タスク] を実装してください：

# 例1
Input: [入力例1]
Output: [期待出力1]

# 例2
Input: [入力例2]
Output: [期待出力2]

# 実装対象
Input: [実際の入力]
```

## プロンプトライブラリの構築

頻繁に使うプロンプトをテンプレート化して保存：

```markdown
# .claude/commands/implement-feature.md（/implement-feature コマンド）
---
name: implement-feature
description: 新機能実装用テンプレート
---

以下のテンプレートに従って機能を実装してください：

## Context
$ARGUMENTS に記載の機能について：
- 関連ファイルを読んで既存パターンを把握する
- 変更が必要なファイルを特定する

## To-dos
$ARGUMENTS の To-dos セクションに記載の内容を実装する

## Not-to-dos
$ARGUMENTS の Not-to-dos セクションに記載の内容は変更しない

## Acceptance Criteria
$ARGUMENTS の Acceptance Criteria を全て満たすこと
実装後に [テストコマンド] を実行して確認する
```

```markdown
# .claude/commands/write-tests.md（/write-tests コマンド）
---
name: write-tests
description: テスト作成用標準テンプレート
---

@$ARGUMENTS を読んで、以下の基準でテストを書いてください：

対象：
- 正常ケース（最低 2 パターン）
- エラーケース（最低 3 パターン：入力不正・権限なし・存在しない ID）
- エッジケース（空文字・null・最大値等）

制約：
- モックを最小限にする（DB は実際に使う）
- テストは独立して実行できる（順序依存なし）
- describe/it のネストは 2 レベルまで
```

## 悪いプロンプト → 良いプロンプトの変換例

```
❌ 悪い例（曖昧）：
"決済機能を追加してください"

✅ 良い例（アジャイルチケット型）：
## Context
Stripe を使った月次サブスクリプション決済を実装する。
既存の src/billing/ を参照し同じパターンを使うこと。

## To-dos
- POST /api/subscribe エンドポイントを追加
- Stripe Checkout セッションを作成する
- Webhook で支払い完了を処理する（/api/webhooks/stripe）

## Not-to-dos
- 独自の決済ロジックを書かない（Stripe SDK のみ使用）
- 本番の Stripe キーはテストしない（テスト用キーのみ）
- 既存の src/billing/invoice.ts は変更しない

## Acceptance Criteria
- [ ] テストモードで決済フローが完了する
- [ ] Webhook 署名検証が実装されている（セキュリティ必須）
- [ ] 支払い失敗時に適切なエラーが返る
- [ ] npm test が通る
```

## ステップ実行の明示

タスクを段階的に分割してAIをコントロールする：

```markdown
以下の手順で実装してください：

Step 1：src/auth/ を読んで既存パターンを理解する
→ 理解したことを要約してから次へ

Step 2：実装計画を提示する（コードは書かない）
→ 計画を確認してから次へ

Step 3：Step 2 の計画に従って実装する

Step 4：テストを実行して結果を報告する
```

# 検証ステップ

```
プロンプト作成後のセルフチェック：
□ 「なぜ」（Context）が含まれているか
□ 「やること」（To-dos）が具体的か（動詞 + 対象）
□ 「やらないこと」（Not-to-dos）が明記されているか
□ 「完了基準」（Acceptance Criteria）が検証可能か
□ 使うライブラリ・パターンが指定されているか
□ このプロンプトだけで実装できるか（追加質問不要か）
```

# 成功基準

- AI が「何をすればよいか」を 1 回のプロンプトで理解できる
- 実装が Acceptance Criteria を全て満たす
- 範囲外の変更が発生しない（Not-to-dos が守られる）

# 失敗パターン

- **Acceptance Criteria が測定できない**：「良いコードを書く」→どう確認するか不明
- **Not-to-dos がない**：AI がスコープ外のファイルを「改善」してしまう
- **Context がない**：「なぜそうするか」がなく、AI が最短経路を選んで既存パターンと矛盾する
- **例が古い**：フューショットの例が現在のコードと違うパターンで実装される

# ベストプラクティス

- Acceptance Criteria は「[テストコマンド] が通る」という客観的な基準にする
- Not-to-dos に「触れてはいけないファイルパス」を具体的に書く
- 最初に作った良いプロンプトは `.claude/commands/` にテンプレートとして保存する
- Context には「なぜ」だけでなく「関連する既存実装のパス」も含める

# アンチパターン

- Acceptance Criteria が「動作する」「テストが通る」のみ（何のテスト？）
- Context を省略して「分かるだろう」と思う
- 同じプロンプトをコピー&ペーストし続ける（テンプレート化しない）

# プロンプト例

```markdown
## Context
[なぜこのタスクが必要か]
[参照すべき既存実装のパス]
[使用するライブラリ・パターン]

## To-dos
- [具体的な実装内容]
- [テスト作成]

## Not-to-dos
- [触れてはいけないファイル]
- [使ってはいけないアプローチ]

## Acceptance Criteria
- [ ] [テストコマンド] が通る
- [ ] [型チェックコマンド] が通る
- [ ] [具体的な動作確認]
```

# コマンド例

```bash
# プロンプトテンプレートを /implement-feature コマンドとして登録
cat > .claude/commands/implement-feature.md << 'EOF'
---
name: implement-feature
description: Context/To-dos/Not-to-dos/Acceptance Criteria 形式で機能実装
---
以下のテンプレートに従って実装してください：$ARGUMENTS
EOF

# 使い方
/implement-feature "
## Context: JWT認証の追加
## To-dos: ログインエンドポイントの実装
## Not-to-dos: 既存セッション管理を変更しない
## Acceptance Criteria: npm test が通る
"
```

# 関連スキル

- [[CLAUDE.md設計アーキテクチャ]] — CLAUDE.md でのグローバルルール設定
- [[CLAUDE.mdカリキュレーションプロトコル]] — プロンプトルールの定期見直し
- [[ワークスペース初期化]] — コマンドファイルの初期設定
- [[バイブコーディング脱出プロトコル]] — プロンプト品質と実装品質の関係
