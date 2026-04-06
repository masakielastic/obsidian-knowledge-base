---
title: select・poll・epoll・kqueue をもう一度見る
status: draft
type: chapter
order: 3
book: php-async-runtime-beyond-fpm
tags:
  - select
  - poll
  - epoll
  - kqueue
---

# この章の問い

OS の polling 機構は、event loop の下でどんな役割を果たしているのか。

# この章の核心

多数の待機対象を扱うには、
OS の readiness 通知と、それを束ねる loop の両方が必要である。
