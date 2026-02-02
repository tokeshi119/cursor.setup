# 画面仕様: 友だち一覧/検索

Last updated: 2026-02-02

## 1. 基本情報
- 画面ID: friends
- 画面名: 友だち一覧/検索
- 目的/ゴール: 友だち（ユーザー）を検索・一覧表示し、BAN（ブロック）状態を確認できるようにする
- 対象ユーザー/権限: 管理者
- 添付スクショ/モック: 共有済み画像（2026-02-02, 一覧/検索）

## 2. 画面タイプ
- 一覧: yes
- 詳細: no
- 作成: no
- 編集: no
- 削除: yes（物理削除）

## 3. 必要データ（最小）
- 参照テーブル:
  - line_friends
  - advertisement_codes（広告コード表示/検索）
  - line_official_accounts（LINE連携表示/検索）
- 主キー: line_friends.id
- 表示項目:
  - 管理ID（line_friends.id）
  - 管理コード（incode）※要確認
  - 広告コード（advertisement_codes.advertisement_code）
  - LINE_ID（line_friends.messaging_api_user_id）
  - 友だち登録日時（line_friends.created_at）
  - 友だち状態（line_friends.is_blocked）
  - LINE連携（line_official_accounts.basic_id）
  - 操作（削除）
- 編集項目: なし
- 既定値/初期値:
  - 1ページ100件
  - 並び順: 友だち登録日時の降順

## 4. 検索・フィルタ・ソート・ページング
- 検索条件:
  - 管理ID: 完全一致（複数可、除外チェックでNOT）
  - 管理コード（incode）: 部分一致（複数可、除外チェックでNOT）※要確認
  - 広告コード: 部分一致（複数可、除外チェックでNOT）
  - LINE_ID: 部分一致
  - 友だち登録日時: 範囲指定（JST）
  - 友だち状態: 正常 / ブロック
  - LINE連携: basic_id 複数選択（除外チェックでNOT）
- フィルタ条件: 上記検索条件に同じ
- ソート条件: 登録日時の降順固定
- ページング: page/limit（default=100, max=100）

## 5. 主要操作（ボタン/アクション）
- 検索実行
- ページ移動（100件/ページ）
- 友だち削除（物理削除）

## 6. バリデーション/制約
- 管理ID: 数値のみ
- 管理コード（incode）: 対応カラム要確認
- 広告コード/LINE_ID: 文字列
- 登録日時: ISO 8601（JST）
- BANアカウントは一覧上でグレー表示

## 7. APIメモ（暫定）
- エンドポイント候補: GET /friends, DELETE /friends/{id}
- LINE連携選択肢: GET /line-official-accounts?fields=basic_id

## 8. 備考
- 友だち詳細ページは作成しない
- LINE名は表示しない
