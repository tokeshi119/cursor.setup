---
name: api-generate-openapi
description: Screen Specなどの入力からOpenAPI 3.0.3 YAMLを生成する変換スキル。components集約、examples必須、TODO保持、推測禁止のガードレールで叩き台を作るときに使う。
---

# OpenAPI生成（Screen Spec → OpenAPI 3.0.3）

## 目的
- Screen Spec を OpenAPI 3.0.3 の叩き台に変換する
- 形式を固定し、後段の機械検査に通る素材を作る

## 入力
- Screen Spec（Markdown）
- 命名/エラー/認証/分類のルール（提供されている場合）
- 既存の OpenAPI または共通コンポーネント（ある場合）

## ガードレール
- 推測でフィールド/エンドポイント/エラーを追加しない
- 未確定は description に "TODO:" を残す
- インラインスキーマは禁止（components に集約）
- examples は必須（request/response/error）

## 手順
1) Screen Spec から必要なエンドポイントとモデルを抽出する
2) components に schemas/parameters/responses/securitySchemes を集約する
3) 既存 components があれば再利用し、重複定義を避ける
4) 各 operation に operationId と tags を付与する（ルールが無ければ TODO）
5) すべての request/response/error に examples を付ける
6) 未確定事項は TODO を残し、勝手に決めない

## 出力
- OpenAPI 3.0.3 YAML（単一ファイル）
- 最低限含めるセクション:
  - info
  - paths
  - components (schemas/parameters/responses/securitySchemes)

## 禁止事項
- ルール不在のまま認証/認可/ページング/エラー形式を決める
- 既存の概念名を変えて新規型を増やす（必要なら TODO で提案止まり）
- 例（examples）を省略する

## 更新日
2026-02-03
