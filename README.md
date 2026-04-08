# user-tester

Claude Code のスキル・エージェントを使って、AIペルソナによるユーザーテストを実施するツールキット。

PRD/MRDがあれば精度が上がるが、なくてもヒアリングベースで実行できる。

## スキル

| スキル | 説明 | 用途 |
|---|---|---|
| `/user-survey` | 大量ペルソナによるアンケート調査 | 定量データ収集（100人規模のリッカート尺度・選択式・自由記述） |
| `/user-test` | ペルソナ視点でChromeを実操作 | ユーザビリティ評価（画面遷移・操作感・つまずきポイント） |
| `/user-interview` | 1人のペルソナと対話形式でインタビュー | 定性データ収集（課題の深掘り・動機の理解） |

### 推奨フロー

```
/user-survey → /user-interview → /user-test
```

1. `/user-survey` で定量的に全体傾向を把握する
2. `/user-interview` で注目テーマを深掘りする
3. `/user-test` で実画面の操作性を検証する

## エージェント

| エージェント | モデル | 説明 |
|---|---|---|
| `survey-respondent` | haiku | アンケート回答を大量生成する（`/user-survey` から呼び出し） |
| `interviewee` | sonnet | ペルソナとして対話に応じる（`/user-interview` から呼び出し） |

## 入力

以下のドキュメントがあれば精度が上がる（いずれも任意）:

- **PRD** - ターゲットユーザー、ペイン、提供価値
- **MRD** - 市場背景、競合、ターゲットセグメント
- **ユーザーストーリー** - シナリオ（Given/When/Then）
- **UI仕様** - 画面定義・インタラクション

ドキュメントがない場合は、スキル実行時にヒアリングで情報を収集する。

## 前提条件

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) がインストール済みであること
- `/user-test` を使う場合は Chrome 連携が有効であること（`--chrome`）

## セットアップ

このリポジトリを Claude Code のプロジェクトディレクトリに配置するか、`skills/` と `agents/` を既存プロジェクトにコピーする。

```bash
# リポジトリをクローン
git clone https://github.com/taketetsu1982/user-tester.git

# 既存プロジェクトに組み込む場合
cp -r user-tester/skills/ your-project/.claude/skills/
cp -r user-tester/agents/ your-project/.claude/agents/
```

## ライセンス

[MIT License](LICENSE)
