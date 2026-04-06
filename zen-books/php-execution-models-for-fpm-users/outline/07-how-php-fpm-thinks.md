---
title: php-fpm は何を担当しているのか
status: draft
type: chapter
order: 7
book: php-execution-models-for-fpm-users
tags:
  - php-fpm
  - fastcgi
  - process-management
---

# この章の問い

php-fpm は Web サーバーの代わりに何を引き受け、何を引き受けていないのか。

# この章で扱うこと

- プロセス管理
- プール管理
- FastCGI プロセスマネージャーとしての位置づけ
- nginx / Apache との責務分担

# この章の核心

php-fpm は PHP の実行を引き受けるが、
HTTP サーバーそのものでも event loop そのものでもない。
役割は「分離された PHP ワーカー群の管理」である。

# メモ

ここは実務読者の足場になる章。
pm 設定の細部より責務を優先する。
