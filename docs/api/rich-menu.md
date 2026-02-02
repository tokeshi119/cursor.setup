# API設計: リッチメニュー

Last updated: 2026-02-02

## 1. 画面要件分析: リッチメニュー

### 表示データ
- status（string）: 適用ステータス（applied/released）
- menu_bar_text（string）: メニューバー表示（14文字まで）
- image_url（string）: 画像URL
- template_id（integer）: テンプレート（1〜10）
- actions（array）: アクション設定（最大6枠）
  - action_type（string）: url/none
  - url（string）: URL（url指定時のみ）

### 入力データ
- status（必須, applied/released）
- menu_bar_text（必須, 14文字まで）
- image（必須, JPEG/PNG）
- template_id（必須, 1〜10）
- actions（必須, 最大6枠）
  - action_type（必須, url/none）
  - url（任意, action_type=urlのとき必須）

### ユーザー操作
- 適用/解除
- 画像アップロード
- テンプレート選択（10種）
- アクション指定（URL指定/使用しない）
- 適用/解除の実行

### 事業部からの依頼（固定要件）
- 固定で1個のみ
- 「適用」か「解除」のどちらか
- アクションは「URL指定」か「使用しない」の2つのみ
- LINE連携追加時、ステータスが「適用中」なら自動的に適用

## 2. エンドポイント一覧

| メソッド | パス | 用途 |
|---------|------|------|
| GET | /api/v1/rich-menu | リッチメニュー取得 |
| PUT | /api/v1/rich-menu | リッチメニュー更新 |

## 3. リクエスト仕様

### PUT /api/v1/rich-menu

#### リクエストボディ（multipart/form-data）
- status（必須）: applied / released
- menu_bar_text（必須, 14文字まで）
- image（必須）: JPEG/PNG
- template_id（必須）: 1〜10
- actions（必須）: JSON文字列（最大6枠）

#### actionsの形式（例）
```json
[
  { "action_type": "url", "url": "https://example.com" },
  { "action_type": "none" }
]
```

#### テンプレート別の制約（要確認）
- template_id により必要な actions の枠数が異なる
- actions の枠数はテンプレート定義に合わせる

#### バックエンド確認事項
- 画像保存先と公開URLの発行方式
- 画像の寸法/容量制約（幅810pxまたは405px、高さ1200px、1MB以内）
- URLにシステム置換文字が使えない制約の扱い
- LINE連携追加時の自動適用の実装方式
- テンプレート別の actions 枠数（2/3/6など）の定義

## 4. レスポンス構造サンプル

### GET /api/v1/rich-menu

```json
{
  "status": "applied",
  "menu_bar_text": "メニュー",
  "image_url": "https://cdn.example.com/line/rich-menu.png",
  "template_id": 3,
  "actions": [
    { "action_type": "url", "url": "https://example.com" },
    { "action_type": "none", "url": null }
  ]
}
```

## 5. 共通仕様
- 命名規則・型・エラーレスポンス: `docs/api/conventions.md` を参照
- ベースパス: `/api/v1`
- 認証: Bearer Token（仮置き）

## 6. DDL対応（要確認）
- rich_menus.status
- rich_menus.menu_bar_text
- rich_menus.image_url
- rich_menus.template_id
- rich_menu_actions.actions

## 7. 備考
- 固定1件のため、一覧APIは不要
