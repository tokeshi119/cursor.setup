# API設計: 配信履歴

Last updated: 2026-02-02

## 1. 画面要件分析: 配信履歴

### 表示データ（一覧）
- id（integer）: 配信ID
- delivery_type（string）: 配信種別（push/greet/reply）
- delivery_status（string）: 配信状態（delivering/completed/error）
- accepted_count（integer）: 受付件数
- executed_count（integer）: 実行件数
- accepted_at（datetime）: 受付日時
- delivery_executed_at（datetime）: 配信実行日時
- execution_completed_at（datetime）: 実行完了日時

### 表示データ（詳細）
- 一覧の項目
- json_data（object）: 配信内容の詳細

### ユーザー操作
- 検索実行
- ページ移動（100件/ページ）
- 並び順は受付日時の降順で固定
- 配信履歴詳細の閲覧

### 事業部からの依頼（固定要件）
- 配信種別は「プッシュメッセージ」「挨拶メッセージ」「自動応答メッセージ」の3種
- プッシュメッセージはCMS側指示で実行
- 履歴情報は詳細表示が理想（簡易でも可）
- 配信内容の詳細を表示
- 1ページ100件程度でOK

## 2. エンドポイント一覧

| メソッド | パス | 用途 |
|---------|------|------|
| GET | /api/v1/delivery-history | 配信履歴一覧/検索 |
| GET | /api/v1/delivery-history/{delivery_history_id} | 配信履歴詳細 |

## 3. リクエスト仕様

### GET /api/v1/delivery-history

#### クエリパラメータ

page:
  type: integer
  required: false
  default: 1
  minimum: 1
  description: ページ番号

limit:
  type: integer
  required: false
  default: 100
  minimum: 1
  maximum: 100
  description: 1ページあたりの件数

delivery_types:
  type: array
  items: string (push/greet/reply)
  required: false
  description: 配信種別。複数指定可。

delivery_statuses:
  type: array
  items: string (delivering/completed/error)
  required: false
  description: 配信状態。複数指定可。

accepted_from:
  type: string
  required: false
  description: 受付日時の開始（JST, ISO 8601）

accepted_to:
  type: string
  required: false
  description: 受付日時の終了（JST, ISO 8601）

#### バックエンド確認事項
- 配信種別の表記（reply を自動応答として扱うか）
- json_data の内容とマスキング要否
- 画像情報が json_data に含まれる前提でよいか（DDLに専用カラムなし）
- 削除ボタンの仕様（削除可否/論理削除の有無）

## 4. レスポンス構造サンプル

### GET /api/v1/delivery-history

```json
{
  "items": [
    {
      "id": 4556,
      "delivery_type": "push",
      "delivery_status": "completed",
      "accepted_count": 600,
      "executed_count": 545,
      "accepted_at": "2025-08-19T13:22:00+09:00",
      "delivery_executed_at": "2025-08-19T13:22:00+09:00",
      "execution_completed_at": "2025-08-19T13:22:00+09:00"
    }
  ],
  "page": 1,
  "limit": 100,
  "total": 1
}
```

### GET /api/v1/delivery-history/{delivery_history_id}

```json
{
  "id": 4556,
  "delivery_type": "push",
  "delivery_status": "completed",
  "accepted_count": 600,
  "executed_count": 545,
  "accepted_at": "2025-08-19T13:22:00+09:00",
  "delivery_executed_at": "2025-08-19T13:22:00+09:00",
  "execution_completed_at": "2025-08-19T13:22:00+09:00",
  "json_data": {
    "title": "ああああああああああ",
    "body": "ああああああああああああああ",
    "button": "あああああ",
    "link_url": "https://aaaa.com/ssss/ssss/ssss"
  }
}
```

## 5. 共通仕様
- 命名規則・型・エラーレスポンス: `docs/api/conventions.md` を参照
- ベースパス: `/api/v1`
- 認証: Bearer Token（仮置き）

## 6. DDL対応（要確認）
- line_delivery_history.delivery_type
- line_delivery_history.delivery_status
- line_delivery_history.accepted_count
- line_delivery_history.executed_count
- line_delivery_history.accepted_at
- line_delivery_history.delivery_executed_at
- line_delivery_history.execution_completed_at
- line_delivery_history.json_data

## 7. 備考
- 詳細表示の項目は json_data の内容に依存
