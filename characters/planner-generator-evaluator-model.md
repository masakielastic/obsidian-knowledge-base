---
title: "Planner / Generator / Evaluator Model Overview"
aliases:
  - "PGEモデル概要"
  - "対話設計の役割分担モデル"
tags:
  - planner-generator-evaluator
  - pge-model
  - dialogue
  - thinking-framework
  - information-design
  - knowledge-design
  - communication
  - tech-writing
status: evergreen
type: concept
topic: dialogue-design
framework:
  - planner
  - generator
  - evaluator
created:
updated:
source:
cssclasses:
---

# Planner / Generator / Evaluator Model

## Overview
Planner / Generator / Evaluator（PGE）モデルは、  
思考・文章作成・議論を安定させるための**役割分担モデル**である。

このモデルでは、ひとつの思考プロセスを次の3つに分解する。

- Planner：何を扱うかを決める
- Generator：内容を具体化する
- Evaluator：誤解や弱点を検証する

重要なのは、これを「人の役職」ではなく、  
**発話・思考の機能として分けること**である。

---

## Why This Model Matters

1人で文章を書く場合でも、

- 論点整理
- 本文生成
- 自己評価

が頭の中で混線しやすい。

AIに文章を書かせる場合も同様で、

- 論点が曖昧なまま
- それっぽい説明だけが増える

という問題が起きやすい。

PGEモデルは、この混線を防ぐための  
**思考の交通整理**として機能する。

---

## Roles

### Planner
- 論点と非目標を決める
- 話す範囲と粒度を定義する
- 記事や議論の骨格を作る

**Not**
- すべてを詳細設計する人ではない

---

### Generator
- 枠内で説明を具体化する
- 例、比喩、対比を使って展開する
- 実務や現場の文脈に落とす

**Not**
- 無制限に話を広げる人ではない

---

### Evaluator
- 誤解や弱点を先に見つける
- 読者のつまずきを代弁する
- 境界条件や抜けを確認する

**Not**
- 単なる否定やダメ出しの役ではない

---

## Key Principle

このモデルの本質は、

👉 **キャラではなく機能で考えること**

である。

対話形式の記事でも重要なのは、

- 誰がしゃべるか
ではなく
- この発話は何の役割か

である。

---

## Dialogue Pattern (Recommended)

1. Planner：今回の論点を提示する
2. Evaluator：誤解や雑な理解を出す
3. Generator：説明・具体例を提示する
4. Evaluator：弱点や境界を確認する
5. Planner：要点をまとめる

この流れがあるだけで、対話は大きく安定する。

---

## Applications

このモデルは記事だけでなく、さまざまな場面に適用できる。

- 技術記事・対話記事の設計
- RFC（論点整理・仕様・FAQ）
- AIプロンプト設計
- コードレビュー
- 会議・議論の進行

---

## Summary

- Planner は論点と非目標を決める
- Generator は枠内で具体化する
- Evaluator は誤解と弱点を可視化する
- 役割はキャラではなく思考機能として扱う
