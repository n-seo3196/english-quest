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

## 作問ルール

### 対象レベル

小学校高学年〜中学生（英語学習初期〜中期）

---

### 1. 語彙（動詞）の選び方

**基準：中学1年生が聞いたとき、意味がわかるか？**

- `fly / use / cross / make / fight / run / help / save / clean / watch / learn / clear / attack` このくらいのレベルを目安にする
- 「知らなくても想像できる」ではなく「絶対聞いたことがある」単語を選ぶ
- 動詞の意味がわからないと文法ではなく語彙の問題になるため、なじみのない語は避ける

**使いやすい動詞リスト（優先度高）**

| 動詞 | 活用 | 備考 |
|------|------|------|
| use | use / uses / used | 汎用性高い |
| attack | attack / attacks / attacked | バトルテーマの定番 |
| help | help / helps / helped | 自然な文が作りやすい |
| save | save / saves / saved | バトル・ファンタジー向き |
| clear | clear / clears / cleared | 「クリア」として子どもに馴染み深い |
| watch | watch / watches / watched | 三単現で es がつく練習にも使える |
| run | run / runs / ran | 不規則変化。シンプル |
| fly | fly / flies / flew | 不規則変化。ゲームで自然 |
| fight | fight / fights / fought | 不規則変化。バトル感あり |
| learn | learn / learns / learned | 魔法・学院テーマに合う |
| clean | clean / cleans / cleaned | 装備の手入れとして自然 |
| make | make / makes / made | 不規則変化。汎用性高い |
| cross | cross / crosses / crossed | 進行形の練習に使いやすい |
| visit | visit / visits / visited | 過去形の練習に使いやすい |
| heal | heal / heals / healed | 回復役・魔法テーマに合う |
| train | train / trains / trained | 「鍛える」。バトル文脈で自然 |

**避けるべき語彙**

- `defeat`（「倒す」の英語として子どもに定着していない）
- `equip`（活用形が難しい）
- `rescue`（rescuedの発音・スペルが難しい）
- `avoid`、`protect`、`explore`（中学1年生には難易度高め）

---

### 2. 文の作り方

- **テーマ**：バトル・ファンタジー要素（勇者・魔法・ドラゴン・城・ダンジョンなど）
- **時制の手がかり**を文中に必ず入れる
  - 現在形 → `every day / every morning / every night` など
  - 現在進行形 → `now / right now` など
  - 過去形 → `yesterday / last night / last week / last month` など
- 空欄は文中に1箇所のみ（`______` 6個のアンダースコアで統一）
- 文が長くなりすぎないよう注意（クイズボックスに1行で収まる長さが理想）

---

### 3. 選択肢（choices）の作り方

- 必ず3つ（ゲートが3つのため）
- 正解を1つ含む
- 残り2つは「ありがちな間違い」にする

**カテゴリ別の選択肢パターン**

| カテゴリ | 選択肢の構成例 |
|---------|--------------|
| 現在形 | 動詞そのまま / 動詞+s / 動詞+ed |
| 三単現 | 動詞そのまま / 動詞+s / 動詞+ed |
| 現在進行形 | 動詞そのまま / 動詞+s / be+動詞ing |
| 過去形（規則） | 動詞そのまま / 動詞+s / 動詞+ed |
| 過去形（不規則） | 動詞そのまま / 動詞+s / 不規則過去形 |

- 表示順はゲーム側でシャッフルされるため、配列の順番は気にしなくてOK

---

### 4. ヒント（hints）の作り方

- 最大3つ。段階的に詳しくなるように並べる：
  1. **一般的な文法ルール**（例：「now = 今 → 現在進行形」）
  2. **この文への当てはめ**（例：「The dragon = 単数 → is」）
  3. **ほぼ答え**（例：「進行形 = is + 動詞ing」）
- 文字が多い場合は `<br>` で改行できる（`innerHTML` で描画されるため）
- 専門用語は避け、子どもでもわかる言葉で書く
  - ✕「三人称単数」→ ✓「自分と相手以外の1人か1つ」

---

### 5. 日本語訳（japanese / j_answer）の書き方

- `japanese`：英文の日本語訳。空欄部分は `______` のまま残す
- `j_answer`：空欄に入る日本語。文脈に合った自然な活用形にする
  - 例：過去の文なら「使った」「戦った」など活用済みでOK
- 辞書形でも活用形でも、読んで自然に聞こえる方を選ぶ

---

### 6. カテゴリバランス（30問プールの場合）

| カテゴリ | 問数 |
|---------|------|
| 現在形（I/you/we/they） | 7問 |
| 三単現（he/she/it/固有名詞） | 8問 |
| 現在進行形 | 8問 |
| 過去形 | 7問 |
| **合計** | **30問** |

