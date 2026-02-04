# API Conventions

Last updated: 2026-02-04

## 1. 基本方針

- API の正本は OpenAPI（`docs/openapi/openapi.yaml`）
- DDL は DB 実装の正本（API と 1:1 で一致しない場合があるため、齟齬があればバックエンドレビューで調整）
- JSON のプロパティ名は snake_case
- フィールド名・enum 値は OpenAPI の定義を正とし、DDL とズレる場合はどちらかに寄せる（要確認）

## 2. ベースパス

- 既定: `/api/v1`

## 3. 認証（仮置き）

- 方式: Bearer Token
- ヘッダー: `Authorization: Bearer {token}`
- 認証方式が確定したら本項目を更新する

## 4. 型とフォーマット

- ID: integer (int64) 例: `12345`
- 日時: ISO 8601 (timezone 付き) 例: `2026-02-02T09:00:00+09:00`
- 真偽値: boolean
- JSON/JSONB: object として扱い、詳細は各 API で定義

## 5. null / optional

- レスポンス: DDL で NULL 可能な列は `null` を返す可能性がある
- リクエスト: 任意項目は省略可（`null` を許容するかは各 API で明示）

## 6. ページング

- 方式: `page` / `limit`
- `page`: 1 開始
- `limit`: default=100, max=100（暫定、OpenAPI に準拠）
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

## 8. エラーコード一覧（仮）

以下は HTTP ステータスごとのエラーコードです。**バックエンドと要確認**。

| HTTP ステータス | error.code         | 説明                     | message 例                           |
| --------------- | ------------------ | ------------------------ | ------------------------------------ |
| 400             | `VALIDATION_ERROR` | リクエストパラメータ不正 | リクエストパラメータが不正です。     |
| 401             | `UNAUTHORIZED`     | 認証が必要               | 認証が必要です。                     |
| 403             | `FORBIDDEN`        | 権限なし                 | この操作を実行する権限がありません。 |
| 404             | `NOT_FOUND`        | リソースが存在しない     | 対象のリソースが見つかりません。     |
| 409             | `CONFLICT`         | リソース競合・重複       | リソースが既に存在します。           |
| 500             | `INTERNAL_ERROR`   | サーバー内部エラー       | サーバー内部でエラーが発生しました。 |

### 備考

- 上記コード値は仮定義のため、バックエンド実装時に調整の可能性あり
- 必要に応じて追加のエラーコード（例: `TOO_MANY_REQUESTS` など）を定義予定
