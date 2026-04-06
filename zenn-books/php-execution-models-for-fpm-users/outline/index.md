---
title: PHP-FPMユーザーのための実行モデル基礎
aliases:
  - php-fpm-users-execution-models
status: planning
type: book
topic: php-execution-models
tags:
  - php
  - php-fpm
  - cgi
  - mod-php
  - fastcgi
  - apache
  - mpm
  - web-server
---

# 概要

PHP の実行形態を、歴史と責務分解の両面から学ぶための本。
CGI、mod_php、FastCGI、php-fpm、Apache MPM を整理し、
なぜ php-fpm が主流になったのかを理解する。

# 中心問い

PHP はなぜ CGI や mod_php から離れ、php-fpm が主流になったのか。

# 想定読者

- 普段は nginx / Apache + php-fpm を使っている人
- PHP の実行形態の違いを整理したい人
- ReactPHP や FrankenPHP に興味はあるが、その前提知識を固めたい人

# この本で重視すること

- CGI / mod_php / FastCGI / php-fpm を歴史としてではなく設計比較として理解する
- Apache MPM と PHP の実行モデルを混同しない
- 非同期の話に入る前に、短命リクエスト型の前提を固める

# 章一覧

- [[00-introduction]]
- [[01-php-is-not-always-the-same]]
- [[02-why-cgi-became-a-problem]]
- [[03-why-mod-php-was-powerful]]
- [[04-why-mod-php-faded]]
- [[05-apache-mpm-basics]]
- [[06-what-fastcgi-changed]]
- [[07-how-php-fpm-thinks]]
- [[08-processes-threads-and-connections]]
- [[09-select-poll-epoll-kqueue-intro]]
- [[10-review-why-fpm-made-sense]]
- [[11-license]]

# メモ

この本は async 本ではなく、実行モデルの基礎本である。
第2冊の前提知識をつくる役割を持つ。