毎回5問がランダムで選ばれる。問題を追加するときもこのバランスを意識する。

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

### バトルモード（30問プール・毎回5問ランダム出題）

#### 現在形（7問）― 主語 I/you/we/they + 動詞そのまま

| # | 絵文字 | 英文 | 正解 | 日本語訳 |
|---|--------|------|------|---------|
| 1 | ⚔️ | I ______ every day to become stronger. | train | 私は強くなるために毎日______。 |
| 2 | 🏰 | We ______ the enemy every night. | watch | 私たちは毎晩、敵を______。 |
| 3 | 🗡️ | You ______ magic at the academy. | learn | あなたは学院で魔法を______。 |
| 4 | 👾 | They ______ the village every day. | save | 彼らは毎日、村を______。 |
| 5 | 🔥 | We ______ the dungeon every weekend. | clear | 私たちは毎週末、ダンジョンを______。 |
| 6 | 🛡️ | I ______ my sword after every battle. | clean | 私は戦いのたびに剣を______。 |
| 7 | 💫 | You ______ your friends in every battle. | help | あなたは毎回の戦いで仲間を______。 |

#### 三単現（8問）― 主語 he/she/it/固有名詞 + 動詞+s

| # | 絵文字 | 英文 | 正解 | 日本語訳 |
|---|--------|------|------|---------|
| 8 | 🐉 | The hero ______ the dragon. | attacks | 勇者はドラゴンを______。 |
| 9 | 🧙 | She ______ magic every day. | uses | 彼女は毎日、魔法を______。 |
| 10 | 🔥 | The dragon ______ in the sky every night. | flies | ドラゴンは毎晩、空を______。 |
| 11 | 🧝 | The elf ______ through the forest every day. | runs | エルフは毎日、森の中を______。 |
| 12 | 👸 | The princess ______ her friends every battle. | helps | 王女は毎回の戦いで、仲間を______。 |
| 13 | 🧙‍♂️ | The wizard ______ a new spell every day. | learns | 魔法使いは毎日、新しい呪文を______。 |
| 14 | ⚔️ | He ______ his sword every morning. | cleans | 彼は毎朝、剣を______。 |
| 15 | 🛡️ | The knight ______ the weak every day. | saves | 騎士は毎日、弱い者を______。 |

#### 現在進行形（8問）― be動詞 + 動詞ing

| # | 絵文字 | 英文 | 正解 | 日本語訳 |
|---|--------|------|------|---------|
| 16 | 👾 | The monsters ______ the town now. | are attacking | モンスターたちは今、街を______。 |
| 17 | 🐉 | The dragon ______ over the castle right now. | is flying | ドラゴンは今、城の上を______。 |
| 18 | 🔮 | I ______ a new magic spell now. | am learning | 私は今、新しい呪文を______。 |
| 19 | 🌲 | The heroes ______ the dark forest now. | are crossing | 勇者たちは今、暗い森を______。 |
| 20 | 🧪 | The witch ______ a potion right now. | is making | 魔女は今、薬を______。 |
| 21 | ⚔️ | He ______ with the dark knight right now. | is fighting | 彼は今、ダークナイトと______。 |
| 22 | 🏃 | We ______ to the castle now. | are running | 私たちは今、城へ______。 |
| 23 | 🏰 | The enemy ______ our castle right now. | is attacking | 敵は今、私たちの城を______。 |

#### 過去形（7問）― 動詞+ed（または不規則変化）

| # | 絵文字 | 英文 | 正解 | 日本語訳 | 備考 |
|---|--------|------|------|---------|------|
| 24 | 🛡️ | I ______ my shield yesterday. | used | 私は昨日、盾を______。 | 規則 |
| 25 | ⚔️ | We ______ the final stage last night. | cleared | 私たちは昨夜、最終ステージを______。 | 規則 |
| 26 | 🏛️ | They ______ the ancient ruins last month. | visited | 彼らは先月、古代遺跡を______。 | 規則 |
| 27 | 👸 | The knight ______ the princess last week. | helped | 騎士は先週、王女を______。 | 規則 |
| 28 | 👾 | The monsters ______ the village last night. | attacked | モンスターたちは昨夜、村を______。 | 規則 |
| 29 | 🔮 | We ______ the dragon yesterday. | fought | 私たちは昨日、ドラゴンと______。 | **不規則**（fight→fought） |
| 30 | 💚 | She ______ the wounded knight yesterday. | healed | 彼女は昨日、傷ついた騎士を______。 | 規則 |

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
