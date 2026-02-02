# API設計: 自動応答メッセージ

Last updated: 2026-02-02

## 1. 画面要件分析: 自動応答メッセージ

### 表示データ
- send_status（string）: 送信する/しない（enabled/disabled）
- message_text（string）: 自動返信文
- button_label（string）: ボタン文言
- button_link_url（string）: ボタンリンク先URL

### 入力データ
- send_status（必須）
- message_text（必須）
- button_label（必須）
- button_link_url（必須）

### ユーザー操作
- 送信ON/OFFの切替
- メッセージ内容の編集
- 更新

### 事業部からの依頼（固定要件）
- ユーザーがLINE上で送信したメッセージへの自動返信
- 固定で1つのみ（キーワードマッチによる打ち分けなし）
- 使用タイプはボタンテンプレート型のみ
- 画像は使用しない
- 送る/送らないの設定ができる

## 2. エンドポイント一覧

| メソッド | パス | 用途 |
|---------|------|------|
| GET | /api/v1/auto-response-message | 自動応答メッセージ取得 |
| PUT | /api/v1/auto-response-message | 自動応答メッセージ更新 |

## 3. リクエスト仕様

### PUT /api/v1/auto-response-message

#### リクエストボディ
- send_status（必須）: enabled / disabled
- message_text（必須）
- button_label（必須, 10文字まで）※要確認
- button_link_url（必須, 1000文字まで）※要確認

#### バックエンド確認事項
- message_text の最大文字数
- button_label / button_link_url の制約

## 4. レスポンス構造サンプル

### GET /api/v1/auto-response-message

```json
{
  "send_status": "enabled",
  "message_text": "お問い合わせありがとうございます。",
  "button_label": "詳細を見る",
  "button_link_url": "https://example.com/help"
}
```

## 5. 共通仕様
- 命名規則・型・エラーレスポンス: `docs/api/conventions.md` を参照
- ベースパス: `/api/v1`
- 認証: Bearer Token（仮置き）

## 6. DDL対応（要確認）
- auto_response_messages.send_status
- auto_response_messages.settings

## 7. 備考
- 固定1件のため、一覧APIは不要
