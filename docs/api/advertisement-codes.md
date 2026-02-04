# API設計: 広告コード一覧/検索/新規/編集

Last updated: 2026-02-04

## 1. 画面要件分析: 広告コード一覧/検索/新規/編集

### 表示データ
- id（integer）: 管理ID（advertisement_codes.id）
- advertisement_code（string）: 広告コード
- conversion_code（string）: コンバージョンコード（LCO + 7桁, 自動生成）
- name（string）: 広告名
- postback_setting（string）: ポストバック設定（none/custom/google/meta/tiktok/line）
- friend_add_url（string）: 友だち追加URL（自動生成, 要確認）
- memo（string）: メモ
- custom_settings（object）: custom時の設定（詳細取得のみ。一覧では返却しない）

### ユーザー操作
- 検索実行
- 新規登録
- 編集（PUT）
- 削除（物理削除）

### 関連画面
- 広告コード一覧/検索
- 広告コード新規登録/編集

## 2. エンドポイント一覧

| メソッド | パス | 用途 |
|---------|------|------|
| GET | /api/v1/advertisement-codes | 広告コード一覧/検索 |
| POST | /api/v1/advertisement-codes | 広告コード作成 |
| GET | /api/v1/advertisement-codes/{advertisement_code_id} | 広告コード詳細 |
| PUT | /api/v1/advertisement-codes/{advertisement_code_id} | 広告コード更新 |
| DELETE | /api/v1/advertisement-codes/{advertisement_code_id} | 広告コード削除（物理） |

## 3. リクエスト仕様

### GET /api/v1/advertisement-codes

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

advertisement_codes:
  type: array
  items: string
  required: false
  description: 広告コードの部分一致。複数指定は同名パラメータの繰り返し。

exclude_advertisement_codes:
  type: boolean
  required: false
  description: true の場合、advertisement_codes を除外条件として扱う

name:
  type: string
  required: false
  description: 広告名の部分一致

postback_settings:
  type: array
  items: string (none/custom/google/meta/tiktok/line)
  required: false
  description: ポストバック設定。複数指定可。

memo:
  type: string
  required: false
  description: メモの部分一致

#### バックエンド確認事項
- 複数指定の受け渡しは同名パラメータ繰り返し（暫定）
- 広告コード検索は部分一致（複数はOR）
- 並び順は作成日時の降順固定

### GET /api/v1/advertisement-codes/{advertisement_code_id}

#### パスパラメータ
advertisement_code_id:
  type: integer (int64)
  required: true
  description: 広告コードID

### POST /api/v1/advertisement-codes

#### リクエストボディ
- advertisement_code（必須, 30文字まで）
- name（必須, 30文字まで）
- postback_setting（必須）
- memo（任意, 1000文字まで）
- custom_settings（custom時のみ）
  - postback_url
  - param_1_name / param_2_name / param_3_name（最大3つ）
  - friend_id_param_name（友だち管理ID用のパラメータ名）

#### 補足
- conversion_code は自動生成（LCO + 7桁, 10文字）

### PUT /api/v1/advertisement-codes/{advertisement_code_id}

#### パスパラメータ
advertisement_code_id:
  type: integer (int64)
  required: true
  description: 広告コードID

#### リクエストボディ
POSTと同じ

#### バックエンド確認事項
- 広告コード更新時の conversion_code 再生成ルール（要確認）

### DELETE /api/v1/advertisement-codes/{advertisement_code_id}

#### バックエンド確認事項
- 物理削除 + 関連履歴/設定の連鎖削除（要確認）

## 4. レスポンス構造サンプル

### GET /api/v1/advertisement-codes

```json
{
  "items": [
    {
      "id": 545,
      "advertisement_code": "18ag1",
      "conversion_code": "LCO1234567",
      "name": "ああああ",
      "postback_setting": "custom",
      "friend_add_url": "https://li-connector2.com/ad/?afl=18ag1",
      "memo": null
    }
  ],
  "page": 1,
  "limit": 100,
  "total": 1
}
```

### GET /api/v1/advertisement-codes/{advertisement_code_id}

```json
{
  "id": 545,
  "advertisement_code": "18ag1",
  "conversion_code": "LCO1234567",
  "name": "ああああ",
  "postback_setting": "custom",
  "friend_add_url": "https://li-connector2.com/ad/?afl=18ag1",
  "memo": null,
  "custom_settings": {
    "postback_url": "https://example.com/postback",
    "param_1_name": "param_1",
    "param_2_name": "param_2",
    "friend_id_param_name": "line_friend_id"
  }
}
```

## 5. 共通仕様
- 命名規則・型・エラーレスポンス: `docs/api/conventions.md` を参照
- ベースパス: `/api/v1`
- 認証: Bearer Token（仮置き）

## 6. DDL対応
- advertisement_codes.id
- advertisement_codes.advertisement_code
- advertisement_codes.conversion_code
- advertisement_codes.name
- advertisement_codes.postback_setting
- advertisement_codes.memo
- advertisement_code_custom_settings.custom_settings

## 7. 備考
- 友だち追加URLの生成ルールは要確認
- ポストバック実行タイミングは要確認
- customパラメータ名の制約は要確認
- 置換タグ: {+afn+}, {+afn2+}, {+line_friend_id+}
