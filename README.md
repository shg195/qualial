# クオリアル ─ Qualial

「緑」と「青」の境目を測る、色相の二分探索ツール。

## 概要

クオリアル（Qualial）は、ユーザー自身の「緑」と「青」の境目となる色相を測定する、ブラウザだけで動く診断ツールです。8 問のテストで提示される色を「緑寄り／青寄り」と判断していくと、二分探索によって境界の色相が絞り込まれていきます。

## デモ

公開URL：https://shg195.github.io/qualial/

## 主な機能

- 8 問の色相判定テスト（HSL 155°〜210° 間を二分探索）
- ドラッグ／クリック／キーボード（← → / G B）操作対応
- 結果の境界色・色相数値・中間色サンプル表示
- 結果テキストのクリップボードコピー
- 結果カード画像（810×1080 PNG）のダウンロード
- X（Twitter）への共有

## 使い方

1. 「は じ め る」を押して診断を開始
2. 表示される色を、「緑」と感じたら**左**へ、「青」と感じたら**右**へカードをドラッグ
3. 8 問終了後、あなたの「緑」と「青」の境目となる色相が表示される
4. 必要に応じて「もう一度」「結果をコピー」「画像を保存」「Xで共有」を利用

## 動作環境

モダンブラウザ（Chrome / Firefox / Safari / Edge の最新版）。スマートフォンにも対応。

## ローカルでの起動

依存関係・ビルド処理なし。`index.html` をブラウザで直接開くだけで動作します。

```bash
# macOS
open index.html

# Linux
xdg-open index.html

# Windows
start index.html
```

## ディレクトリ構成

```
.
├── index.html              # 単一HTML（CSS/JS 全部入り）
├── favicon.svg             # ブラウザタブのアイコン
├── README.md               # このファイル
├── LICENSE                 # MIT ライセンス
├── .gitignore
└── docs/
    ├── spec.md             # 仕様（観察ベース逆生成）
    ├── design.md           # UI 設計メモ
    ├── conventions.md      # 規約・現状コードインベントリ
    ├── dev-guide-config.md # 開発ガイド設定
    └── mvp-scope.md        # MVP スコープ
```

## デプロイ

GitHub Pages を想定しています。リポジトリ設定の「Settings → Pages」から `main` ブランチをソースに指定すると、`https://{user}.github.io/{repo}/` で公開されます。

## 技術スタック

- 素の HTML / CSS / JavaScript（ビルド処理・パッケージ依存なし）
- 外部 CDN：Google Fonts（Shippori Mincho B1 / Noto Sans JP / Cormorant Garamond）

## ライセンス

[MIT License](LICENSE)
