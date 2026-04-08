# user-tester

Claude Code のスキル・エージェントを使って、AIペルソナによるユーザーテストを実施するツールキット。

[![GitHub Release](https://img.shields.io/github/v/release/taketetsu1982/user-tester)](https://github.com/taketetsu1982/user-tester/releases)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

PRD/MRDがあれば精度が上がるが、なくてもヒアリングベースで実行できる。

## ドキュメント

### 利用者向け

プラグインを使いたい方はこちら：

- **[利用者ガイド](./docs/USER_GUIDE.md)** - インストール方法、使い方、トラブルシューティング

### 開発者向け

プラグインを開発・改善したい方はこちら：

- **[開発ガイド](./docs/DEVELOPMENT.md)** - 開発環境構築、テスト、リリース手順

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

プロジェクトの状況に応じて順序は入れ替えてよい。単体でも使える。

## エージェント

| エージェント | モデル | 説明 |
|---|---|---|
| `survey-respondent` | haiku | アンケート回答を大量生成する（`/user-survey` から呼び出し） |
| `interviewee` | sonnet | ペルソナとして対話に応じる（`/user-interview` から呼び出し） |

## ライセンス

[MIT License](LICENSE)
