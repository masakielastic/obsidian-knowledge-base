---
title: "PHP のビルトインサーバー改善構想 — 全面改修ではなく、実行可能な分割線を先につくる"
date: 2026-03-31
tags:
  - php
  - phpinternals
  - webserver
  - refactoring
  - architecture
  - http
  - networking
  - async
  - eventloop
  - tls
---

# PHP のビルトインサーバー改善構想 — 全面改修ではなく、実行可能な分割線を先につくる

PHP のビルトインサーバー改善計画を短く整理しておく。今回の構想は `php_cli_server.c` を一気に作り直す話ではない。主眼は、`http_parser` 依存の緩和、TLS 対応に向けた Stream API 化の準備、そして Polling API を見据えた event loop/backend 分離に耐えられる構造へ、段階的に変えていくことにある。

現状の課題は、イベントループ、request parse callback、response/send 系、SAPI 層が強く結びついていて、どこか一つを直すだけでも影響範囲が大きいことだ。そこで先に必要なのは全面改修ではなく、責務の分割と交換可能な境界の整備になる。狙っているのは file split 自体ではなく、parser backend、request builder、response builder / sender、poll backend、transport backend、SAPI bridge のような差し替え点を作ることだ。

内部構造の目標像も比較的明確で、`poll` はイベントループ、`client` は接続単位の管理、`request_builder` は parser callback の受け皿、`request` は完成済みリクエスト、`parser_*` は parser backend、`response` は応答の意味内容、`sender` は partial write や chunk 処理、`sapi` は SAPI 接続面、`log` はログや補助処理、という役割分担を想定している。巨大な一枚岩の処理を、変更軸ごとに分けていくイメージだ。

進め方としては、最初から `llhttp` 置換や TLS 対応を入れるのではなく、まず no behavior change の小さな分割から始める。最初の段階では `poll`、`log`、utility のような独立度の高い部分を切り出し、その後に `request_builder` を導入して parser callback 直結の構造をほどく。ここが重要で、`http_parser` から将来別の parser に移行したいなら、先に callback 依存の密結合を崩さなければならない。

response 側も同様で、status line や header 構築のような意味内容を持つ層と、実際の送信処理を担う層を分ける必要がある。さらにその先で transport abstraction を導入し、accept/read/write/close を transport 層にまとめる。最初は socket transport のみを対象にし、stream transport や TLS は後続段階で扱う想定だ。Polling API も同じで、いきなり機能追加するのではなく、backend を交換できる構造を先に整える。

要するに、この改善構想の中心は新機能追加ではない。parser、transport、poll、SAPI が絡み合った現状を、upstream に説明しやすい小さな PR に分解しながらほどいていくことにある。高水準 API を急いで議論する前に、まず差し替え可能な内部構造を育てる。その順番が、この計画のいちばん重要なポイントだ。
