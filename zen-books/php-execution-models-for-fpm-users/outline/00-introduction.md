---
title: はじめに
status: draft
type: chapter
order: 0
book: php-execution-models-for-fpm-users
tags:
  - php
  - introduction
---

# この章の問い

PHP の非同期や実行モデルを学ぶとき、なぜ最初に php-fpm の前提から整理する必要があるのか。

# この章で扱うこと

- PHP の「言語知識」と「実行形態の知識」は別であること
- この本の対象が async ライブラリそのものではないこと
- まず CGI / mod_php / php-fpm の流れを押さえる必要があること

# この章の核心

PHP はいつも同じ形で動いているわけではない。
実行形態の違いを理解しないと、非同期ランタイムや FrankenPHP の話も比較できない。

# メモ

導入では専門用語を増やしすぎない。
SAPI という語は出してよいが、最初は「PHP の動き方の違い」と平易に言い換える。
