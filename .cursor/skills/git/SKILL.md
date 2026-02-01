---
name: git
description: Gitワークフローの運用指針。
---

# Gitワークフロー

## ブランチの確認
`@Branch` を使用して現在のブランチの変更を確認する。

## コミットとプッシュ
- コミット前に `/test` でテストが通ることを確認する
- コミットメッセージは日本語で明確に書く

## コミット前チェック
1) `/test`
2) `npm run typecheck`
3) `npm run build`（必要に応じて）

## PR作成
- PR作成は `/pr` を使う
