---
title: プロセス・スレッド・接続の基礎
status: draft
type: chapter
order: 8
book: php-execution-models-for-fpm-users
tags:
  - process
  - thread
  - connection
  - concurrency
---

# この章の問い

php-fpm や Apache MPM を理解するには、プロセス・スレッド・接続をどう見ればよいのか。

# この章で扱うこと

- プロセスとスレッドの違い
- シングルスレッドとマルチスレッド
- 同時接続の見方
- keep-alive の基本感覚

# この章の核心

PHP の実行モデルを理解するには、
コードの書き方だけでなく、処理単位がどこで分かれているかを見る必要がある。

# メモ

OS 教科書的に広げすぎない。
php-fpm / Apache / 後続の event loop 入門に必要な範囲に絞る。
