---
title: PHP はいつも同じ形で動いているわけではない
status: draft
type: chapter
order: 1
book: php-execution-models-for-fpm-users
tags:
  - php
  - sapi
  - runtime
---

# この章の問い

PHP の実行形態にはどんな違いがあり、それは後の議論にどう影響するのか。

# この章で扱うこと

- SAPI の見取り図
- apache2handler / cgi-fcgi / fpm-fcgi / cli / embed の位置づけ
- 「PHP を学ぶ」と「PHP がどう動くかを学ぶ」は別であること

# この章の核心

PHP は単一の実行モデルを持つ言語ではない。
接続形態やホスト環境によって、役割分担と制約が変わる。

# メモ

最初から詳細比較表にしすぎない。
読者が後続章の地図を持てる程度に留める。
