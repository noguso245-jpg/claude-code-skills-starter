# PUBLISH GUIDE — 公開手順書（オーナー用）

このリポジトリ（`free-repo-build/`）はそのまま公開できる無料スキル集です。
公開は**オーナーのGitHub / Redditアカウントで手動**で行ってください（自動化はBANリスク）。

---

## 0. プレースホルダ置換（JOBSが実施済み ✅）

公開で最も事故りやすい「プレースホルダ未置換」は **JOBSが既に処理済み**です。あなたの手作業は不要です。

| 項目 | 状態 |
|---|---|
| GitHubユーザー名 `noguso245-jpg` | ✅ README・本ガイド・LICENSEの全URLに反映済み |
| LICENSE著作権表示名 | ✅ `noguso245-jpg` を設定済み（変えたい場合はLICENSEを編集） |
| Xハンドル | ⚠️ 未確認のため**公開物から削除**（元の `k___n___t_1125` は実在未確認だったため）。正しいハンドルが分かれば、README末尾に1行追加するだけ |

> 念のため、`git push` 前に「リポ名を `claude-code-skills-starter` 以外にした場合」は全URLの修正が必要です。リポ名を変えないならこのままでOK。

---

## 1. 公開前チェックリスト

### コンテンツ・線引き
- [ ] 無料公開は4本のみ（上級・MCP・マルチエージェントを含んでいない）
- [ ] 無料4本の本文に有料スキルへの内部リンク・依存記述が残っていない

### リンク有効性（実クリック確認）
- [ ] Gumroad EN Starter https://streamsolty.gumroad.com/l/wrtgun が開く
- [ ] Gumroad EN Workflow OS https://streamsolty.gumroad.com/l/guuhox が開く
- [ ] Gumroad JA Starter https://streamsolty.gumroad.com/l/gliwz が開く
- [ ] Gumroad JA Workflow OS https://streamsolty.gumroad.com/l/vhcysn が開く
- [ ] BOOTH https://streamsolty.booth.pm/ が開く
- [ ] README内の相対リンク（skills/en/・skills/ja/・README.ja.md）が実ファイルと一致
- [ ] プレースホルダ（§0）を全て正式値に一括置換した

### 価格整合
- [ ] README記載の価格が現行と一致（Starter ¥1,980/$14・Workflow OS ¥9,800/$65）
- [ ] product2値上げ（¥12,800/$85）は6/9に淡々と反映（"今だけ"訴求はしない方針）

### ライセンス
- [ ] LICENSE の著作権表示名を入れた
- [ ] （任意）GitHubのライセンス自動検出が欲しい場合、LICENSE本文を CC BY 4.0 の正式legalcodeに差し替えた

### 規約順守
- [ ] awesome-claude-code の最新 CONTRIBUTING / Issueテンプレートを送信直前に確認した
- [ ] Issue/PR・Reddit本文に価格・購入リンクを含めていない（無料リポのリンク1本のみ）
- [ ] Reddit投稿は人間が手動で行う／投稿直後の自演upvote・サクラコメントをしない
- [ ] r/ClaudeCode の self-promotion ルール・フレアー要件を当日確認した

---

## 2. GitHub 公開コマンド

```bash
# free-repo-build/ の中身をリポジトリのルートにする
cd free-repo-build
git init
git add .
git commit -m "Add free Claude Code skills starter pack (4 skills, EN/JA)"
git branch -M main
git remote add origin https://github.com/noguso245-jpg/claude-code-skills-starter.git
git push -u origin main
```
※ 事前にGitHubで空のリポジトリ `claude-code-skills-starter` を作成しておくこと。

---

## 3. awesome-claude-code への申請

> 送信直前に hesreallyhim/awesome-claude-code の最新 CONTRIBUTING を必ず確認（Issue方式かPR方式か・テンプレ有無）。
> トーンは「価値あるリソースの追加提案」=セールス臭ゼロ。価格・購入リンクは書かない。

### Issueタイトル
```
[Resource Submission] Claude Code Skills Starter — 4 free, self-contained workflow skills
```

