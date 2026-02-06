# AGENTS.md

## 目的
このプロジェクトでは `C:\Users\渡慶次裕太\AGENTS.md` を共通基盤として適用し、
本ファイルにはプロジェクト固有の差分のみを記載する。

## 適用範囲
- `C:\Users\渡慶次裕太\dev\LINE-connecter2\Skills` 配下

## プロジェクト固有運用
- 返答は日本語で行う（ユーザーが別言語を明示した場合のみ切り替え）
- 変更前に影響範囲を短く共有する
- API仕様に関わる変更は、関連する `SKILL.md` と `skills.md` の整合を取る

## プロジェクトコマンド
- 実行コマンドは固定化していないため、必要なコマンドは実行前に確認する

## Skill運用
- Skill一覧: `skills.md`
- Skill本体: `.cursor/skills/*/SKILL.md`
- 追加・変更時は `skills.md` を同時更新する

## このプロジェクトで使う主なSkill
- `api-design-pipeline`: API設計全体の進行
- `api-intake-screen-spec`: 画面仕様の要件整理
- `api-generate-openapi`: OpenAPI生成
- `api-validate-openapi`: OpenAPI検証
- `api-review-checklist`: APIレビュー
- `api-openapi-maintenance`: 既存OpenAPIの最小修正

## 更新
- Last updated: 2026-02-06
