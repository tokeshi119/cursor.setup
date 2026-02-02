# API設計: LINE設定（一覧/検索/新規/編集/CSV/一括操作）

Last updated: 2026-02-02

## 1. 画面要件分析: LINE設定

### 表示データ（一覧）
- id（integer）: 管理ID
- usage_status（string）: 使用状況（使用中/未使用/無効）
- basic_id（string）: ベーシックID（@から始まるID）
- friend_count_total（integer）: 友だち数（合計）
- friend_count_non_blocked（integer）: 友だち数（非ブロック）
- line_guidance_status（string）: 別LINE誘導（ON/OFF）
- distribution_target（string）: 振り分け先（対象/除外）
- delivery_limit（integer）: 配信数上限
- current_month_delivery_count（integer）: 当月配信数
- delivery_consumption_rate（number）: 配信消化率（%）
- liveness_status（string）: 生存状況（正常/BAN）
- webhook_status（string）: webhook（OK/Error）
- ban_at（datetime）: BAN発生日
- system_memo_masked（string）: システムメモ（一覧ではマスク表示）

### 入力データ（新規/編集）
- usage_status（必須）
- basic_id（必須, 20文字以内）※要確認
- channel_id（必須, 200文字以内）※要確認
- channel_secret（必須, 200文字以内）※要確認
- line_guidance_status（必須）
- distribution_target（必須）
- system_memo（任意, 1000文字以内）

### 表示データ（編集）
- access_token（string）※表示/マスク方針は要確認
- access_token_expires_at（datetime）
- redirect_url（string）
- webhook_url（string）

### ユーザー操作
- 検索実行
- ページ移動（100件/ページ）
- 新規登録（完了して連携チェック）
- 編集（完了して連携チェック）
- 削除（物理削除）
- 一括操作（使用状況/別LINE誘導/振り分け先の切替）
- 生存チェック（複数）
- webhookチェック（複数）
- webhook更新（複数）
- アクセストークン更新（複数）
- CSVフォーマットダウンロード
- CSVインポート登録
- CSVダウンロード
- リッチメニュー自動適用（リッチメニューが適用中の場合）

### 関連画面
- 友だち一覧/検索（LINE連携の参照）

## 2. エンドポイント一覧

| メソッド | パス | 用途 |
|---------|------|------|
| GET | /api/v1/line-accounts | LINE設定一覧/検索 |
| POST | /api/v1/line-accounts | LINE設定作成（手動登録） |
| GET | /api/v1/line-accounts/{line_account_id} | LINE設定詳細 |
| PUT | /api/v1/line-accounts/{line_account_id} | LINE設定更新 |
| DELETE | /api/v1/line-accounts/{line_account_id} | LINE設定削除（物理） |
| POST | /api/v1/line-accounts/{line_account_id}/linkage-check | 連携チェック |
| POST | /api/v1/line-accounts/bulk/usage-status | 使用状況の一括切替 |
| POST | /api/v1/line-accounts/bulk/line-guidance | 別LINE誘導の一括切替 |
| POST | /api/v1/line-accounts/bulk/distribution-target | 振り分け先の一括切替 |
| POST | /api/v1/line-accounts/bulk/liveness-check | 生存チェック（複数） |
| POST | /api/v1/line-accounts/bulk/webhook-check | webhookチェック（複数） |
| POST | /api/v1/line-accounts/bulk/webhook-refresh | webhook更新（複数） |
| POST | /api/v1/line-accounts/bulk/access-token-refresh | アクセストークン更新（複数） |
| GET | /api/v1/line-accounts/csv-format | CSVフォーマットダウンロード |
| POST | /api/v1/line-accounts/csv-import | CSVインポート登録 |
| GET | /api/v1/line-accounts/csv | CSVダウンロード |

## 3. リクエスト仕様

### GET /api/v1/line-accounts

#### 要相談
- 検索条件の対象範囲（どの項目をサーバー側検索に含めるか）
- 友だち数/配信数/消化率など集計系の検索条件の可否

#### 暫定（最小条件のみ）
- 管理ID（management_ids）
- 使用状況（usage_statuses）
- ベーシックID（basic_id）

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

usage_statuses:
  type: array
  items: string (in_use/unused/disabled)
  required: false
  description: 使用状況。複数指定可。

basic_id:
  type: string
  required: false
  description: ベーシックIDの部分一致

friend_count_type:
  type: string
  required: false
  description: 友だち数の対象（total/non_blocked）

friend_count_min:
  type: integer
  required: false
  description: 友だち数の下限

friend_count_max:
  type: integer
  required: false
  description: 友だち数の上限

line_guidance_statuses:
  type: array
  items: string (on/off)
  required: false
  description: 別LINE誘導の表示。複数指定可。

distribution_targets:
  type: array
  items: string (target/excluded)
  required: false
  description: 振り分け先。複数指定可。

delivery_limit_min:
  type: integer
  required: false
  description: 配信数上限の下限

delivery_limit_max:
  type: integer
  required: false
  description: 配信数上限の上限

current_month_delivery_count_min:
  type: integer
  required: false
  description: 当月配信数の下限

current_month_delivery_count_max:
  type: integer
  required: false
  description: 当月配信数の上限

delivery_consumption_rate_min:
  type: number
  required: false
  description: 配信消化率（%）の下限

delivery_consumption_rate_max:
  type: number
  required: false
  description: 配信消化率（%）の上限

liveness_statuses:
  type: array
  items: string (normal/banned)
  required: false
  description: 生存状況。複数指定可。

