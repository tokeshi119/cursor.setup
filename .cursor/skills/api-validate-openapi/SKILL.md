---
name: api-validate-openapi
description: OpenAPIの構文/参照整合/ルール遵守を機械検査するスキル。SpectralやRedocly等のツールが使えるとき、AI出力を未信頼として検査する場面で使う。
---

# OpenAPI機械検査

## 目的
- AI生成のOpenAPIを機械的に検査し、レビュー前に欠陥を落とす
- 命名/例/参照整合/破壊的変更を検出する

## 使うとき
- OpenAPIを生成した直後
- 既存仕様との差分を安全に確認したいとき

## 入力
- OpenAPIファイル（.yaml/.json）
- 既存のOpenAPI（破壊的変更検出を行う場合）
- ルールセット（Spectralなどがある場合）

## 手順
1) 構文検証を行う（ツールがある場合のみ）
2) Lint を実行し、examples/命名/operationId 等の違反を検出する
3) 既存仕様がある場合は破壊的変更を検出する
4) エラー/警告を一覧化し、修正方針を添える

## コマンド例（環境にある場合のみ）
- redocly: `redocly lint <openapi.yaml>`
- spectral: `spectral lint <openapi.yaml>`
- oasdiff: `oasdiff --fail-on ERRORS <old.yaml> <new.yaml>`

## 修正方針（代表例）
- スキーマ重複 → components に統合する
- example不足 → request/response/error に examples を追加する
- 型の揺れ → 共通型（ID/日時/カーソル等）に寄せる
- operationId未設定 → ルールに沿って付与する（不明なら TODO）

## 出力
- 検査結果（errors / warnings）
- 重要な修正ポイントの短いメモ

## 注意
- ツールが無い場合は無理に導入しない（追加の依存は増やさない）
- 検査結果が0でも、人間レビューは省略しない

## 更新日
2026-02-03
