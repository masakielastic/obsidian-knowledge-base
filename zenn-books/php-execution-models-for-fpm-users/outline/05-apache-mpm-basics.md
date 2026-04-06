---
title: Apache MPM 入門
status: draft
type: chapter
order: 5
book: php-execution-models-for-fpm-users
tags:
  - apache
  - mpm
  - prefork
  - worker
  - event
---

# この章の問い

prefork・worker・event は、どの層の何を切り替える仕組みなのか。

# この章で扱うこと

- Apache MPM の基本
- prefork / worker / event の違い
- Web サーバー側の並行処理モデルという見方
- PHP 側の実行モデルとの違い

# この章の核心

Apache MPM は Web サーバー側の並行処理の話であり、
PHP の event loop や非同期ランタイムの話とは別レイヤーである。

# メモ

「event という名前でも ReactPHP の event loop とは違う」を意識して書く。
