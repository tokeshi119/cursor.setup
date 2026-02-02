# API Conventions

Last updated: 2026-02-02

## 1. 基本方針
- DDLを正本とし、APIもそれに合わせる
- JSONのプロパティ名は snake_case
- 列名・enum値はDDLの表記をそのまま使う

## 2. ベースパス
- 既定: `/api/v1`

## 3. 認証（仮置き）
- 方式: Bearer Token
- ヘッダー: `Authorization: Bearer {token}`
- 認証方式が確定したら本項目を更新する

## 4. 型とフォーマット
- ID: integer (int64) 例: `12345`
- 日時: ISO 8601 (timezone付き) 例: `2026-02-02T09:00:00+09:00`
- 真偽値: boolean
- JSON/JSONB: object として扱い、詳細は各APIで定義

## 5. null / optional
- レスポンス: DDLでNULL可能な列は `null` を返す可能性がある
- リクエスト: 任意項目は省略可（`null` を許容するかは各APIで明示）

## 6. ページング
- 方式: `page` / `limit`
- `page`: 1開始
- `limit`: 最大値はAPIごとに定義
- レスポンス例:
  ```json
  {
    "items": [],
    "page": 1,
    "limit": 50,
    "total": 0
  }
  ```

## 7. エラーレスポンス
- 形式:
  ```json
  {
    "error": {
      "code": "VALIDATION_ERROR",
      "message": "入力内容を確認してください",
      "details": [
        {
          "field": "name",
          "issue": "required"
        }
      ],
      "trace_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    }
  }
  ```
- `details` と `trace_id` は任意
