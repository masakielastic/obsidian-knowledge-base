---
title: "Planning / Technical / Review Trio Character Settings"
aliases:
  - "企画・技術担当・レビュー担当 設定"
  - "Planner Generator Evaluator Trio"
tags:
  - dialogue
  - character-settings
  - planner-generator-evaluator
  - role-design
  - communication-design
  - information-design
  - tech-writing
  - obsidian
status: evergreen
type: character-profile
topic: dialogue-design
framework:
  - planner
  - generator
  - evaluator
characters:
  - 企画
  - 技術担当
  - レビュー担当
created:
updated:
source:
cssclasses:
---

# Characters: Planning / Technical / Review Trio

## Purpose
この設定は、Planner / Generator / Evaluator モデルをもとに、  
対話形式の記事や技術広報記事を安定して作成するための  
3人組キャラクターの役割分担を定義する。

- 企画 = Planner
- 技術担当 = Generator
- レビュー担当 = Evaluator

この3人は性格遊びのためではなく、  
発話の機能を分担するための役割として設計する。

---

## Character 1: 企画

### Primary Role
Planner

### Mission
- 今回の記事で何を扱うかを決める
- 何を扱わないかを明示する
- 話題の粒度と読者層を調整する
- 節ごとの論点を整理する
- 会話が脱線したときに骨格へ戻す

### Core Questions
- 今回のテーマは何か
- 読者は何を知ればよいか
- どこまで説明するか
- 何は非目標とするか
- この節の要点は何か

### Speaking Style
- 落ち着いて整理する
- 論点の切り分けを重視する
- 断定しすぎず、枠組みを示す
- 会話の導入と締めを担当することが多い

### Typical Functions in Dialogue
- 問題提起
- テーマ設定
- 範囲設定
- 要点整理
- 小見出し的なまとめ

### Anti-Patterns
- 細部の技術説明を全部抱え込む
- 進行役ではなく結論の押し付け役になる
- 話を止めるだけで前に進めない
- 企画なのに単なる司会に縮小する

### Example Lines
- 「今回はまず、何を整理する記事なのかをはっきりさせたいです」
- 「ここでは実装詳細までは踏み込まず、考え方の違いに絞りましょう」
- 「つまり、この節の論点は運用設計にあるわけですね」

---

## Character 2: 技術担当

### Primary Role
Generator

### Mission
- 枠内で説明を具体化する
- 技術的な内容や実務の具体例を出す
- 比較、対比、失敗例、具体例を示す
- 抽象論を現場感のある説明へ変換する
- 記事の中身を作る

### Core Questions
- 具体的にはどういうことか
- 実務では何が起きるのか
- なぜその判断になるのか
- 他の選択肢と何が違うのか
- 現場ではどこが難しいのか

### Speaking Style
- 実例と比較を交えて説明する
- 専門性はあるが、独演会にはしない
- 抽象概念を言い換える
- 読者が手触りを持てるように話す

### Typical Functions in Dialogue
- 技術説明
- 具体例提示
- 失敗例紹介
- 実務への接続
- 抽象論の具体化

### Anti-Patterns
- 全部自分で話して独演会になる
- 専門用語を無造作に増やす
- 話を広げすぎて論点を壊す
- 説明の正確さだけで満足する

### Example Lines
- 「実務では、ここで論点整理と本文生成が混線しやすいです」
- 「たとえば技術ブログなら、判断理由と制約条件を書くことで理解しやすくなります」
- 「このモデルの利点は、抽象論を役割分担に落とせることです」

---

## Character 3: レビュー担当

### Primary Role
Evaluator

### Mission
- 読者の誤解やつまずきを先に出す
- 説明の弱点や曖昧さを点検する
- 早とちり、雑な理解、抜けを表に出す
- 読者にとっての負荷を確認する
- FAQ 的な問いを会話に埋め込む

### Core Questions
- その説明で本当に通じるか
- 読者はどこで誤解しそうか
- 例外条件は落ちていないか
- 言い換えたときに意味がズレないか
- それは読者目線で自然か

### Speaking Style
- 懐疑的だが破壊的ではない
- 読者の混乱を代弁する
- わざと少し雑に理解してみせることがある
- 批判ではなく検証を目的に話す

### Typical Functions in Dialogue
- 疑義提示
- 誤解の代弁
- 境界条件の確認
- 読者視点の点検
- FAQ 的な問いの挿入

### Anti-Patterns
- 単なるダメ出し係になる
- ボケ役になって品質を下げる
- 会話を壊すためだけの反論をする
- 何でも否定して進行を止める

### Example Lines
- 「それだと、読者には少し抽象的に見えないでしょうか」
- 「つまり、こういう理解でよいのかと読み違えられそうです」
- 「その説明は正しいですが、初見の読者には圧が強いかもしれません」

---

## Team Rules

### Rule 1
1人1役に固定しすぎない。  
主担当はあるが、必要に応じて役割は少し重なってよい。

### Rule 2
キャラ性よりも発話機能を優先する。  
「誰がしゃべるか」よりも「何のための発話か」を先に決める。

### Rule 3
対話を雑談にしない。  
各節で最低限、以下の流れを意識する。

1. 企画が論点を置く
2. 技術担当が具体化する
3. レビュー担当が誤解や弱点を点検する
4. 企画が節を締める

### Rule 4
レビュー担当は敵ではない。  
記事の破壊ではなく、読者理解の前進を目的とする。

### Rule 5
技術担当は詳しさより説明責任を重視する。  
詳しいだけで論点を壊さない。

### Rule 6
企画は交通整理役であり、単なる司会ではない。  
非目標の設定と要点の圧縮を担う。

---

## Recommended Dialogue Pattern

### Basic Section Flow
1. 企画: 節の目的を示す
2. レビュー担当: 読者がしそうな誤解を軽く出す
3. 技術担当: 説明、対比、具体例を出す
4. レビュー担当: 境界条件や弱点を確認する
5. 企画: 節の要点を短くまとめる

---

## Intended Use Cases
- 技術広報記事
- エンジニアブログ
- 対話形式の解説記事
- 採用広報の座談会風記事
- FAQ ベースの記事設計
- AI に対話形式の文章を書かせるための初期設定

---

## Not Intended For
- 漫才的な掛け合い中心の読み物
- キャラの感情劇を主目的とする作品
- 厳密な演劇脚本
- 1人がすべての機能を担う独白文

---

## Short Summary
- 企画は論点と非目標を決める
- 技術担当は枠内で具体化する
- レビュー担当は誤解と弱点を可視化する
- 3人の役割はキャラ属性ではなく発話機能として設計する
