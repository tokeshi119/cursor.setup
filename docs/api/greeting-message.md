# API設計: 挨拶メッセージ

Last updated: 2026-02-02

## 1. 画面要件分析: 挨拶メッセージ

### 表示データ
- send_timing（string）: 送信タイミング（friend_added / unblocked）
- icon_image_url（string）: LINEアイコン画像URL
- notice_image_url（string）: 通知用画像URL
- notice_title（string）: 通知用タイトル
- notice_body（string）: 通知用本文
- notice_button_label（string）: 通知用ボタン
- notice_link_url（string）: 通知用リンク先

### 入力データ
- send_timing（必須, 単一選択）
- icon_image（任意, JPEG/PNG）
- notice_image（任意, JPEG/PNG）
- notice_title（必須, 40文字以内）
- notice_body（必須, 60文字以内）
- notice_button_label（必須, 10文字以内）
- notice_link_url（必須, 1000文字以内）

### ユーザー操作
- 送信切り替え（初回/ブロック解除時）
- 画像アップロード（LINEアイコン画像/通知用画像）
- テキスト入力（タイトル/本文/ボタン/リンク先）
- プレビュー表示
- 更新

### 事業部からの依頼（固定要件）
- 固定で1種類のみ
- 使用タイプはボタンテンプレート型のみ
- 初回時/ブロック解除時は共通でOK
- 広告コードの計測はアイコン画像側で実施

## 2. エンドポイント一覧

| メソッド | パス | 用途 |
|---------|------|------|
| GET | /api/v1/greeting-message | 挨拶メッセージ取得 |
| PUT | /api/v1/greeting-message | 挨拶メッセージ更新 |

## 3. リクエスト仕様

### PUT /api/v1/greeting-message

#### リクエストボディ（multipart/form-data）
- send_timing（必須）: friend_added / unblocked
- icon_image（任意）: JPEG/PNG
- notice_image（任意）: JPEG/PNG
- notice_title（必須, 40文字以内）
- notice_body（必須, 60文字以内）
- notice_button_label（必須, 10文字以内）
- notice_link_url（必須, 1000文字以内）

#### バックエンド確認事項
- 画像保存先と公開URLの発行方式
- 画像のファイルサイズ/寸法制約
- アイコン画像での広告コード計測方式

## 4. レスポンス構造サンプル

### GET /api/v1/greeting-message

```json
{
  "send_timing": "friend_added",
  "icon_image_url": "https://cdn.example.com/line/icon.png",
  "notice_image_url": "https://cdn.example.com/line/notice.png",
  "notice_title": "ようこそ",
  "notice_body": "まずはメニューをご確認ください。",
  "notice_button_label": "開く",
  "notice_link_url": "https://example.com/welcome"
}
```

## 5. 共通仕様
- 命名規則・型・エラーレスポンス: `docs/api/conventions.md` を参照
- ベースパス: `/api/v1`
- 認証: Bearer Token（仮置き）

## 6. DDL対応（要確認）
- greeting_messages.send_timing
- greeting_messages.icon_image_url
- greeting_messages.notice_image_url
- greeting_messages.notice_title
- greeting_messages.notice_body
- greeting_messages.notice_button_label
- greeting_messages.notice_link_url

## 7. 備考
- 固定1種類のため、一覧APIは不要
- テンプレート型はボタンテンプレートのみ
