# 画面仕様: ポストバック履歴

Last updated: 2026-02-02

## 1. 基本情報
- 画面ID: postback-history
- 画面名: ポストバック履歴
- 目的/ゴール: 広告コード単位のポストバック実行履歴を検索・確認する
- 対象ユーザー/権限: 管理者
- 添付スクショ/モック: 共有済み画像（2026-02-02, ポストバック履歴）

## 2. 画面タイプ
- 一覧: yes
- 詳細: no
- 作成: no
- 編集: no
- 削除: no

## 3. 必要データ（最小）
- 参照テーブル: postback_history, advertisement_codes, line_friends
- 主キー: postback_history.id
- 表示項目:
  - 実行日時（postback_history.created_at）
  - 広告コード（advertisement_codes.advertisement_code）
  - 友だち管理ID（line_friends.id）
  - ポストバック種別（advertisement_codes.postback_setting）
  - 実行URL（postback_history.execution_url）

## 4. 検索・フィルタ・ソート・ページング
- 検索条件:
  - 広告コード: 部分一致（単一）
  - コンバージョンコード: 部分一致（単一）
  - 友だち管理ID: 完全一致（単一）
  - 実行URL: 部分一致
  - 実行日: from/to 範囲（JST）
- ソート条件: 実行日時の降順固定
- ページング: page/limit（default=100, max=100）

## 5. 主要操作（ボタン/アクション）
- 検索実行
- ページ移動（100件/ページ）

## 6. バリデーション/制約
- 実行日: from/to はISO 8601（JST）
- 検索は単一指定（複数入力なし）

## 7. APIメモ（暫定）
- エンドポイント候補: GET /postback-history

## 8. 備考
- 参照のみ（削除/再送はなし）
