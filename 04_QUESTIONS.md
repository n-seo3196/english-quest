# 04_QUESTIONS - 問題データ一覧と追加ルール

## 問題データの形式

全テーマ共通のフォーマット。JavaScriptオブジェクトとして記述する。

```javascript
{
  emoji: "🐉",                              // ステージのシンボル絵文字
  sentence: "The hero ______ the dragon.",   // 穴埋め文（______ が空欄）
  answer: "attacks",                         // 正解の英語文字列
  japanese: "勇者はドラゴンを______。",       // 日本語訳（______ は空欄のまま）
  j_answer: "攻撃する",                      // 正解の日本語訳（正解時にブランクに表示）
  choices: ["attack", "attacks", "attacked"],// 選択肢（3つ。ゲートに表示）
  hints: [                                   // ヒント（最大3つ）
    "ヒント1：基本ルール",
    "ヒント2：この問題への当てはめ",
    "ヒント3：ほぼ答えに直結"
  ]
}
```

---

## 問題文の表示ルール（全テーマ共通）

### クイズボックスの表示

```
🐉 The hero ______ the dragon.
　 勇者はドラゴンを______。
```

- 英語の問題文を上、日本語訳を下に表示する
- 両方とも空欄は `______` のまま表示する
- ヒントのスロット表示（ヒント1・ヒント2・ヒント3）はクイズボックスには出さない

### 正解時の表示

```
🐉 The hero attacks the dragon.     ← 英語：正解単語が緑色
　 勇者はドラゴンを攻撃する。         ← 日本語：j_answer が緑色
```

- 英語の `______` → `answer`（緑・太字）
- 日本語の `______` → `j_answer`（緑）
- 英語と日本語で **別の単語を使う**（英語はそのまま、日本語は自然な訳語）

### j_answer の書き方ルール

- 辞書形（基本形）で書く：「攻撃する」「使う」「倒した」など
- 文脈に合った自然な日本語にする
- 動詞は活用済みでも可（例：過去の文なら「使った」「倒した」）

---

## 問題作成ルール

### 選択肢（choices）

- 必ず3つ（ゲートが3つのため）
- 正解を1つ含む
- 残り2つは「ありがちな間違い」にする（適当な単語ではなく）
- 表示順はゲーム側でシャッフルされるので、配列の順番は気にしなくてOK

### ヒント（hints）

- 最大3つ。1〜3つの範囲で設定可能
- 段階的に詳しくなるように並べる：
  1. 一般的な文法ルール（例：「三人称単数は動詞にsがつく」）
  2. この問題への当てはめ（例：「The hero = 三人称単数」）
  3. ほぼ答え（例：「現在の文なので現在形を使う」）

### 空欄（______）

- 文中に `______` を1箇所入れる
- 6個のアンダースコアで統一

### 絵文字（emoji）

- ステージの雰囲気が伝わるものを選ぶ
- テーマに合わせた絵文字にする

---

## 語彙選択ガイドライン

対象：小学校高学年〜中学生（英語学習初期〜中期）

### 基本方針

- **答えの動詞は「聞いたことがある」レベルを優先する**
- 「知らなくても意味は想像できる」ではなく「絶対聞いたことがある」単語を選ぶ
- ゲームのテーマ（バトル、動物など）に合った文脈で使われていれば、なじみ感が増す

### 使いやすい動詞リスト（優先度高）

| 動詞 | 活用の特徴 | 備考 |
|------|-----------|------|
| use | use / uses / used | 規則変化。汎用性高い |
| attack | attack / attacks / attacked | バトルテーマの定番 |
| play | play / plays / played | 学校英語の最頻出 |
| watch | watch / watches / watched | 三単現で es がつく練習に使える |
| help | help / helps / helped | 規則変化。自然な文が作りやすい |
| clear | clear / clears / cleared | ゲームの「クリア」として子どもに馴染み深い |
| find | find / finds / found | 不規則変化。過去形の練習に使える |
| win | win / wins / won | 不規則変化。ゲームの文脈で自然 |
| run | run / runs / ran | 不規則変化。シンプルで覚えやすい |
| eat | eat / eats / ate | 不規則変化。汎用性高い |

### 避けるべき語彙

- `defeat`（なじみが薄い。「倒す」の英語として定着していない）
- `equip`（ゲーム用語として知っていても活用形が難しい）
- 動詞自体の意味がわからないと文法問題に集中できなくなるもの全般

### 判断基準

> その単語を中学1年生が聞いたとき、意味がわかるか？

