# プロジェクト構成

## ディレクトリ構成

```
docs/
├── index.md               # Required — list of feature areas and sub-items
├── project_overview.md    # Required — overall project overview
├── glossary.md            # Required — domain-specific terminology
├── tags.md                # Required — inline tag definitions and schema
└── {feature-area}/        # One directory per feature area
    ├── overview.md
    ├── spec.md
    ├── design.md
    ├── tasks.md
    ├── test-cases.md
    ├── dev-notes.md
    └── {sub-item}/        # Created as needed per split rules
        └── ...

templates/                 # Document templates (all copied by /add-feature)
├── overview.md
├── spec.md
├── design.md
├── tasks.md
├── test-cases.md
└── dev-notes.md

issues/                    # Bug ticket management
├── index.md               # Ticket list and state legend
├── templates/
│   └── BUG-template.md
└── tickets/
    └── BUG-{NNN}.md        # Added sequentially as bugs are reported

tests/                     # Automated tests (see docs/testing.md for the line)

tmp/                       # Not committed. Working area
├── 確認/                   # Verification requests written by the agent
│   └── old/               #   archived after each round
└── フィードバック/           # Reports from the verification session
    └── old/               #   archived after each round
```

---

## スキル

| スキル | 用途 |
|---|---|
| `/add-feature <path>` | 機能エリアまたはサブ項目を作成する: ディレクトリ、テンプレート、index.md の行 |
| `/verify-request <title> [--run <id>] [--workflow <name>]` | 確認依頼書を生成する。**CI の成果物をスクリプト自身が測る**（大きさ、SHA256）ので写し間違いが起きない。作業ツリーが汚れているときや、HEAD が run と異なるときに警告し、前回の未了項目を持ち越す。`--run` を指定しなければ、現在のブランチで最新の緑の run を選ぶ。`gh`・`git`・`awk` が必要 |

---

## ドキュメントのテンプレート

### project_overview.md
- **目的・背景**: このプロジェクトが解決する問題と、存在する理由
- **スコープ**: 作るもの・作らないもの
- **技術スタックの概要**: 使用する技術とその理由
- **全体のアーキテクチャ**: システムの大まかな構造
- **制約**: 技術・環境・リソースの制限

### overview.md（機能エリアごと）
- **目的・背景**: この機能が必要な理由
- **スコープ**: 作るもの・作らないもの
- **制約**: 技術・リソースの制限
- **完了の定義**: 何をもって完了とするか

### spec.md
- **機能一覧**: 提供する機能の列挙
- **画面・操作フロー**: ユーザー操作の手順
- **各画面 / 状態の詳細**: 表示内容、操作、遷移
- **エラーケース**: 異常時の挙動
- **未対応ケース**: 意図的に扱わないこと

### design.md
- **技術選定**: 使用する技術とその理由
- **アーキテクチャ**: コンポーネント構成
- **データ構造**: 主要なデータモデルとスキーマ
- **インターフェース**: API と関数のインターフェース
- **依存関係**: 外部ライブラリとサービス

### tasks.md
- **タスク一覧**: チェックボックス形式
- **依存関係**: タスク間の順序制約
- **ステータス**: 未着手 / 進行中 / 完了

### test-cases.md
- spec.md の機能一覧の各行は、テストケース列にテストケースの ID を書くか、
  明示的に「無し」と書く。空欄は見落とし、「無し」は判断の結果である —
  そして空欄の機能は、移植・書き換えの網羅表から見えなくなる
- E2E の行には **対照（control）** 列を設ける。対照が無い場合はそう書く:
  失敗し得ない項目は証拠にならない

### dev-notes.md
- **実装上の判断**: ある方法を選んだ理由
- **問題と解決**: 遭遇した問題とその解決方法
- **設計からの逸脱**: 設計書との違いとその理由
- **今後の課題**: 現時点の制限と、後で対応すべき事項
- **ユーザーへの依頼**: スキル・権限・情報が不足しているときに記録する
- **「どう確認するか」** の節をここに置く。確認の方法はコードよりも速く積み上がり、
  次のプロジェクトへ持っていく価値が最も高い部分である

### 確認依頼書（.claude/skills/verify-request/template.md）
- `templates/` ではなくスキルの中に置く。そのため `/add-feature` はこれをコピーしない
- ヘッダの成果物の表は**生成される**もので、手で書くことはない
- **測ったこと / 推測していること** — どちらか示していない主張は許されない
- 項目ごとの **対照** 列、`0` を期待するすべての項目とその対照の表、
  そして対照がまったく無い項目を挙げる節を設ける
- 末尾は「報告の書式」で終わる。確認セッションはこの書式で返答する（フィードバック用の
  雛形を別に置かない — 確認セッションがこのリポジトリを持っているとは限らないため）

### docs/testing.md
- 初日に引く線: 対象環境無しで CI で回るもの、実機が必要なため
  手動のループに残るもの、そしてその理由

### バグ票（issues/tickets/BUG-{NNN}.md）

`issues/templates/BUG-template.md` に従って作成する。
