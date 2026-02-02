# 画面仕様: 広告コード一覧/検索/新規/編集

Last updated: 2026-02-02

## 1. 基本情報
- 画面ID: advertisement-codes
- 画面名: 広告コード一覧/検索/新規/編集
- 目的/ゴール: 広告コードの登録・検索・更新・削除を行い、ポストバック設定を管理する
- 対象ユーザー/権限: 管理者
- 添付スクショ/モック: 共有済み画像（2026-02-02, 一覧/検索/新規/編集）

## 2. 画面タイプ
- 一覧: yes
- 詳細: no
- 作成: yes
- 編集: yes
- 削除: yes（物理削除）

## 3. 必要データ（最小）
- 参照テーブル:
  - advertisement_codes
  - advertisement_code_custom_settings（custom時）
- 主キー: advertisement_codes.id
- 表示項目:
  - 管理ID（advertisement_codes.id）
  - 広告コード（advertisement_codes.advertisement_code）
  - コンバージョンコード（advertisement_codes.conversion_code, 自動生成）
  - 広告名（advertisement_codes.name）
  - ポストバック設定（advertisement_codes.postback_setting）
  - 友だち追加URL（自動生成, 要確認）
  - メモ（advertisement_codes.memo）
  - 操作（編集/削除）
- 編集項目:
  - 広告コード（必須, 30文字まで）
  - 広告名（必須, 30文字まで）
  - ポストバック設定（なし/custom/google/meta/tiktok/line）
  - custom設定（postback URL, パラメータ名 最大3つ, 友だち管理ID用パラメータ名）
  - メモ（1000文字まで）
- 既定値/初期値:
  - 1ページ100件
  - 並び順: 作成日時の降順
  - conversion_code: LCO + 7桁（10文字）で自動生成

## 4. 検索・フィルタ・ソート・ページング
- 検索条件:
  - 管理ID: 完全一致（複数可, NOT可）
  - 広告コード: 部分一致（複数可, NOT可）
  - 広告名: 部分一致
  - ポストバック設定: 複数選択
  - メモ: 部分一致
- ソート条件: 作成日時の降順固定
- ページング: page/limit（default=100, max=100）

## 5. 主要操作（ボタン/アクション）
- 検索実行
- 新規登録
- 編集
- 削除（物理削除）

## 6. バリデーション/制約
- 広告コード: 半角英数（30文字まで）※詳細は要確認
- 広告名: 30文字まで
- conversion_code: LCO + 7桁（10文字）
- customのパラメータ名: 最大3つ（許可文字は要確認）
- 置換タグ:
  - {+afn+}, {+afn2+}（LPRO互換）
  - 友だち管理IDは {+line_friend_id+} を使用

## 7. APIメモ（暫定）
- エンドポイント候補:
  - GET /advertisement-codes
  - POST /advertisement-codes
  - PUT /advertisement-codes/{advertisement_code_id}
  - DELETE /advertisement-codes/{advertisement_code_id}
- CAPIコードは自動生成
- 手動ポストバックは不要（自動実行のみ）

## 8. 備考
- conversion_code の再生成ルールは要確認
- 友だち追加URLの生成/保存ルールは要確認
- ポストバック実行タイミングは要確認
- 削除時の連鎖（custom_settings / postback_history）は要確認
