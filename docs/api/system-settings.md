# API設計: システム設定

Last updated: 2026-02-02

## 1. 画面要件分析: システム設定

### 表示データ
- id（integer）: システム設定ID（自動採番、要確認）
- site_name（string）: 利用サイト名
- system_domain（string）: システムドメイン
- site_code（string）: サイトコード（自動発番、要確認）
- image_storage_directory（string）: 画像保存ディレクトリ（パス）
- internal_image_url（string）: サイト内画像URL
- external_image_url（string）: 外部サーバー画像URL
- line_access_key_masked（string）: LINE連携用アクセスキー（RPA連携キー、マスク表示）
- cms_webhook_key_masked（string）: CMS Webhook Key（マスク表示）
- cms_api_key_masked（string）: CMS API Key（マスク表示）
- conversion_api_target_url（string）: コンバージョンAPIアクセス先URL

### 入力データ（更新）
- site_name（必須）
- system_domain（必須）
- image_storage_directory（必須）
- internal_image_url（必須）
- external_image_url（必須）
- line_access_key（任意、未指定/空は更新しない想定）
- cms_webhook_key（任意、未指定/空は更新しない想定）
- cms_api_key（任意、未指定/空は更新しない想定）
- conversion_api_target_url（必須、置換文字は要確認）

### ユーザー操作
- システム設定の表示（単一レコード想定、要確認）
- システム設定の更新

## 2. エンドポイント一覧

| メソッド | パス | 用途 |
|---------|------|------|
| GET | /api/v1/system-settings | システム設定取得 |
| PUT | /api/v1/system-settings | システム設定更新 |

## 3. リクエスト仕様

### PUT /api/v1/system-settings

#### リクエストボディ
- site_name（必須）
- system_domain（必須）
- image_storage_directory（必須）
- internal_image_url（必須）
- external_image_url（必須）
- line_access_key（任意、未指定/空は更新しない想定）
- cms_webhook_key（任意、未指定/空は更新しない想定）
- cms_api_key（任意、未指定/空は更新しない想定）
- conversion_api_target_url（必須）

#### バックエンド確認事項
- 単一レコード運用でよいか
- 自動採番の対象（system_settings.id / site_code）
- コンバージョンAPIアクセス先URLの置換文字（拡張タグ）
- 連携キー類の更新時の扱い（空文字の許可、マスク方針）

## 4. レスポンス構造サンプル

### GET /api/v1/system-settings

```json
{
  "id": 1,
  "site_name": "利用サイト名",
  "system_domain": "example.com",
  "site_code": "LCONNESYS-2",
  "image_storage_directory": "/var/www/images",
  "internal_image_url": "https://example.com/images/",
  "external_image_url": "https://cdn.example.com/images/",
  "line_access_key_masked": "****abcd",
  "cms_webhook_key_masked": "****wxyz",
  "cms_api_key_masked": "****1234",
  "conversion_api_target_url": "https://example.com/conversion?afn={+afn+}&line_friend_id={+line_friend_id+}"
}
```

## 5. 共通仕様
- 命名規則・型・エラーレスポンス: `docs/api/conventions.md` を参照
- ベースパス: `/api/v1`
- 認証: Bearer Token（仮置き）

## 6. DDL対応（要確認、サンプルとして記述）
- system_settings.id
- system_settings.site_name
- system_settings.system_domain
- system_settings.site_code
- system_settings.image_storage_directory
- system_settings.internal_image_url
- system_settings.external_image_url
- system_settings.line_access_key
- system_settings.cms_webhook_key
- system_settings.cms_api_key
- system_settings.conversion_api_target_url

## 7. 備考
- アクセスキーはマスク表示、更新は同一エンドポイントで実施（再発行専用ではない）
