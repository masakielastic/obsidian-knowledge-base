---
title: mod_php はなぜ主流でなくなったのか
status: draft
type: chapter
order: 4
book: php-execution-models-for-fpm-users
tags:
  - php
  - mod-php
  - apache
  - architecture
---

# この章の問い

mod_php は便利だったのに、なぜ主流ではなくなっていったのか。

# この章で扱うこと

- 責務分離の必要
- Apache MPM との関係
- 運用や構成の柔軟性
- php-fpm への移行が支持された背景

# この章の核心

mod_php の衰退は、単なる流行の変化ではない。
Web サーバーと PHP の責務を分ける方向が、運用上も設計上も有利だったからである。

# メモ

ここで mod_php を悪者にしない。
「時代遅れ」より「前提条件が変わった」を主軸にする。
