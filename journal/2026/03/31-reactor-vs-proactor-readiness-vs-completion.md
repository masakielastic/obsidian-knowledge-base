---
title: "Reactor / Proactor は「通知の意味」で覚えると整理しやすい"
date: 2026-03-31
tags:
  - ReactorPattern
  - ProactorPattern
  - EventDriven
  - AsyncIO
  - Networking
  - ReadinessModel
  - CompletionModel
  - SystemDesign
  - Concurrency
  - IOModel
---

# Reactor / Proactor は「通知の意味」で覚えると整理しやすい

Reactor と Proactor を見分けるコツは、**通知が何を意味しているか**を見ることだと整理できた。記事では、この違いを readiness と completion の差として説明している。readiness は「今なら読める・書ける・受け取れる」、completion は「依頼した作業が終わった」という合図だ。

Reactor は readiness 寄りの設計として理解すると腹落ちしやすい。流れは、**監視する → 準備できたイベントが来る → ハンドラを呼ぶ → ハンドラ自身が read/write する**。つまり「準備ができたら呼んで。実際の作業はそのあとで自分でやる」という世界観になる。

一方の Proactor は completion 寄りだ。**先に I/O を依頼しておき、完了したら通知を受け、ハンドラは結果を処理する**。こちらは「これをやっといて、終わったら教えて」に近い。合図が来た時点で、仕事そのものはすでに進んでいるか終わっている。

この違いは、誰が仕事を進めるかにもつながる。Reactor では通知はあくまで「今できるよ」なので、呼ばれた側が実作業を進める。Proactor では OS やランタイム側が完了まで面倒を見る度合いが強く、アプリは結果処理に寄りやすい。

実装面の感触もかなり違う。Reactor は仕組みが比較的単純で、多くの OS が得意としてきたが、「読めると言われたのに思ったほど読めない」のような状態管理が増えやすい。Proactor は結果処理に集中しやすい反面、完了通知まで支える基盤が複雑になりやすい。

大事なのは、**Reactor と Proactor は設計パターンであって、OS の I/O モデルと完全に1対1で固定されるわけではない**ことだ。外からは Proactor 風でも、中では Reactor 的に動く実装は普通にありうる。

覚え方としてはこれで十分だと思う。  
**Reactor = 「準備できたら呼んで」**  
**Proactor = 「終わったら知らせて」**

イベント駆動設計を読むときは、「通知が準備なのか、完了なのか」を先に確かめると迷いにくい。
