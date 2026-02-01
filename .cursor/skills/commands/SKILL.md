---
name: commands
description: カスタムコマンド運用手順。
---

# Custom Commands

## 目的
定義済みコマンドの実行手順を統一する。

## `/pr`
1) `git diff` で変更内容を確認
2) 変更に基づいて明確なコミットメッセージを書く
3) コミットしてプッシュ
4) `gh pr create` でPRを作成
5) PR URLを返す

## `/review`
1) Linter実行
2) よくある問題のチェック
3) 型チェック
4) テスト実行
5) 注意点を要約

## `/test`
1) `git diff` で変更内容を確認
2) `npm run test` を実行
3) 失敗時は分析して修正
4) すべて通るまで反復
