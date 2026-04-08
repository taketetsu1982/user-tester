# 利用者ガイド

## 前提条件

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) がインストール済みであること
- `/user-test` を使う場合は以下が追加で必要:
  - [Claude in Chrome](https://chromewebstore.google.com/detail/claude-in-chrome/) 拡張機能がインストール済みであること
  - Claude Code を Chrome 連携モードで起動すること（`claude --chrome`）

## インストール

### A. このリポジトリ単体で使う

```bash
git clone https://github.com/taketetsu1982/user-tester.git
cd user-tester
claude  # そのまま /user-survey 等が使える
```

### B. 既存プロジェクトに組み込む

```bash
cp -r user-tester/skills/ your-project/.claude/skills/
cp -r user-tester/agents/ your-project/.claude/agents/
```

### C. プラグインとしてインストール

```bash
claude --plugin-dir ./user-tester
```

## 使い方

### /user-survey — アンケート調査

大量のAIペルソナにアンケートを実施し、定量データを収集する。

```bash
/user-survey
```

**実行の流れ:**

1. PRD/MRDがあれば読み込む。なければプロダクトについてヒアリングされる
2. 調査テーマを聞かれるので答える
3. 設問セット（5〜10問）が提示される → 承認または修正指示
4. 回答者数と属性分布が提示される → 承認または修正指示
5. AIペルソナが並列で回答を生成する
6. 統計サマリが提示される

### /user-interview — インタビュー

1人のAIペルソナと対話形式でインタビューを行う。

```bash
/user-interview
```

**実行の流れ:**

1. PRD/MRDがあれば読み込む。なければヒアリング
2. ペルソナのロール・属性を選択する
3. ペルソナとの対話セッションが開始される
4. 自由に質問する。「インタビュー終了」で終了
5. インタビューサマリが提示される

### /user-test — ユーザビリティテスト

AIペルソナの視点でChromeブラウザを実操作し、操作感を評価する。

```bash
# Chrome連携モードで起動
claude --chrome
```

```bash
/user-test
```

**実行の流れ:**

1. PRD/MRDがあれば読み込む。なければヒアリング
2. テスト対象のURLを聞かれる
3. ペルソナ一覧が提示される → 承認または修正指示
4. テストシナリオが構成される
5. 1人ずつChromeで実操作してフィードバック
6. 全ペルソナ完了後にサマリが提示される

## 入力ドキュメント

以下のドキュメントがあれば精度が上がる（いずれも任意）:

| ドキュメント | 内容 | 効果 |
|---|---|---|
| PRD | ターゲットユーザー、ペイン、提供価値 | ペルソナの精度向上 |
| MRD | 市場背景、競合、ターゲットセグメント | 競合比較の精度向上 |
| ユーザーストーリー | シナリオ（Given/When/Then） | テストシナリオの精度向上 |
| UI仕様 | 画面定義・インタラクション | 操作フローの精度向上 |

ドキュメントがない場合は、スキル実行時に5項目（プロダクト概要・ターゲット・ペイン・提供価値・主な機能）をヒアリングして補完する。

## トラブルシューティング

### スキルが見つからない

`skills/` ディレクトリが正しい位置にあるか確認する。

```bash
# リポジトリ単体利用の場合
ls skills/

# 既存プロジェクトに組み込んだ場合
ls .claude/skills/
```

### /user-test でChromeが操作できない

1. Claude in Chrome 拡張機能がインストールされているか確認する
2. `claude --chrome` で起動しているか確認する
3. テスト対象のURLにアクセスできるか確認する

### アンケートの回答がエラーになる

survey-respondent エージェントは haiku モデルを使用する。モデルのAPI制限に達した場合はバッチサイズを小さくして再実行する。
