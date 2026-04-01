# 03_CODE_STRUCTURE - コード構造と技術仕様

## 技術スタック

| 項目 | 内容 |
|------|------|
| 言語 | HTML5 + CSS + JavaScript（ES5互換） |
| 描画 | Canvas 2D API |
| ファイル形式 | 単一HTMLファイル |
| 外部依存 | なし |
| 対応環境 | モダンブラウザ（Chrome, Safari, Firefox, Edge） |
| サーバー | 不要（ファイルを直接開くだけで動く） |

### なぜES5互換で書いているか

- `let` / `const` / アロー関数 (`=>`) は使わず `var` / `function` で統一
- 一部の古い環境や組み込みWebViewでも動作するようにするため
- Canva Sites への埋め込みも考慮

---

## HTMLファイルの構造

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="...">  ← スマホ対応
  <title>English Quest</title>
  <style>
    /* ===== 全てのCSS ===== */
  </style>
</head>
<body>
  <canvas id="c"></canvas>              ← ゲーム描画領域
  
  <div id="ui">                         ← UI層（Canvas の上に重ねて表示）
    <div id="score-bar">...</div>       ← スコア表示
    <div id="feedback">...</div>        ← 正解/不正解テキスト
    <div id="dpad">...</div>            ← スマホ用十字キー
  </div>
  
  <div id="quiz-box">...</div>          ← 問題文パネル
  
  <div id="start-screen">...</div>      ← スタート画面（オーバーレイ）
  <div id="game-over">...</div>         ← 完了画面（オーバーレイ）
  
  <script>
    /* ===== 全てのJavaScript ===== */
  </script>
</body>
</html>
```

---

## JavaScript の構造（上から順に）

### 1. 初期設定と定数

```javascript
var cv, cx;        // Canvas とコンテキスト
var W, H;          // 画面サイズ
var TILE = 40;     // 1タイルのピクセルサイズ
var ROAD_LEFT = 4; // 道路の左端 col
var ROAD_RIGHT = 16; // 道路の右端 col
var MAP_W = 21;    // マップの横幅（タイル数）
```

### 2. 問題データ（questions配列）

```javascript
var questions = [
  {
    emoji: "🐉",
    sentence: "The hero ______ the dragon.",
    answer: "attacks",
    choices: ["attack", "attacks", "attacked"],
    hints: ["ヒント1", "ヒント2", "ヒント3"]
  },
  // ... 以下同じ形式
];
```

### 3. ゲーム状態変数

```javascript
var player = { col, worldY, px, py, dir, frame };
var camY;          // カメラの Y 位置
var score, combo, stage;
var currentQ;      // 現在の問題オブジェクト
var hintsOpened;   // 解放済みヒント数
var answered;      // 回答済みフラグ
var gameState;     // 'title' | 'playing' | 'gameover'
var hintItems;     // ヒントアイテム配列
var gateObj;       // ゲート（看板）オブジェクト
var particles;     // パーティクル効果配列
var keysDown;      // 押下中のキー
var lastMoveTime;  // 最後に移動した時刻
```

### 4. ユーティリティ関数

| 関数 | 役割 |
|------|------|
| `resize()` | 画面サイズ変更時にCanvasをリサイズ |
| `seededRand(x, y)` | 座標ベースの決定的乱数（装飾用） |
| `getTile(col, row)` | 指定座標のタイル種類を返す |
| `shuffleArray(arr)` | 配列をランダムにシャッフル |
| `drawRoundRect(x,y,w,h,r)` | 角丸四角形のパスを描く |
| `addP(wx, wy, col, n)` | パーティクルを生成 |

### 5. ゲーム進行関数

| 関数 | 役割 |
|------|------|
| `startGame()` | ゲーム開始（スタート画面を閉じる） |
| `startStage()` | ステージ初期化（マップ・アイテム・ゲート配置） |
| `showQuiz()` | 問題パネルの表示を更新 |
| `updateUI()` | スコア・コンボの表示を更新 |
| `showFB(txt, col)` | フィードバックテキストを一瞬表示 |
| `tryMove(dir)` | 指定方向に移動を試みる |
| `checkTile()` | 現在位置でアイテム・ゲートとの接触判定 |
| `nextStageOrEnd()` | 次のステージへ進む or ゲーム完了 |

### 6. 入力ハンドリング

```javascript
// キーボード
window.addEventListener('keydown', function(e) { ... });
window.addEventListener('keyup', function(e) { ... });

// スマホ D-pad（各ボタンに touchstart / touchend）
dpadBtns[i].addEventListener('touchstart', function(e) { ... });

// キー長押しの連続移動処理
function handleKeys() { ... }  // 100ms間隔で移動
```

### 7. ゲームループ

```javascript
function update() {
  handleKeys();                // 入力処理
  // プレイヤー位置のスムーズ補間
  // カメラ追従
  // パーティクル更新
}

function draw() {
  // 1. 背景クリア
  // 2. タイル描画（見える範囲のみ）
  // 3. ヒントアイテム描画
  // 4. ゲート（看板）描画
  // 5. プレイヤーキャラ描画
  // 6. パーティクル描画
  // 7. 方向ヒント表示
}

function loop() { update(); draw(); requestAnimationFrame(loop); }
loop();
```

---

## 座標系

### ワールド座標

- `col`：横位置（タイル単位、0〜20）
- `worldY`：縦位置（タイル単位、0がスタート、負の値が前方）
- ピクセル変換：`px = col * TILE + TILE/2`, `py = worldY * TILE + TILE/2`

### スクリーン座標

- `screenX = worldPixelX - camOX` （camOX = player.px - W/2）
- `screenY = worldPixelY - camOY` （camOY = camY - H*0.55）

---

## テーマ別にカスタマイズする箇所

新テーマを作るとき、以下の部分を差し替える：

| 箇所 | 変更内容 |
|------|----------|
| `questions` 配列 | テーマに合った問題文・絵文字 |
| タイル描画部分 | 草原→水辺、土の道→ホーム等 |
| プレイヤー描画部分 | キャラの見た目 |
| ゲート描画部分 | 看板のデザイン |
| ヒントアイテム描画 | アイテムの見た目 |
| 配色定数 | テーマに合った色 |
| スタート画面のテキスト | テーマに合った説明文 |

詳しくは `05_THEME_GUIDE.md` を参照。
