---
title: "stream_select() があるのに、なぜ PHP に Polling API が必要なのか"
date: 2026-03-31
tags:
  - PHP
  - PHPInternals
  - AsyncIO
  - EventLoop
  - Polling
  - StreamSelect
  - ReactPHP
  - AMPHP
  - Revolt
  - libuv
  - libevent
  - SystemsProgramming
  - Networking
---

# stream_select() があるのに、なぜ PHP に Polling API が必要なのか

`stream_select()` があるなら十分では、と思っていた。けれど整理してみると、あれは「待つ」ための古い共通手段ではあっても、現代的な非同期 I/O の土台としては限界がある。今回の Polling API RFC を私は、「新しい派手な機能の追加」ではなく、「PHP の足場をそろえる話」として読むのが重要だと思った。

まず、polling と event loop は同じではない。polling は「どの FD が読み書き可能になったかを OS に問い合わせる仕組み」で、select / poll / epoll / kqueue / port などはその実装差分だ。一方 event loop は、その上で timer、シグナル、タスク再開、キャンセル、Fiber との連携まで扱う実行制御の層になる。つまり Polling API は event loop そのものではなく、その下の共通基盤の話だ。

では `stream_select()` の何がつらいのか。最大の問題は、PHP 側がそこに強く縛られると、OS ごとのよりよい待機機構を共通の形で扱いにくいことだと思う。`select()` 系は昔からあるぶん互換性は高いが、大量の監視対象や効率の面で現代的な要件には向きにくい。非同期 I/O の基盤として考えると、「とりあえず待てる」だけでは足りず、内部 API としても userspace API としても整理が弱い。

この RFC の意義は二つある。第一に internal API の統一だ。OS 差を吸収しながら、PHP 内部で polling を共通化できる。第二に userspace でも価値があることだ。ただし complete event loop を標準搭載する話ではない。この慎重さは大事で、「これで async/await が来る」とか「Node.js みたいになる」と読むのは違う。

ここで面白いのは、PHP には前例があることだ。php-fpm はすでに epoll / kqueue / port / poll / select を切り替えて使える。つまり OS ごとの差を吸収する event mechanism 抽象は、FPM の内部では以前から必要だった。Polling API RFC は、その発想を FPM の外にも広げ、PHP 全体の共有基盤にしようとするものとして理解できる。

だから ext-uv や ext-event がすぐ不要になるわけではない。あちらは polling 以上の機能を持っているし、Revolt / ReactPHP / AMPHP も timer、Fiber、キャンセル、スケジューリングといった高レイヤの責務を担い続ける。ただ、標準で polling の共通基盤があるなら、backend 実装の重複を減らし、責務分割を見直せる可能性がある。価値が消えるのではなく、むしろ境界がきれいになる。

私にとってこの RFC は、「PHP でも非同期をやれるようにする魔法」ではない。そうではなく、非同期 PHP の世界で、みんなが毎回同じ低レイヤを抱え込まなくてよくするための整備だ。地味だが、こういう共通の足場こそ後から効いてくる。