webhook_statuses:
  type: array
  items: string (ok/error)
  required: false
  description: webhookの状態。複数指定可。

ban_from:
  type: string
  required: false
  description: BAN発生日の開始（JST, ISO 8601）

ban_to:
  type: string
  required: false
  description: BAN発生日の終了（JST, ISO 8601）

system_memo:
  type: string
  required: false
  description: システムメモの部分一致

#### バックエンド確認事項
- 管理IDの複数指定の受け渡し方式（同名パラメータ繰り返しは暫定）
- 友だち数の対象指定（合計/非ブロック）の扱い
- 各数値レンジの対象カラム
- BAN発生日の定義（BAN確定日か最終検知日か）

### POST /api/v1/line-accounts

#### リクエストボディ
- usage_status（必須）
- basic_id（必須）
- channel_id（必須）
- channel_secret（必須）
- line_guidance_status（必須）
- distribution_target（必須）
- system_memo（任意）

### PUT /api/v1/line-accounts/{line_account_id}

#### パスパラメータ
line_account_id:
  type: integer (int64)
  required: true
  description: LINE設定ID

#### リクエストボディ
POSTと同じ

### POST /api/v1/line-accounts/{line_account_id}/linkage-check

#### パスパラメータ
line_account_id:
  type: integer (int64)
  required: true
  description: LINE設定ID

### 一括切替（使用状況/別LINE誘導/振り分け先）

#### リクエストボディ（共通）
- line_account_ids（必須, 配列）
- value（必須, 切替値）

### POST /api/v1/line-accounts/csv-import

#### リクエストボディ（multipart/form-data）
- file（必須）: CSVファイル

#### CSVフォーマット
- 文字コード: UTF-8
- 1行目: ヘッダーとして読み飛ばし
- カンマ区切り
- 列定義:
  - A列: 状態（0=無効, 1=使用中, 2=未使用）
  - B列: ベーシックID（20文字以内, 半角英数字）※要確認
  - C列: ChannelID（200文字以内, 半角英数字）※要確認
  - D列: Channel secret（200文字以内, 半角英数字）※要確認
  - E列: 別LINE誘導（0=無効, 1=OFF）
  - F列: 振り分け先（0=除外, 1=対象）
  - G列: システムメモ（1000文字以内）

## 4. レスポンス構造サンプル

### GET /api/v1/line-accounts

```json
{
  "items": [
    {
      "id": 45,
      "usage_status": "in_use",
      "basic_id": "@213huezt",
      "friend_count_total": 125,
      "friend_count_non_blocked": 54,
      "line_guidance_status": "on",
      "distribution_target": "excluded",
      "delivery_limit": 500,
      "current_month_delivery_count": 25,
      "delivery_consumption_rate": 5.0,
      "liveness_status": "normal",
      "webhook_status": "ok",
      "ban_at": null,
      "system_memo_masked": "******"
    }
  ],
  "page": 1,
  "limit": 100,
  "total": 1
}
```

### GET /api/v1/line-accounts/{line_account_id}

```json
{
  "id": 224,
  "usage_status": "unused",
  "basic_id": "@213huezt",
  "channel_id": "2007592653",
  "channel_secret": "ffff86b35fb8144ef1cebbeaa242cde8",
  "line_guidance_status": "off",
  "distribution_target": "target",
  "system_memo": null,
  "access_token_expires_at": "2025-09-18T02:00:02+09:00",
  "access_token": "xxxxx",
  "redirect_url": "https://line.me/R/ti/p/%40l23huezt",
  "webhook_url": "https://muteking.com/api/linebot/"
}
```

### POST /api/v1/line-accounts/bulk/liveness-check

```json
{
  "summary": {
    "requested": 3,
    "succeeded": 2,
    "failed": 1
  },
  "items": [
    {
      "id": 35,
      "status": "success"
    },
    {
      "id": 40,
      "status": "failed",
      "message": "アクセストークンが無効"
    }
  ]
}
```

### POST /api/v1/line-accounts/csv-import

```json
{
  "imported_count": 10,
  "skipped_count": 2,
  "errors": [
    {
      "row_number": 5,
      "message": "B列: ベーシックIDの形式が不正"
    }
  ]
}
```

## 5. 共通仕様
- 命名規則・型・エラーレスポンス: `docs/api/conventions.md` を参照
- ベースパス: `/api/v1`
- 認証: Bearer Token（仮置き）

## 6. DDL対応（要確認）
- line_official_accounts.id
- line_official_accounts.basic_id
- line_official_accounts.channel_id
- line_official_accounts.channel_secret
- line_official_accounts.usage_status
- line_official_accounts.line_guidance_status
- line_official_accounts.distribution_target
- line_official_accounts.system_memo
- line_official_accounts.webhook_url
- line_official_accounts.redirect_url
- line_official_account_tokens.access_token
- line_official_account_tokens.access_token_expires_at
- line_official_account_stats.friend_count_total
- line_official_account_stats.friend_count_non_blocked
- line_official_account_stats.current_month_delivery_count
- line_official_account_stats.delivery_limit
- line_official_account_stats.delivery_consumption_rate
- line_official_account_status.liveness_status
- line_official_account_status.webhook_status
- line_official_account_status.ban_at

## 7. 備考
- enum値（使用状況/別LINE誘導/振り分け先/生存状況/webhook）とDB値は要確認
- access_token/secret の表示/マスク方針は要確認
- webhook更新/アクセストークン更新は非同期化の要否を要確認
