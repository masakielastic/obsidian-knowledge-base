---
title: ReactPHP 入門
status: draft
type: chapter
order: 5
book: php-async-runtime-beyond-fpm
tags:
  - reactphp
  - event-loop
  - php
---

# この章の問い

ReactPHP は 1 プロセスの中で何を担当しているのか。

# この章の核心

ReactPHP の中心は、非同期処理の魔法ではなく、
event loop を核に I/O 待機を扱う実行基盤である。
