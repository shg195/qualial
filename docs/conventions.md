# conventions.md（共通規約・現状コードインベントリ）

> 既存コード `index.html`（元 `aoi-midori.html`）から抽出した規約と現状コードの構造一覧。
> 以降のモジュールはここに記載した規約に従う（参照しない実装は監査で要修正扱い）。

---

## 1. ファイル構成（現状）
- `index.html`（46KB・1505行）：HTML / CSS / JS をすべてインラインで含む単一ファイル
- 外部依存：Google Fonts のみ（CDN）

## 2. 命名規約

### HTML / CSS（クラス名）
- **BEM 風記法**：`block__element--modifier`
- 状態クラスは `is-` プレフィックス（`.is-open`, `.is-target`, `.is-glow`, `.is-closing`, `.is-green`, `.is-blue`, `.is-boundary`）
- 例：
  - `.intro`, `.intro__brand`, `.intro__brand-jp`
  - `.card`, `.card__color`, `.card__corner--tl`
  - `.reference--green`, `.reference--blue`
  - `.modal__panel`, `.modal__close`
  - `.result__main`, `.result__samples-axis`

### HTML（id 属性）
- **camelCase**（JS から参照する要素）
- 例：`startBtn`, `helpModal`, `cardColor`, `demoRefGreen`, `resultSwatch`

### CSS 変数
- ケバブケース、`--` プレフィックス（CSS 仕様）
- 種別プレフィックスでグループ化：
  - 背景：`--bg`, `--bg-deep`, `--paper`
  - インク（テキスト）：`--ink`, `--ink-soft`, `--ink-faint`
  - 罫線：`--line`
  - アクセント：`--accent`
  - 参照色：`--ref-green`, `--ref-blue`
  - 動的設定値：`--marker-color`（JS から `setProperty` で注入）
  - フォールバック専用：`--user-color`（CSS 側で `var(--user-color, hsl(180, 78%, 50%))` のフォールバック値として参照。JS は `setProperty` せず `style.background` を直接設定する）

### JavaScript
- 変数・関数：**camelCase**（`currentHue`, `pickNextHue`, `onPointerDown`）
- 定数（不変の設定値）：**UPPER_SNAKE_CASE**（`HUE_MIN`, `COMMIT_THRESHOLD`, `SAMPLE_OFFSETS`）
- DOM 参照変数：要素種別を末尾に付けるパターンあり（`cardEl`, `markerEl`, `swatchEl`, `samplesEl`）／単純名のみのパターンもあり（`intro`, `test`, `result`, `startBtn`）

## 3. コメント規約
- **セクション区切りコメント**を一貫して使用
  - CSS：`/* ─────────── セクション名 ─────────── */`（罫線文字 `─` で囲む）
  - JS：`// ─────────── セクション名 ───────────`
  - HTML：`<!-- ─── セクション名 ─── -->`
- インラインコメントは「なぜ」を記述（例：`// 判定確定までのドラッグ距離(px)`、`// ラバーバンド：閾値内は1:1、超過分は強く減衰`）
- 自明な「何を」のコメントは書かない

## 4. 言語・文字コード
- HTML：`<html lang="ja">`、`<meta charset="UTF-8">`
- 表示テキスト：日本語（一部欧文：「Qualial」「←」「→」など）
- ファイル文字コード：UTF-8（BOM なし）

## 5. CSS スタイル方針

### 単位
- 余白・サイズ：基本 `rem`、細部のみ `px`
- レスポンシブな文字サイズ：`clamp(min, vw, max)` を使用
- ブレークポイント：`max-width: 720px`、`max-height: 640px`

### レイアウト
- フレックス／グリッド併用
- `position: fixed; inset: 0;` で全画面化（テスト・モーダル）
- z-index 階層：背景レイヤー 0–1、ステージ 2、カードエリア 3、ヘルプアイコン 5、テスト画面 10–11、モーダル 100

### 装飾パターン
- **二重枠**：外側パネル + `::before` 内側ボーダー（5〜8px インセット）
- **ノイズテクスチャ**：`body::after` に SVG fractalNoise（インラインデータURI）
- **トランジション・カーブ**：`cubic-bezier(0.34, 1.56, 0.64, 1)`（オーバーシュート）が標準

### アニメーション
- `@keyframes`：`fadeUp`, `pulse`, `modalFade`, `modalSlide`, `modalFadeOut`, `modalSlideOut`
- transition の標準 duration：0.3〜0.5s

## 6. JavaScript スタイル方針

### 言語・記法
- **モジュール化なし**：単一の `<script>` ブロック（IIFE 包装も無し）
- `var` は使用しない（`const` / `let`）
- アロー関数と `function` 宣言の併用
  - 関数宣言：トップレベルの主要関数（`function pickNextHue() {}` 等）
  - アロー：`addEventListener` のコールバック等
- 非同期：`async/await` + `Promise` ベースの `sleep` ヘルパー（デモ部分のみ）

### DOM 取得
- 起動時にトップレベルでまとめて `getElementById` する（一部関数内で個別取得もあり）

### イベント登録
- `addEventListener` を使用
- ポインタイベント（`pointerdown` / `pointermove` / `pointerup` / `pointercancel`）でドラッグを統一
- `setPointerCapture` でドラッグ追従

### キーボード操作
- `keydown` イベントで `e.key` を判定
- 矢印キーと文字キーの両方をサポート（Arrow / g / b など）

