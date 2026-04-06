---
title: select・poll・epoll・kqueue への入口
status: draft
type: chapter
order: 9
book: php-execution-models-for-fpm-users
tags:
  - select
  - poll
  - epoll
  - kqueue
  - io
---

# この章の問い

多数の接続待機を扱うとき、OS はどのような仕組みを用意しているのか。

# この章で扱うこと

- readiness の基本感覚
- select / poll の素朴な考え方
- epoll / kqueue の方向性
- 第2冊につながる入口としての位置づけ

# この章の核心

非同期ランタイムの前に、
まず OS が「待てるものをどう知らせるか」を理解する必要がある。

# メモ

深入りしすぎない。
ここは「橋をかける章」。
