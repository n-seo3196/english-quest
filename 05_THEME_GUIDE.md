# 05_THEME_GUIDE - 新テーマ追加ガイド

## 概要

新しいテーマを追加するときの手順書。
バトルモードの `index.html` をコピーして、以下の箇所を書き換える。

---

## 手順

### Step 1: フォルダを作る

```
themes/
├── battle/      ← 既存
├── zoo/         ← 新規作成
│   ├── index.html
│   └── README.md
```

### Step 2: HTMLファイルをコピー

`themes/battle/index.html` を `themes/zoo/index.html` にコピーする。

### Step 3: 以下の7箇所を書き換える

---

## 書き換え箇所一覧

### ① タイトルとスタート画面のテキスト

**場所：** HTMLの `<title>` と `#start-screen` 内

```html
<!-- 変更前（バトル） -->
<title>English Quest</title>
<h1>⚔️ English Quest</h1>
<p>冒険の道を進んで正しい答えのゲートをくぐろう！</p>

<!-- 変更後（動物園の例） -->
<title>English Zoo Adventure</title>
<h1>🦁 English Zoo Adventure</h1>
<p>動物園を歩いて正しい答えのゲートをくぐろう！</p>
```

### ② 問題データ（questions配列）

**場所：** `<script>` 内の `var questions = [...]`

テーマの世界観に合った問題文・絵文字に差し替える。
文法内容は同じに保つ。形式は 04_QUESTIONS.md を参照。

```javascript
// バトル版
{ emoji: "🐉", sentence: "The hero ______ the dragon.", ... }

// 動物園版に差し替え
{ emoji: "🦁", sentence: "The lion ______ meat every day.", ... }
```

### ③ タイル描画（道や地面の見た目）

**場所：** `draw()` 関数内のタイル描画部分

各タイルタイプの色を変更する：

```javascript
// バトル版
if (t === 0) { cx.fillStyle = '#3a7a28'; }  // 草原
if (t === 1) { cx.fillStyle = '#c4a86a'; }  // 土の道
if (t === 2) { cx.fillStyle = '#8a7a50'; }  // 縁石

// 動物園版（例）
if (t === 0) { cx.fillStyle = '#4a8a38'; }  // 芝生
if (t === 1) { cx.fillStyle = '#d4c098'; }  // 砂利道
if (t === 2) { cx.fillStyle = '#8B7355'; }  // 木の柵

// 電車版（例）
if (t === 0) { cx.fillStyle = '#555555'; }  // 線路脇の砂利
if (t === 1) { cx.fillStyle = '#999999'; }  // プラットフォーム
if (t === 2) { cx.fillStyle = '#cccccc'; }  // 白線
```

### ④ 装飾の描画

**場所：** `draw()` 関数内、タイル0（草原）の装飾部分

草・花・木の代わりにテーマに合った装飾を描く：

```javascript
// 動物園版（例）：柵、ベンチ、案内板
if (rnd > 0.85) {
  // 柵を描画
  cx.fillStyle = '#8B7355';
  cx.fillRect(sx + 5, sy + 15, 30, 3);
  cx.fillRect(sx + 8, sy + 8, 3, 12);
  cx.fillRect(sx + 28, sy + 8, 3, 12);
}

// 電車版（例）：信号、看板、自販機
if (rnd > 0.88) {
  // 信号を描画
  cx.fillStyle = '#333';
  cx.fillRect(sx + 18, sy + 5, 4, 20);
  cx.fillStyle = rnd > 0.94 ? '#0f0' : '#f00';
  cx.beginPath(); cx.arc(sx + 20, sy + 8, 4, 0, Math.PI * 2); cx.fill();
}
```

### ⑤ プレイヤーキャラの見た目

**場所：** `draw()` 関数内のプレイヤー描画部分

色やアクセサリーを変更する。パーツの構造は同じ：

```javascript
// バトル版：青い服、赤マント、剣
cx.fillStyle = '#2266aa';  // 服の色
cx.fillStyle = 'rgba(170,35,35,0.75)'; // マント

// 動物園版（例）：緑のベスト、帽子、双眼鏡
cx.fillStyle = '#2a8a3a';  // 服の色（緑）
// マントの代わりにリュック
// 剣の代わりに双眼鏡

// 電車版（例）：紺の制服、帽子
cx.fillStyle = '#1a1a6a';  // 服の色（紺）
// 剣の代わりに切符
```

### ⑥ ゲート（看板）のデザイン

**場所：** `draw()` 関数内のゲート描画部分

看板の形・色を変更する：

```javascript
// バトル版：木の看板
bgCol = 'rgba(50,35,15,0.92)';  // 木目調
borderCol = '#b8941e';            // 金枠

// 動物園版（例）：案内板風
bgCol = 'rgba(20,60,20,0.9)';   // 深緑
borderCol = '#4caf50';            // 明るい緑枠

// 電車版（例）：駅名標風
bgCol = 'rgba(255,255,255,0.9)'; // 白
borderCol = '#1565c0';            // 青枠
```

### ⑦ ヒントアイテムの見た目

**場所：** `draw()` 関数内のヒントアイテム描画部分

```javascript
// バトル版：金色の星
cx.fillStyle = '#ffd700';

// 動物園版（例）：足跡マーク
// 星型の代わりに足跡型に描画

// 電車版（例）：切符マーク
// 星型の代わりに四角形 + テキスト「?」
```

---

## 書き換え不要な箇所

以下はテーマが変わっても同じまま使える：

- ゲームの仕組み（ヒントシステム、スコア計算、画面遷移）
- 操作方法（キーボード、D-pad）
- マップ構造（道幅、タイルの並び）
- カメラの動き
- UIパネルのレイアウト（問題パネル、スコアバー）
- パーティクルエフェクト

---

## テーマ別 README.md のテンプレート

各テーマフォルダに入れるREADME：

```markdown
# [テーマ名] モード

## 概要
- 対象：[どんな子向けか]
- 世界観：[テーマの説明]

## ファイル
- index.html：ゲーム本体（ブラウザで直接開く）

## 固有の設定
- 問題数：[X]問
- キャラクター：[説明]
- 道の見た目：[説明]
- ゲートデザイン：[説明]

## 状態
- [✅ 完成 / 🔧 開発中 / 🔲 未着手]
```
