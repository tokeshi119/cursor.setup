---
name: api-design-pipeline
description: 画面仕様インテイク→OpenAPI生成→機械検査→レビューの順でAPI設計を進めるオーケストレーションスキル。工程を一気通貫で回したいときに使う。
---

# API設計パイプライン（オーケストレーター）

## 目的
- API設計の工程を順番に進め、抜け漏れと推測を防ぐ
- "AI生成＝採用" を避ける運用を固定する

## 使うとき
- 画面要件からOpenAPIまで一気通貫で進めたいとき
- 生成→検査→レビューの順序を固定したいとき

## 前提
- 出力は叩き台であり、採用は機械検査 + レビューで決める
- 未確定は TODO を残し、勝手に決めない

## 手順
1) 画面仕様を固める（api-intake-screen-spec を適用）
2) OpenAPIを生成する（api-generate-openapi を適用）
3) 機械検査を行う（api-validate-openapi を適用）
4) レビューを行う（api-review-checklist を適用）

## 出力
- Screen Spec（テンプレ埋め）
- OpenAPI 3.0.3 YAML
- 機械検査の結果（errors / warnings と修正方針）
- レビュー・チェックリストと指摘

## 注意
- どの工程も省略しない
- 不足情報は確認し、未確定は TODO で残す

## 更新日
2026-02-03
