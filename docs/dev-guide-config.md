# dev-guide 設定

## 動作モード
- 部分監査：**毎回確認してから実行**
  - 各モジュール完了時に AskUserQuestion で実行可否を確認する
  - 「実行」が選ばれたら dev-audit-1-partial を呼び出す

## 公開・配布の予定
- **公開する**
- 種別：**Web（静的サイト）**
- デプロイ先：**GitHub Pages**

## 特殊事項
- 本プロジェクトは仕様書なし。既存コード `aoi-midori.html`（46KB・単一HTML）を「実装の正」として扱う。
- 仕様書（`docs/spec.md`）と UI 設計資料（`docs/design.md`）は既存コードからの**観察ベース逆生成**。仕様の補完・推測はしない。
- 目的：既存コードの清書 + GitHub Pages へのデプロイ。
