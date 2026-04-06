---
title: ReactPHP と PHP-PM は何が違うのか
status: draft
type: chapter
order: 7
book: php-async-runtime-beyond-fpm
tags:
  - reactphp
  - php-pm
  - process-management
---

# この章の問い

ReactPHP と PHP-PM は、どこまでが同じ世界で、どこからが別責務なのか。

# この章の核心

ReactPHP は 1 プロセス内の待機と実行を扱い、
PHP-PM は worker 群の運用を扱う。
両者は補完関係にあるが、同じものではない。
