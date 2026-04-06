---
title: supervisord / systemd など外側の監督者
status: draft
type: chapter
order: 8
book: php-async-runtime-beyond-fpm
tags:
  - supervisord
  - systemd
  - operations
---

# この章の問い

長時間型 PHP プロセスを本番で回すには、アプリの外側に何が必要なのか。

# この章の核心

プロセスマネージャーがあっても、
サービスとして継続稼働させる責務はさらに外側に残る。
