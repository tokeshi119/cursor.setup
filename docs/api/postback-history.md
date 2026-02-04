# API設計: ポストバック履歴

Last updated: 2026-02-04

## 1. 画面要件分析: ポストバック履歴

### 表示データ
- executed_at（datetime）: 実行日時（postback_history.created_at）
- advertisement_code（string）: 広告コード（advertisement_codes.advertisement_code）
- conversion_code（string）: コンバージョンコード（advertisement_codes.conversion_code）
- line_friend_id（integer）: 友だち管理ID（line_friends.id）
- postback_setting（string）: ポストバック種別（advertisement_codes.postback_setting）
- execution_url（string）: 実行URL（postback_history.execution_url）

### ユーザー操作
- 検索実行
- ページ移動（100件/ページ）

### 関連画面
- 広告コード一覧/検索（参照元）

## 2. エンドポイント一覧

| メソッド | パス | 用途 |
|---------|------|------|
| GET | /api/v1/postback-history | ポストバック履歴一覧/検索 |

## 3. リクエスト仕様

### GET /api/v1/postback-history

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

advertisement_code:
  type: string
  required: false
  description: 広告コードの部分一致（単一）

conversion_code:
  type: string
  required: false
  description: コンバージョンコードの部分一致（単一）

line_friend_id:
  type: integer (int64)
  required: false
  description: 友だち管理IDの完全一致（単一）

execution_url:
  type: string
  required: false
  description: 実行URLの部分一致

executed_from:
  type: string
  required: false
  description: 実行日時の開始（JST, ISO 8601）

executed_to:
  type: string
  required: false
  description: 実行日時の終了（JST, ISO 8601）

#### バックエンド確認事項
- 広告コード/コンバージョンコードは部分一致
- 実行日はfrom/toの範囲検索
- 並び順は実行日時の降順固定

## 4. レスポンス構造サンプル

```json
{
  "items": [
    {
      "executed_at": "2026-04-01T10:57:00+09:00",
      "advertisement_code": "18ag1",
      "conversion_code": "LCO1234567",
      "line_friend_id": 545,
      "postback_setting": "google",
      "execution_url": "https://ads.example.com/track?af=18ag1&line_friend_id=545"
    }
  ],
  "page": 1,
  "limit": 100,
  "total": 1
}
```

## 5. 共通仕様
- 命名規則・型・エラーレスポンス: `docs/api/conventions.md` を参照
- ベースパス: `/api/v1`
- 認証: Bearer Token（仮置き）

## 6. DDL対応
- postback_history.created_at
- postback_history.execution_url
- postback_history.advertisement_code_id
- postback_history.line_friend_id
- advertisement_codes.advertisement_code
- advertisement_codes.conversion_code
- advertisement_codes.postback_setting
- line_friends.id
