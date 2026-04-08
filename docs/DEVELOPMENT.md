# 開発ガイド

## ディレクトリ構成

```
user-tester/
├── .claude-plugin/
│   └── plugin.json          # プラグインマニフェスト
├── agents/
│   ├── interviewee.md       # インタビュー応答エージェント（sonnet）
│   └── survey-respondent.md # アンケート回答エージェント（haiku）
├── skills/
│   ├── user-interview/
│   │   └── SKILL.md         # インタビュースキル
│   ├── user-survey/
│   │   └── SKILL.md         # アンケートスキル
│   └── user-test/
│       └── SKILL.md         # ユーザビリティテストスキル
├── docs/
│   ├── USER_GUIDE.md        # 利用者ガイド
│   └── DEVELOPMENT.md       # このファイル
├── LICENSE
└── README.md
```

## 開発環境構築

```bash
git clone https://github.com/taketetsu1982/user-tester.git
cd user-tester
```

ローカルでスキルの動作確認をする場合:

```bash
claude  # リポジトリ内で起動すれば skills/ と agents/ が認識される
```

## スキルの編集

各スキルは `skills/{スキル名}/SKILL.md` に定義されている。

### SKILL.md の構造

```markdown
---
name: スキル名
description: スキルの説明
---

# /スキル名

概要説明

## Input
入力ドキュメントの定義

## Output
出力の定義

## Execution Steps
### Step 1: ...
### Step 2: ...

## 注意
制約事項
```

### 編集時の注意

- SKILL.md は200行以内に収める
- Step 1 は「コンテキストを収集する」で統一されている（PRD/MRD任意、なければヒアリング）
- ペルソナ生成はターゲットユーザー像に基づく（PRD固有の表現を使わない）

## エージェントの編集

各エージェントは `agents/{エージェント名}.md` に定義されている。

### エージェント定義の構造

```markdown
---
name: エージェント名
description: エージェントの説明
model: haiku / sonnet
---

# エージェント名

役割の説明

## あなたの役割
ルールと振る舞い

## Input
prompt から受け取る情報

## Output
出力形式
```

### 編集時の注意

- エージェント定義は100行以内に収める
- model はタスクの性質に応じて選択する:
  - `haiku`: 定型的な大量生成（アンケート回答など）
  - `sonnet`: 対話・推論が必要なタスク（インタビューなど）

## リリース手順

1. `plugin.json` の `version` を更新する
2. PR を作成してレビュー・merge する
3. GitHub Release を作成する

```bash
# バージョン更新
# .claude-plugin/plugin.json の "version" を更新

# リリース作成
gh release create v{バージョン} --title "v{バージョン}" --notes "リリースノート"
```

### バージョニング

セマンティックバージョニング（`MAJOR.MINOR.PATCH`）に従う。

| 桁 | いつ上げるか |
|---|---|
| MAJOR | 後方互換性が壊れる変更（スキル名変更など） |
| MINOR | 後方互換のある機能追加（新スキル追加など） |
| PATCH | バグ修正・文言修正 |
