# API設計: 友だち一覧/検索

Last updated: 2026-02-02

## 1. 画面要件分析: 友だち一覧/検索

### 表示データ
- id（integer）: 管理ID（line_friends.id）
- management_code（string）: 管理コード（incode）※要確認
- advertisement_code（string）: 広告コード（advertisement_codes.advertisement_code）
- line_id（string）: LINE_ID（line_friends.messaging_api_user_id）
- registered_at（datetime）: 友だち登録日時（line_friends.created_at）
- is_blocked（boolean）: 友だち状態（ブロック判定）
- line_official_account_id（integer）: LINE連携ID
- line_official_basic_id（string）: LINE連携 basic_id

### ユーザー操作
- 検索実行
- ページ移動（100件/ページ）
- 友だち削除（物理削除）

### 関連画面
- 友だち検索（検索フォーム）
- 友だち一覧（結果表示）

## 2. エンドポイント一覧

| メソッド | パス | 用途 |
|---------|------|------|
| GET | /api/v1/friends | 友だち一覧/検索 |
| DELETE | /api/v1/friends/{friend_id} | 友だち削除（物理） |
| GET | /api/v1/line-official-accounts | LINE連携候補取得（id/basic_id） |

## 3. リクエスト仕様

### GET /api/v1/friends

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

management_ids:
  type: array
  items: integer (int64)
  required: false
  description: 管理IDの完全一致。複数指定は同名パラメータの繰り返し。

exclude_management_ids:
  type: boolean
  required: false
  description: true の場合、management_ids を除外条件として扱う

management_codes:
  type: array
  items: string
  required: false
  description: 管理コード（incode）の部分一致。複数指定は同名パラメータの繰り返し。※要確認

exclude_management_codes:
  type: boolean
  required: false
  description: true の場合、management_codes を除外条件として扱う

advertisement_codes:
  type: array
  items: string
  required: false
  description: 広告コードの部分一致。複数指定は同名パラメータの繰り返し。

exclude_advertisement_codes:
  type: boolean
  required: false
  description: true の場合、advertisement_codes を除外条件として扱う

line_id:
  type: string
  required: false
  description: LINE_IDの部分一致

registered_from:
  type: string
  required: false
  description: 登録日時の開始（JST, ISO 8601）

registered_to:
  type: string
  required: false
  description: 登録日時の終了（JST, ISO 8601）

friend_statuses:
  type: array
  items: string (normal | blocked)
  required: false
  description: 友だち状態（正常/ブロック）。複数指定可。

line_official_basic_ids:
  type: array
  items: string
  required: false
  description: LINE連携 basic_id の完全一致。複数指定は同名パラメータの繰り返し。

exclude_line_official_basic_ids:
  type: boolean
  required: false
  description: true の場合、line_official_basic_ids を除外条件として扱う

#### バックエンド確認事項
- 複数指定の受け渡しは同名パラメータ繰り返し（暫定）
- 管理コード（incode）の対応カラム
- 広告コードは advertisement_codes.advertisement_code で検索（JOIN想定）
- 並び順は登録日時の降順固定

### DELETE /api/v1/friends/{friend_id}

#### パスパラメータ
friend_id:
  type: integer (int64)
  required: true
  description: 友だちID（line_friends.id）

#### バックエンド確認事項
- 物理削除 + 関連履歴（line_friend_histories 等）の連鎖削除

### GET /api/v1/line-official-accounts
※クエリパラメータなし

## 4. レスポンス構造サンプル

### GET /api/v1/friends

```json
{
  "items": [
    {
      "id": 545,
      "management_code": null,
      "advertisement_code": "yokota_x",
      "line_id": "U11ae9eba73ac607c52138ed61bc0183a",
      "registered_at": "2026-04-01T10:57:00+09:00",
      "is_blocked": true,
      "line_official_account_id": 1,
      "line_official_basic_id": "@937jhhcx"
    }
  ],
  "page": 1,
  "limit": 100,
  "total": 9
}
```

### GET /api/v1/line-official-accounts

```json
{
  "items": [
    {
      "id": 1,
      "basic_id": "@937jhhcx"
    }
  ]
}
```

## 5. 共通仕様
- 命名規則・型・エラーレスポンス: `docs/api/conventions.md` を参照
- ベースパス: `/api/v1`
- 認証: Bearer Token（仮置き）

## 6. DDL対応
- line_friends.id
- line_friends.messaging_api_user_id
- line_friends.created_at
- line_friends.is_blocked
- line_friends.advertisement_code_id
- line_friends.line_official_account_id
- advertisement_codes.advertisement_code
- line_official_accounts.basic_id

## 7. 備考
- 管理コード（incode）は要確認
