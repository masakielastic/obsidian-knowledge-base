---
title: CGI は何がつらかったのか
status: draft
type: chapter
order: 2
book: php-execution-models-for-fpm-users
tags:
  - php
  - cgi
  - web-server
---

# この章の問い

CGI は何が分かりやすく、何が限界だったのか。

# この章で扱うこと

- CGI の基本的な考え方
- リクエストごとに外部プロセスを起動するモデル
- 分かりやすさとコストの両面
- FastCGI が必要になった背景

# この章の核心

CGI の問題は「古い」ことではなく、毎回起動するという構造にある。
FastCGI や php-fpm は、その構造上の問題に対する答えとして理解するべきである。

# メモ

懐古話にしない。
「なぜ次の方式が必要になったか」に集中する。