## 7. アクセシビリティ規約
- アイコンボタン：`aria-label` と `title` を併設
- モーダル：`role="dialog"`、`aria-labelledby`、`aria-hidden`（開閉で切替）
- 装飾要素：`aria-hidden="true"`（イントロのデモなど）

## 8. import / export 方針
- 現状：**なし**（ES Modules 不使用、外部 JS なし、CDN は Google Fonts のみ）
- 今後の方針：単一HTML を維持するため、import/export は導入しない

## 9. エラーハンドリング基本方針
- `try/catch`：`releasePointerCapture` のみ（無視可能なケース）
- クリップボード API：`navigator.clipboard` の存在チェックを行ってから呼ぶ
- 異常系の積極的なハンドリング・通知は行わない（UIのみのツールで副作用が極めて小さいため）

---

## 10. 共通定数インベントリ（JS）

| 名前 | 値 | 種別 | 役割 |
|---|---|---|---|
| `TOTAL_QUESTIONS` | `8` | 出題 | 総問題数 |
| `HUE_MIN` | `155` | 色相範囲 | 探索下限（緑寄り） |
| `HUE_MAX` | `210` | 色相範囲 | 探索上限（青寄り） |
| `SATURATION` | `78` | 色 | HSL 彩度（%） |
| `LIGHTNESS` | `50` | 色 | HSL 明度（%） |
| `COMMIT_THRESHOLD` | `130` | ドラッグ | 判定確定までの距離 (px) |
| `TILT_FACTOR` | `0.18` | ドラッグ | 回転角 (度/px) |
| `MAX_TILT` | `24` | ドラッグ | 最大回転角 (度) |
| `RUBBER_DAMP` | `0.28` | ドラッグ | 閾値超過時の減衰係数 |
| `MAX_OVERFLOW` | `50` | ドラッグ | 閾値超過の最大量 (px) |
| `SAMPLE_OFFSETS` | `[-15, -7, 0, 7, 15]` | 結果 | 中間色サンプルの色相オフセット |

## 11. 状態変数インベントリ（JS）

| 名前 | 初期値 | 役割 |
|---|---|---|
| `currentQ` | `0` | 現在の出題番号（0始まり） |
| `lowBound` | `HUE_MIN` | 探索範囲の下限（投票で更新） |
| `highBound` | `HUE_MAX` | 探索範囲の上限（投票で更新） |
| `currentHue` | `(HUE_MIN + HUE_MAX) / 2` | 現在出題中の色相 |
| `testActive` | `false` | 入力受付中かどうか |
| `dragging` | `false` | ドラッグ中かどうか |
| `startX` | `0` | ドラッグ開始 X 座標 |
| `dx` | `0` | ドラッグ移動量 |
| `pointerId` | `null` | 現ポインタID（capture 用） |
| `demoActive` | `false` | LP デモのループ稼働フラグ |

## 12. DOM 参照インベントリ

### グローバル参照（`getElementById`）
- 画面：`intro`, `test`, `result`
- ボタン：`startBtn`, `restartBtn`, `testResetBtn`, `shareBtn`
- カード関連：`cardEl`, `cardColor`
- 進捗：`progress`
- 参照色：`refGreen`, `refBlue`
- LPデモ：`demoCard`, `demoCardColor`, `demoRefGreen`, `demoRefBlue`
- モーダル：`helpBtn`, `helpModal`, `modalBackdrop`, `modalClose`

### 関数内取得
- `finishTest()` 内：`marker`, `resultSwatch`, `resultHue`, `resultSamples`
- イベント登録時：`testHelpBtn`

## 13. 主要関数インベントリ

| 関数 | 種別 | 責務 |
|---|---|---|
| `buildProgressDots` | 進捗 | 進捗ドット要素を構築 |
| `updateProgressDots` | 進捗 | done/current クラス更新 |
| `pickNextHue` | 出題 | 次の出題色相を決定（中点±ジッター） |
| `showColor` | 出題 | カードに色を反映 |
| `clearRefHighlights` | UI | 参照色のターゲット強調を解除 |
| `vote` | 投票 | 緑/青の投票処理＋探索範囲更新 |
| `applyRubberBand` | ドラッグ | 閾値超過時のラバーバンド減衰 |
| `onPointerDown/Move/Up` | ドラッグ | ポインタイベントハンドラ |
| `finishTest` | 結果 | 結果計算と結果画面表示 |
| `goToIntro` | 画面遷移 | LP に戻す（状態リセット） |
| `beginTest` | 画面遷移 | テスト開始 |
| `openHelp/closeHelp` | モーダル | 使い方モーダル開閉 |
| `randomDemoHue` | デモ | デモ用ランダム色相 |
| `setDemoColor` | デモ | デモカードに色を反映 |
| `sleep` | デモ | Promise ベース待機 |
| `runDemo` | デモ | デモループ本体（async） |
| `startDemo/stopDemo` | デモ | デモ起動・停止 |

---

## 14. 今後のモジュールが従う規約まとめ

1. 命名：HTML/CSS は BEM 風、JS は camelCase（定数は UPPER_SNAKE_CASE）
2. ID は camelCase、状態クラスは `is-` プレフィックス
3. セクション区切りは `─` 文字を含むコメントで統一
4. CSS 変数で色・値を一元管理
5. レスポンシブは `max-width: 720px` と `max-height: 640px` を標準ブレークポイントに
6. アクセシビリティ属性（aria-*, role）を新規要素にも付与
7. JS は単一 `<script>` ブロック内に追記する（モジュール化しない）
8. 既存コードを最小差分で変更し、不要な再整形は行わない
