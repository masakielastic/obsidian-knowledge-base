---
title: php-fpm の外に出るPHPランタイム入門
aliases:
  - php-async-runtime-beyond-fpm
status: planning
type: book
topic: php-async-runtime
tags:
  - php
  - async
  - event-loop
  - reactphp
  - amphp
  - revolt
  - swoole
  - frankenphp
  - embed-php
---

# 概要

php-fpm の外にある長時間型 PHP ランタイムを、
責務分解の観点から整理する本。
event loop、ReactPHP、PHP-PM、Swoole、FrankenPHP、embed PHP を扱う。

# 中心問い

php-fpm の外に出て ReactPHP・Swoole・FrankenPHP などを使うと、
何の責務を自分で持つことになるのか。

# 想定読者

- ReactPHP や AMPHP に興味がある人
- 長時間型 PHP プロセスの考え方を整理したい人
- Swoole や FrankenPHP の位置づけを比較したい人
- php-fpm の外に出たとき、何が増えるのかを知りたい人

# この本で重視すること

- event loop とプロセスマネージャーを混同しない
- non-blocking I/O と worker 運用を分けて理解する
- Swoole や FrankenPHP を単なる流行語ではなく実行モデルとして比較する

# 章一覧

- [[00-introduction]]
- [[01-from-short-lived-to-long-lived]]
- [[02-what-an-event-loop-does]]
- [[03-select-poll-epoll-kqueue-revisited]]
- [[04-libevent-and-libuv]]
- [[05-reactphp-basics]]
- [[06-amphp-and-revolt]]
- [[07-reactphp-and-php-pm]]
- [[08-supervisors-and-service-ops]]
- [[09-swoole-as-runtime-extension]]
- [[10-polling-api-rfc]]
- [[11-frankenphp-and-the-return-of-nearness]]
- [[12-embed-php-and-php-node]]
- [[13-review-runtime-responsibilities]]
- [[14-license]]

# メモ

この本は「速くする方法」の本ではなく、
長時間型 PHP ランタイムの責務を理解するための本である。
