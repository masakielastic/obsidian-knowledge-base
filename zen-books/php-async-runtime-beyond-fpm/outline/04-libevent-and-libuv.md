---
title: libevent と libuv は何者か
status: draft
type: chapter
order: 4
book: php-async-runtime-beyond-fpm
tags:
  - libevent
  - libuv
  - event-loop
---

# この章の問い

なぜ OS の polling API だけでは足りず、中間ライブラリが必要になるのか。

# この章の核心

実用的な event loop には、
監視だけでなくタイマー、コールバック、キュー管理などの組み立てが必要になる。
