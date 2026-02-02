---
name: line-mock-api-design
overview: 提供されたLINE設定系のモック（検索/一覧/手動登録/CSVインポート/編集）から、必要なAPI仕様を抽出してOpenAPIへ落とし込む。既存の設計・命名規約に合わせ、一覧/詳細/作成/更新/削除/一括操作/CSV入出力/チェック系アクションまで整理する。
todos:
  - id: extract-ui-requirements
    content: モック画像から表示/操作/制約を整理
    status: in_progress
  - id: define-endpoints
    content: リソース名とCRUD+アクション+CSVのAPI案作成
    status: pending
  - id: spec-requests-responses
    content: 検索/登録/更新/一括/CSVの仕様定義
    status: pending
  - id: update-openapi-docs
    content: OpenAPIとAPI設計書に反映
    status: pending
isProject: false
---

# LINE設定API設計プラン

## 対象と前提

- 対象画面: LINE設定の検索/一覧、手動登録、CSVインポート登録、編集（添付モック5枚）
- 命名・型・エラー形式は [`docs/api/conventions.md`](docs/api/conventions.md) に準拠
- 既存仕様は [`docs/openapi/openapi.yaml`](docs/openapi/openapi.yaml) と既存API設計（例: [`docs/api/friends.md`](docs/api/friends.md)）に合わせる

## 進め方

1. **画面要件の抽出**

- モック画像から表示項目・入力項目・一括操作・各種チェック/更新の操作を洗い出す
- 例: 一覧テーブルの列（使用状況、ベーシックID、友だち数/非ブロック数、配信状況、webhook、BAN発生日、システムメモ等）と検索条件（範囲/部分一致/チェック項目）を整理
- 画面別に「表示データ」「ユーザー操作」「制約」を整理した要件メモを作成

2. **リソース/エンドポイント方針の確定**

- 基本CRUD: 一覧/詳細/作成/更新/削除
- 追加アクション: 連携チェック、webhookチェック/更新、アクセストークン更新、一括更新
- CSV系: フォーマットDL、インポート、エクスポート
- リソース名は既存規約に合わせて提案（例: `/line-accounts` or `/line-official-accounts` の拡張）

3. **リクエスト仕様の定義**

- 一覧検索のクエリパラメータ（ID/状態/範囲/部分一致/数値レンジ/日付レンジ）
- 手動登録/編集のボディ（channel_id/secret/basic_id/振り分け/別LINE誘導/メモ 等）
- 一括操作のリクエスト（対象ID配列 + 操作種別）
- CSVインポートのファイル仕様（文字コード/列順/制約）

4. **レスポンス構造の設計**

- 一覧/詳細/操作結果のレスポンス例を作成
- アクション系（チェック/更新）の成功・失敗・詳細（結果内訳）を定義

5. **OpenAPI反映と引き継ぎメモ**

- [`docs/openapi/openapi.yaml`](docs/openapi/openapi.yaml) に新規パス/スキーマ/パラメータを追加
- 画面ごとのAPI設計書を新規作成（例: [`docs/api/line-accounts.md`](docs/api/line-accounts.md)）
- バックエンド確認事項（データソース/更新の副作用/バッチ処理/権限制御）をメモ化

## 主要アウトプット

- 画面要件分析（モック起点）
- エンドポイント一覧
- リクエスト/レスポンス例
- 共通仕様の準拠メモ
- OpenAPI更新

## 変更予定ファイル

- [`docs/openapi/openapi.yaml`](docs/openapi/openapi.yaml)
- [`docs/api/line-accounts.md`](docs/api/line-accounts.md)（新規予定）
- 必要に応じて [`docs/backend-review-notes.md`](docs/backend-review-notes.md) へ引き継ぎ項目追記

## 注意点

- 既存の `/line-official-accounts` との責務重複があるため、リソース統合/分離を検討
- webhook/アクセストークン更新の責務（即時実行 or 非同期）を明確化
- CSVインポートのバリデーション/エラー返却形式を明確化