わからなければ、文法ではなく語彙の問題になってしまうので変更する。

---

## 文法カテゴリ

現在カバーしている（または予定の）文法テーマ：

| カテゴリ | 説明 | 例 |
|----------|------|-----|
| 現在形 | 主語が I/you/we/they の一般動詞 | I play / We run |
| 三人称単数現在 | he/she/it + 動詞s | He plays / She runs |
| 過去形 | 規則・不規則動詞の過去形 | I played / She ate |
| 現在進行形 | am/is/are + 動詞ing | I am playing / They are running |
| 未来形（予定） | will + 動詞 / be going to | I will go |
| can（予定） | can + 動詞の原形 | She can swim |

---

## 現在の問題一覧

### バトルモード（5問）

#### Stage 1: 三人称単数現在
```javascript
{
  emoji: "🐉",
  sentence: "The hero ______ the dragon.",
  answer: "attacks",
  japanese: "勇者はドラゴンを______。",
  j_answer: "攻撃する",
  choices: ["attack", "attacks", "attacked"],
  hints: [
    "主語 = 自分と相手以外の1人か1つ",
    "The hero = 三人称単数",
    "現在の文 → 現在形"
  ]
}
```

#### Stage 2: 三人称単数現在
```javascript
{
  emoji: "🧙",
  sentence: "She ______ magic every day.",
  answer: "uses",
  japanese: "彼女は毎日、魔法を______。",
  j_answer: "使う",
  choices: ["use", "uses", "used"],
  hints: [
    "主語 = 自分と相手以外の1人か1つ",
    "She = 三人称単数 → s",
    "use の三単現は uses"
  ]
}
```

#### Stage 3: 現在進行形
```javascript
{
  emoji: "👾",
  sentence: "The monsters ______ the town now.",
  answer: "are attacking",
  japanese: "モンスターたちは今、街を______。",
  j_answer: "攻撃している",
  choices: ["attack", "attacks", "are attacking"],
  hints: [
    "now = 今 → 現在進行形",
    "monsters = 複数 → are",
    "進行形 = are + 動詞ing"
  ]
}
```

#### Stage 4: 過去形
```javascript
{
  emoji: "🛡️",
  sentence: "I ______ my shield yesterday.",
  answer: "used",
  japanese: "私は昨日、盾を______。",
  j_answer: "使った",
  choices: ["use", "uses", "used"],
  hints: [
    "yesterday → 過去形",
    "use の過去形は used",
    "過去形は主語で変わらない"
  ]
}
```

#### Stage 5: 過去形
```javascript
{
  emoji: "⚔️",
  sentence: "We ______ the final stage last night.",
  answer: "cleared",
  japanese: "私たちは昨夜、最終ステージを______。",
  j_answer: "クリアした",
  choices: ["clear", "clears", "cleared"],
  hints: [
    "last night → 過去形",
    "clear の過去形 = cleared",
    "We でも過去形は同じ"
  ]
}
```

---

## 元のCanva版からの移植候補

元のゲームには以下の問題がある（50問）。テーマに合わせて問題文を書き換えて移植する。

### 練習モード（25問）の文法パターン

| 文法 | 問題数 |
|------|--------|
| 現在形（I/you/we/they） | 4問 |
| 三人称単数現在 | 7問 |
| 過去形 | 6問 |
| 現在進行形 | 8問 |

### チャレンジモード（25問）の文法パターン

| 文法 | 問題数 |
|------|--------|
| 現在形 | 6問 |
| 三人称単数現在 | 9問 |
| 過去形 | 5問 |
| 現在進行形 | 5問 |

---

## テーマ別の問題文の書き方

同じ文法でもテーマによって使う単語を変える。

### 例：三人称単数現在「動詞にsがつく」

| テーマ | 問題文 | 正解 |
|--------|--------|------|
| バトル | The hero ______ the dragon. | attacks |
| 動物園 | The lion ______ meat every day. | eats |
| 電車 | The train ______ at the station. | arrives |

### 例：過去形

| テーマ | 問題文 | 正解 |
|--------|--------|------|
| バトル | We ______ the final stage last night. | cleared |
| 動物園 | The penguin ______ into the water. | jumped |
| 電車 | The train ______ at 9 AM. | departed |

### 例：現在進行形

| テーマ | 問題文 | 正解 |
|--------|--------|------|
| バトル | The monsters ______ the town now. | are attacking |
| 動物園 | The monkeys ______ in the trees now. | are playing |
| 電車 | The passengers ______ the train now. | are boarding |
