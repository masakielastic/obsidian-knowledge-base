---
title: event loop は何をしているのか
status: draft
type: chapter
order: 2
book: php-async-runtime-beyond-fpm
tags:
  - event-loop
  - io
  - async
---

# この章の問い

event loop は具体的に何を管理しているのか。

# この章の核心

event loop は魔法ではなく、
待機可能なものの監視と、実行の順序づけを担当する基盤である。
