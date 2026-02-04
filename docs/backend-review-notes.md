# バックエンド確認メモ

Last updated: 2026-02-04

## 1. 概要
フロント主導で作成したAPI設計（OpenAPIドラフト）に対して、バックエンド側で確認・決定が必要な事項を集約する。

## 2. 確定事項（共通）
- ベースパス: /api/v1
- 認証: Bearer Token（仮置き）
- 命名: snake_case
- ページング: page/limit（default=100, max=100）
- エラーレスポンス: error.code / error.message / details / trace_id

## 3. 画面/機能ごとの要確認

### 3-0. 共通（Swagger/認証）
- SwaggerUIローカル閲覧/疎通の servers（localhost + port など）の整理
- Bearer認証の bearerFormat=JWT の扱い（暫定のまま/削除/確定）

### 3-1. 友だち一覧/検索
- 管理コード（incode）の対応カラム
- 複数指定パラメータの受け渡し方式（同名パラメータ繰り返しは暫定）
- 広告コード検索のJOIN方式（advertisement_codes.advertisement_code）
- 物理削除時の連鎖（line_friend_histories 等）

### 3-2. 利用集計（月別/日別/時間別）
- stats_monthly / stats_daily / stats_hourly の参照方針
- 0件埋めの実装方針（12ヶ月/日数/24時間）
- granularityごとの条件付き必須（year/month/date）と指定禁止パラメータのバリデーション方針（400）
- time_zone の固定値（Asia/Tokyo）を型としてどう表現するか（enum化可否）

### 3-3. 広告コード一覧/検索/新規/編集
- conversion_code 再生成ルール（広告コード更新時）
- 友だち追加URLの生成/保存ルール
- ポストバック実行タイミング
- 削除時の連鎖（advertisement_code_custom_settings / postback_history）
- customパラメータ名の制約（許可文字/固定リスト）
- custom/non-custom の条件分岐（oneOf）の判別方法と discriminator 追加要否（クライアント生成の都合）
- custom_settings の additionalProperties: true の扱い（未知キー許容/無視/エラー）

### 3-4. ポストバック履歴
- 広告コード/コンバージョンコードは部分一致でOKか
- 実行日時の範囲検索（from/to）でOKか
- 並び順は実行日時の降順固定でOKか

### 3-5. 管理者一覧/新規/編集
- 物理削除の是非（無効化のみ運用か）
- 一般管理者の一覧からシステム管理者を除外する実装方針

### 3-6. LINE設定（一覧/検索/新規/編集/CSV/一括操作）
- 使用状況/別LINE誘導/振り分け先/生存状況/webhook のenum値とDB値の対応
- 一覧のページャ有無（全件1ページ or page/limit運用）
- 友だち数（合計/非ブロック）と検索対象カラムの定義
- BAN発生日の定義（BAN確定日か最終検知日か）
- BAN検知時は使用状況を自動でinactiveへ遷移、inactiveは自動フェイルチェック対象外
- access_token / channel_secret の表示・マスク方針
- 全項目PUT + マスク返却の整合（更新時の送信仕様、上書き事故対策、secret更新専用APIの要否）
- webhook更新/アクセストークン更新の同期/非同期方針
- CSV取り込み時のバリデーションとエラー返却仕様

### 3-7. 挨拶メッセージ
- 画像保存先と公開URLの発行方式
- 画像のファイルサイズ/寸法制約
- アイコン画像での広告コード計測方式

### 3-8. リッチメニュー
- 画像保存先と公開URLの発行方式
- 画像の寸法/容量制約（幅810pxまたは405px、高さ1200px、1MB以内）
- アクションのURL制約（システム置換文字不可）
- LINE連携追加時の自動適用の実装方式
- template_id ごとの actions 枠数定義（2/3/6など）

### 3-9. 自動応答メッセージ
- message_text / button_label / button_link_url の最大文字数

### 3-10. 配信履歴
- delivery_type の表記（reply=自動応答として扱うか）
- json_data の内容とマスキング要否
- 画像情報が json_data に含まれる前提でよいか（DDLに専用カラムなし）
- 削除ボタンの仕様（削除可否/論理削除の有無）

### 3-11. システム設定
- 単一レコード運用でよいか
- 自動採番の対象（system_settings.id / site_code）
- コンバージョンAPIアクセス先URLの置換文字（拡張タグ）
- 連携キー類のマスク表示/更新時の扱い
- キー類の未指定/空文字/null の扱い（更新しない/クリア可能/空文字許容）

## 4. 今後追加予定の画面
- 追加予定（約3画面）
- 画面確定後、要確認を追記する

## 5. 期限・担当（要記入）
- レビュー期限:
- 担当者:
