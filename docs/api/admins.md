# API設計: 管理者一覧/新規/編集

Last updated: 2026-02-02

## 1. 画面要件分析: 管理者一覧/新規/編集

### 表示データ
- id（integer）: 管理ID（admins.id）
- is_active（boolean）: 状態（有効/無効）
- name（string）: 管理者名
- login_id（string）: ログインID
- role（string）: 権限（admin/normal）
- memo（string）: メモ

### ユーザー操作
- 新規登録
- 編集（PUT）
- 削除（物理削除, 要確認）

### 関連画面
- 管理者一覧
- 管理者編集

## 2. エンドポイント一覧

| メソッド | パス | 用途 |
|---------|------|------|
| GET | /api/v1/admins | 管理者一覧 |
| POST | /api/v1/admins | 管理者作成 |
| PUT | /api/v1/admins/{admin_id} | 管理者更新 |
| DELETE | /api/v1/admins/{admin_id} | 管理者削除（物理） |

## 3. リクエスト仕様

### GET /api/v1/admins

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

#### バックエンド確認事項
- 一般管理者はシステム管理者を一覧に表示しない

### POST /api/v1/admins

#### リクエストボディ
- is_active（必須）
- name（必須）
- login_id（必須）
- role（必須: admin/normal）
- memo（任意）

#### 補足
- cognito_sub はサーバー側で設定（フロント入力なし）

### PUT /api/v1/admins/{admin_id}

#### パスパラメータ
admin_id:
  type: integer (int64)
  required: true
  description: 管理者ID

#### リクエストボディ
POSTと同じ

### DELETE /api/v1/admins/{admin_id}

#### バックエンド確認事項
- 物理削除の是非（要確認）

## 4. レスポンス構造サンプル

### GET /api/v1/admins

```json
{
  "items": [
    {
      "id": 1,
      "is_active": true,
      "name": "テスト専用",
      "login_id": "test",
      "role": "admin",
      "memo": null
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
- admins.id
- admins.is_active
- admins.name
- admins.login_id
- admins.role
- admins.memo
- admins.cognito_sub（サーバー側で設定）

## 7. 備考
- 物理削除の是非は要確認