### Issue本文
```markdown
### What is it?

A small, focused collection of **free, self-contained Claude Code skills** (markdown playbooks) that together cover one complete loop of reliable Claude Code development:

- **PIV Development Loop** — Plan → Implement → Verify with a human approval gate
- **CLAUDE.md Architecture** — WHAT/WHY/HOW framework + scope cascade
- **AI Commit Strategy** — 1 task = 1 commit, Conventional Commits, easy rollback
- **Agile Prompt Template** — Context / To-dos / Not-to-dos / Acceptance Criteria

Each skill is a standalone markdown file you can drop into `.claude/skills/` or read as a reference.

### Link

https://github.com/noguso245-jpg/claude-code-skills-starter

### Why it might belong in this list

- Beginner/intermediate friendly — a clean on-ramp for people who use Claude Code but haven't adopted a real workflow yet.
- Self-contained: no install, no external service, just markdown.
- Fills the "how do I actually structure my work?" gap that complements the more advanced hooks/MCP resources already in the list.

### Suggested category

`Skills` (or `Workflows / Knowledge` — happy to follow whatever fits the list's structure).

### License

CC BY 4.0.

### Author

Solo maintainer, building and sharing Claude Code workflows. Glad to adjust the format, category, or description to match the contribution guidelines — just let me know.
```

> PR方式が必須の場合: フォーク → リストの既存フォーマットを厳密に踏襲して1行追記 → 上記要点をPR descriptionに転記。1リソース1PR、説明は簡潔に。

---

## 4. Reddit r/ClaudeCode 投稿草案

> ⚠️ 投稿は必ず人間が手動で。価格・購入リンクは本文に書かない（無料リポのリンク1本のみ）。
> 投稿前に r/ClaudeCode の self-promotion / sharing ルールを当日確認。投稿直後の自演upvoteはしない。

### タイトル案（推奨は1または2＝一人称体験談トーン）
```
1. I compiled 4 free Claude Code "skills" that fixed my plan→code→verify workflow (markdown, no setup)
2. After months of using Claude Code like autocomplete, these 4 workflow patterns finally made it click — sharing them free
3. Free starter pack: 4 self-contained Claude Code skills (PIV loop, CLAUDE.md design, commit strategy, prompt template)
```

### 本文
```markdown
Like a lot of people here, I spent my first few months using Claude Code basically as a faster autocomplete — and kept wondering why my output didn't actually get more reliable.

What changed it for me wasn't the model. It was adopting an actual workflow. So I wrote up the 4 patterns that made the biggest difference as standalone markdown "skills" you can drop into `.claude/skills/` (or just read):

- **PIV loop** — splitting every task into Plan → Implement → Verify with an approval gate before code gets written. This alone killed most of my "confidently wrong" implementations.
- **CLAUDE.md architecture** — a WHAT/WHY/HOW structure + scope cascade. Changing only the config noticeably improved answer quality.
- **Commit strategy** — 1 task = 1 commit as save points, so I can always roll back a bad AI session.
- **Prompt template** — Context / To-dos / Not-to-dos / Acceptance Criteria. Giving Claude a "ticket" instead of a vague ask.

They're free and self-contained (no install, no service). Repo here: https://github.com/noguso245-jpg/claude-code-skills-starter

Curious what workflows others here have settled on — especially how you all handle the verify step. Always looking to improve these.
```

> r/ClaudeAI（891k）へは、同一文面の連投を避け角度を変える（例: CLAUDE.md設計に寄せた深掘り版）か、日を空けて投稿。

---

## 5. 公開後の計測（KPI回収）

戦略 `成果物/ビジネス戦略部/zero-to-one-gtm-2026-06-01.md` の §5 に準拠:
- GitHub stars / リポ流入（traffic insights）
- awesome-list Issue/PR の採否
- Reddit投稿の閲覧・upvote・コメント
- 無料リポ → Gumroad/BOOTH への遷移（紹介リンクのクリック）

公開後しばらくしたら JOBS に数値を渡せば、次の一手（横展開・Week2のCV投稿）を設計します。
