---
title: "Async for JS Developers — Outline"
aliases:
  - "非同期処理の責務分解アウトライン"
tags:
  - async
  - javascript
  - event-loop
  - concurrency
  - design
  - zenn-book
status: draft
created: 2026-04-05
updated: 2026-04-05
book_slug: async-for-js-developers
---

# Async for JS Developers — Outline

## 本書の目的

JavaScript（ブラウザ / Node.js）ユーザーが、非同期処理を

- Promise / await の使い方
ではなく
- **責務分解（値・計算・実行単位・実行基盤・責任スコープ）**

として理解できるようにする。

---

## 想定読者

- Promise / async / await を使ったことがある
- Node.js やブラウザで非同期処理を書いた経験がある
- Event Loop / Stream / task / runtime の違いが曖昧
- Rust / Python の async を読めるようになりたい

---

## 構成一覧

---

## 第01章　この本の読み方 ― 正解ではなく分け方を学ぶ

- なぜ async は「わかった気になる」のか
- 本書のアプローチ
  - 論点仮説
  - アプローチ仮説
  - 答え仮説
- 対話形式の意図
- 3人の登場人物
  - 初学者
  - 解説者
  - 実装者

---

## 第02章　JavaScript ユーザーは何を知っていて、何を誤解しやすいのか

- Promise / await の強み
- Node.js とブラウザの差
- よくある誤解
  - Promise が async の標準形
  - await が万能
- 「動く」と「責任を持つ」の違い

---

## 第03章　Stream・Socket・Fiber・Event Loop を同じものだと思わない

- Stream が主体に見える理由
- Socket / Stream は何者か
  - ハンドルという概念
- 読み書き vs 監視
- Fiber の正体
- Event Loop の誤解
- オブジェクト分類
  - 主体 / 値 / ハンドル / 実行単位

---

## 第04章　イベントループは何を回しているのか

- イベントキューとは何か
- 「仕事の交通整理」としての Event Loop
- readiness モデル
- completion モデル
- libuv の役割
- Node.js で責務が見えにくい理由

---

## 第05章　Promise は何者で、何をしていないのか

- Promise の本質（将来値）
- await の意味（完了待ち）
- Promise の強み
- Promise の限界
- Promise.all の位置づけ
- 「便利さ」と「責務」の違い

---

## 第06章　Future は Promise の別名ではない

- Future = 計算
- Promise = 値
- poll の概念
- executor / runtime の役割
- task の概念
- await / spawn / join の違い

---

## 第07章　task・runtime・Event Loop はどこでつながるのか

- 値・計算・仕事・実行基盤の関係
- Promise / Future / coroutine / Task の比較
- Event Loop と runtime の関係
- Monitor / Scheduler / Dispatcher
- callback と continuation

---

## 第08章　なぜ「動くコード」でも設計は破綻するのか

- 野良タスク
- 例外の取りこぼし
- キャンセルの不整合
- timeout / retry の責務
- Stream の God Object 化
- HTTP/2 で破綻する理由

---

## 第09章　構造化並行性は責任の木である

- 非構造化並行の問題
- coroutine 中心の限界
- TaskGroup の意味
- ExceptionGroup
- 例外は上へ、キャンセルは下へ
- Trio と asyncio の違い

---

## 第10章　ブラウザ・Node.js・Rust・Python を同じ地図で見る

- 責務マップの整理
  - 値
  - 計算
  - 実行単位
  - ハンドル
  - 実行基盤
  - 責任スコープ
- API 対応表
- 「似ている名前」と「違う責務」

---

## 第11章　これから Rust / Python をどう読むか

- Tokio の読み方
- asyncio の読み方
- どこから読むべきか
- 先にメンタルモデルを持つ意味
- 今後の学習ロードマップ

---

## 付録A　登場人物ガイド

- 初学者（Explorer）
- 解説者（Architect）
- 実装者（Engineer）

---

## 付録B　対話の型（3つの仮説）

- 論点仮説
- アプローチ仮説
- 答え仮説

---

## 付録C　責務マップ早見表

- 値
- 計算
- 実行単位
- ハンドル
- 監視
- 調停
- スコープ

---
