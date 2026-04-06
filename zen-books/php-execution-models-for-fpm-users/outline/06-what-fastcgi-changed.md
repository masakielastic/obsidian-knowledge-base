---
title: FastCGI は何を変えたのか
status: draft
type: chapter
order: 6
book: php-execution-models-for-fpm-users
tags:
  - fastcgi
  - php
  - architecture
---

# この章の問い

FastCGI は CGI と比べて何を変え、なぜ php-fpm につながるのか。

# この章で扱うこと

- CGI との差
- 常駐プロセスという発想
- Web サーバーと PHP の分離
- php-fpm への接続

# この章の核心

FastCGI の本質は高速化というより、
PHP を毎回起動するのではなく、外部プロセスとして保持する点にある。

# メモ

性能比較だけで終わらせず、責務分離の意味を書く。
