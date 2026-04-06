---
title: なぜ php-fpm は自然な選択だったのか
status: draft
type: chapter
order: 10
book: php-execution-models-for-fpm-users
tags:
  - review
  - php-fpm
  - architecture
---

# この章の問い

ここまでの話を踏まえると、なぜ php-fpm は広く使われる形になったのか。

# この章で扱うこと

- CGI から FastCGI への流れ
- mod_php の強みと限界
- Apache MPM との関係
- php-fpm の妥当性の再整理

# この章の核心

php-fpm の主流化は偶然ではなく、
Web サーバーと PHP を分離する方向が実務上も設計上も有利だった結果である。

# メモ

読者に「今の構成の意味が見えた」と感じてもらう章にする。
