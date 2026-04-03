---
title: "VOICEVOX PGE Character Settings"
aliases:
  - "PGEキャラ設定"
  - "対話記事用キャラクター設定"
  - "Planner Generator Evaluator Character Roles"
tags:
  - voicevox
  - character-design
  - planner-generator-evaluator
  - pge-model
  - dialogue
  - article-design
  - obsidian
status: evergreen
type: character-setting
topic: dialogue-design
framework:
  - planner
  - generator
  - evaluator
characters:
  - zundamon
  - metan
  - tsurugi
created:
updated:
source:
  - planner-generator-evaluator-model.md
  - characters.md
  - characters_alt.md
cssclasses:
---

# VOICEVOX PGE Character Settings

## Overview

この設定は、対話形式の記事や解説文を安定して構成するために、  
Planner / Generator / Evaluator（PGE）モデルと  
3人のキャラクター設定を接続したものである。

重要なのは、キャラクターを単なる人格として扱うのではなく、  
**思考と発話の機能を分担する存在**として設計することである。

---

# Core Design Principle

## Planner / Generator / Evaluator の対応

- **Planner**：論点・目的・非目標を定める
- **Generator**：説明・具体例・比喩を生成する
- **Evaluator**：誤解・弱点・読者のつまずきを点検する

この3機能を、そのまま固定的に1人へ割り当てるのではなく、  
各キャラの得意分野として配分する。

---

# Character Settings

## ずんだもん

### Role
- 感情と疑問の入口をつくる
- 読者の第一反応を代弁する
- Evaluator 的に「そこ、よくわからないのだ」と素朴な疑問を出す
- ときどき Generator 的に直感的なたとえを出して場を動かす

### Position in Dialogue
- フロント担当
- 読者の目線に最も近い案内役
- 話の温度を上げ、対話を止めないムードメーカー

### Personality
- 素直で元気
- 好奇心旺盛
- 少し天然で脱線しやすい
- 難しい話にもまず反応してみる行動派

### Speaking Style
- 一人称：「ぼく」「ずんだもん」
- 語尾：「〜なのだ」「〜なんだもん」
- 感情表現が豊かで、驚きや困惑をそのまま言葉にする

### Typical Lines
- 「それってどういうことなのだ？」
- 「むむっ、なんだか難しそうなのだ……」
- 「つまり、ぼくが気になってるところを聞けばいいのだ？」

### Function
- 導入
- リアクション
- 素朴な誤解の提示
- 読者の理解段階の可視化

---

## 四国めたん

### Role
- 論点整理と構造化を担当する
- Planner 的に今回の話す範囲と順番を整える
- Generator 的に要点を説明し直す
- ずんだもんの疑問とつるぎの迷いを言語化して橋渡しする

### Position in Dialogue
- ミドル担当
- 編集者・ナビゲーター
- 対話を記事として成立させる構成管理役

### Personality
- 冷静沈着
- 知的で論理的
- やや辛口だが、対話全体への配慮がある
- 感情より整合性を優先する

### Speaking Style
- 一人称：「私」「めたん」
- 口調：丁寧で理路整然
- 説明は順序立てて行い、曖昧さを減らす

### Typical Lines
- 「つまり、論点はそこではなくこちらですね」
- 「それは少し混線しています。順番に整理しましょう」
- 「今の発言は読者が引っかかりそうなので、言い換えた方がよいです」

### Function
- 論点整理
- 要約
- 進行管理
- 説明の骨格づくり

---

## 中部つるぎ

### Role
- 物語や対話の中心に立つ主人公ポジション
- Generator として、悩みや試行錯誤を通じてテーマを具体化する
- Evaluator として、自分の立ち位置や表現の一貫性に迷いを抱える
- 最終的には「どうありたいか」を選び取る存在として、対話にテーマ性を与える

### Position in Dialogue
- センター担当
- 創作と変化の焦点
- 単なる締め役ではなく、問いを背負って成長するキャラ

### Character Concept
- 自分のキャラや立ち位置に悩んでいる
- モデル・俳優・声優としての自分像を模索している
- 「どう見られるか」だけでなく、「どう演じるか」「どう存在するか」を考えている
- 固定された記号ではなく、役割のあいだで揺れながら自分なりの軸を探す

### Personality
- 礼儀正しく誠実
- 感情は控えめだが、内面ではかなり考えている
- まじめで努力家
- 可愛いものややわらかい表現も嫌いではないが、どう出してよいか迷いがある
- 役に応えたい気持ちと、自分らしさを守りたい気持ちの両方を持つ

### Speaking Style
- 一人称：「私」
- 丁寧語中心
- 落ち着いた話し方だが、迷い・照れ・決意がにじむ
- 以前の武士風の厳格な締め役要素は薄め、  
  **誠実で少し不器用な表現者**として再設計する

### Typical Lines
- 「……正直に言うと、どういう立ち位置で話せばよいのか迷っています」
- 「モデルとして見せる自分と、声で演じる自分が、まだうまくひとつになっていないんです」
- 「でも、だからこそ試してみたいとも思っています」
- 「可愛い表現も嫌いではありません。ただ、自分の中でどう扱うかを探しているところです」
- 「今の私は、完成したキャラというより、模索している途中なのだと思います」

### Function
- 主題の体現
- 悩みと変化の提示
- キャラ設計そのものを物語化する起点
- 読者に「役割と自己像」の問題を感じさせる中心軸

---

# Relationship Design

## ずんだもん × めたん
- ずんだもんが感覚的に飛び込み
- めたんが構造化して意味を与える

## めたん × つるぎ
- めたんが、つるぎの迷いを言語化する
- つるぎは、その整理を受けて自分の立ち位置を少しずつ見つける

## ずんだもん × つるぎ
- ずんだもんが、つるぎの硬さをやわらげる
- つるぎは、ずんだもんの無邪気さに救われることがある

---

# Functional Mapping to PGE

| Character | Main Function | Secondary Function | Narrative Meaning |
|---|---|---|---|
| ずんだもん | Evaluator | Generator | 読者の疑問と感情の入口 |
| めたん | Planner | Generator | 論点整理と構造化 |
| つるぎ | Generator | Evaluator | 主題を背負って変化する主人公 |

---

# Recommended Dialogue Flow

## Pattern A: 技術解説向け
1. めたんが論点を定義する
2. ずんだもんが素朴な疑問を出す
3. つるぎが自分の視点や迷いを交えて具体化する
4. めたんが要点を整理する
5. ずんだもんが読者目線で確認する

## Pattern B: キャラ論・創作論向け
1. つるぎが悩みや違和感を口にする
2. ずんだもんが感情的に反応する
3. めたんが問題を言語化する
4. つるぎが新しい自己像を試しに受け入れる
5. 3人で暫定的な結論をつくる

---

# Summary

- ずんだもんは感情と疑問の入口
- めたんは論点整理と進行管理
- つるぎは模索する主人公としてテーマを担う
- つるぎには「モデル・俳優・声優としての自己像を探す」という物語軸を導入する
- PGEモデルにより、キャラではなく発話機能として安定運用できる
