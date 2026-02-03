---
name: api-intake-screen-spec
description: 画面単位のAPI要件をフォーム化して埋め、推測を排除するための入力整理スキル。画面/ユースケースの可変要件を固めたいとき、OpenAPI生成の前段に使う。
---

# 画面仕様インテイク（API設計）

## 目的
- 画面/ユースケースの要件をフォーム化して確定度を上げる
- AIの推測や一般論の盛り込みを防ぐ

## 使うとき
- 画面単位APIの要件を整理したいとき
- OpenAPI生成の前に、可変要件を固めたいとき

## 入力
- 画面名/ルート/概要
- 必要な表示データ（UIに必要な項目）
- ユーザー操作（検索/フィルタ/並び替え/ページング/アクション）
- 認証/認可・運用の前提（分かる範囲）

## 進め方
1) 不足情報を短く確認する（推測しない）
2) 分かる範囲だけ埋め、未確定は TODO を残す
3) UIに不要な項目は入れない
4) 出力は必ずテンプレに従う

## 出力フォーマット（必須）
- 以下テンプレを Markdown で埋めて出力する
- TODO は必ず "TODO:" で始める
- 推測で欄を埋めない

```
## Screen
- name:
- route:
- summary:

## Success Criteria
- [ ] 
- [ ] 
- [ ] 

## Display Model (UIに必要なデータのみ)
- fields:
  - name:
    type:
    note:

## User Actions
- list:
  - action:
    request_params:
    response_data:
    errors:

## Data Sources (分かる範囲)
- primary:
- related:

## Constraints
- auth:
- authz:
- performance:
- caching:

## TODO
- TODO:
```

## 禁止事項
- 仕様の推測追加（認証方式/ページング/エラー形式など）
- 画面要件にないフィールドの追加

## 更新日
2026-02-03
