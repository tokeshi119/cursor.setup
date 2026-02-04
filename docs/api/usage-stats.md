# API設計: 利用集計（月別/日別/時間別）

Last updated: 2026-02-04

## 1. 画面要件分析: 利用集計（月別/日別/時間別）

### 表示データ
- label（string）: 年月 / 日 / 時間の表示ラベル
- period_start（date）: 集計対象期間の開始日
- friend_registrations（integer）: 新規友だち追加数
- push_deliveries（integer）: push配信数
- reply_deliveries（integer）: reply配信数
- total_deliveries（integer）: push + reply の合計
- summary（object）: 期間合計（画面の「合計」行）

### ユーザー操作
- 前年/翌年（月別）
- 前月/翌月（日別）
- 前日/翌日（時間別）
- 粒度切替（月別/日別/時間別）
- ドリルダウン（月別 → 日別 → 時間別）

### 関連画面
- 利用集計（日別/時間別）: 月別から遷移

## 2. エンドポイント一覧

| メソッド | パス | 用途 |
|---------|------|------|
| GET | /api/v1/stats/usage | 利用集計（月別/日別/時間別）取得 |

## 3. リクエスト仕様

### クエリパラメータ

granularity:
  type: string
  required: true
  enum: [month, day, hour]
  description: 集計粒度
  example: "month"

year:
  type: integer
  required: conditionally
  description: 集計対象年（granularity=month のとき必須）
  example: 2026

month:
  type: string
  required: conditionally
  description: 集計対象月（granularity=day のとき必須）
  example: "2026-01"

date:
  type: string
  required: conditionally
  description: 集計対象日（granularity=hour のとき必須）
  example: "2026-01-01"

### バックエンド確認事項
- 0件の期間も必ず返却する（12ヶ月/日数/24時間）
- JST固定で集計する
- 課金対象は push / reply のみ（greetは除外）
- stats_monthly / stats_daily / stats_hourly の参照方針

## 4. レスポンス構造サンプル

```json
{
  "granularity": "month",
  "time_zone": "Asia/Tokyo",
  "range": {
    "from": "2026-01-01",
    "to": "2026-12-31"
  },
  "summary": {
    "friend_registrations": 120,
    "push_deliveries": 300,
    "reply_deliveries": 50,
    "total_deliveries": 350
  },
  "items": [
    {
      "period_start": "2026-01-01",
      "label": "2026-01",
      "friend_registrations": 10,
      "push_deliveries": 25,
      "reply_deliveries": 5,
      "total_deliveries": 30
    },
    {
      "period_start": "2026-02-01",
      "label": "2026-02",
      "friend_registrations": 0,
      "push_deliveries": 0,
      "reply_deliveries": 0,
      "total_deliveries": 0
    }
  ]
}
```

> NOTE: items は期間内の全件を返す（0件も含む）

## 5. 共通仕様
- 命名規則・型・エラーレスポンス: `docs/api/conventions.md` を参照
- ベースパス: `/api/v1`
- 認証: Bearer Token（仮置き、詳細はバックエンドと調整）

## 6. DDL対応
- 集計元:
  - stats_monthly（年月）
  - stats_daily（日）
  - stats_hourly（時間）
- total_deliveries = push_deliveries + reply_deliveries
