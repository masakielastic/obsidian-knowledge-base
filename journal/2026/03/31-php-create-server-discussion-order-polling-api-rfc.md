---
title: "PHP で `createServer()` を議論するなら、どんな順番がよいのか"
date: 2026-03-31
tags:
  - PHP
  - PHPInternals
  - PHPRFC
  - PollingAPI
  - AsyncProgramming
  - EventLoop
  - NetworkProgramming
  - HTTPServer
  - ServerDesign
  - SoftwareArchitecture
  - RuntimeDesign
---

# PHP で `createServer()` を議論するなら、どんな順番がよいのか
— Polling API RFC を起点に考える

PHP で Node.js の `createServer()` のような API を議論するとき、最初から高水準の入口関数を設計しようとすると論点が散らばりやすい。調べてみると、先に見るべきなのは HTTP サーバー API そのものではなく、**Polling API RFC が何を提供しようとしているか**でした。

Polling API RFC は、完成した event loop や HTTP サーバー API を入れる提案ではありません。主眼は、PHP core・拡張・userspace が共有できる polling 基盤を整えることにあります。`stream_select()` の限界を補い、epoll や kqueue などの backend を統一的に扱えるようにする方向です。

この前提に立つと、`createServer()` の議論は次の順番で進めるのが自然です。

**1\. polling primitive**  
まずは複数 I/O をどう待つか。HTTP 専用ではなく、async framework や拡張の内部実装にも関わる共通基盤の話です。

**2\. handle abstraction**  
次に、何を poll 対象として統一的に扱うのか。RFC では `StreamPollHandle` を起点に、将来は socket や curl なども視野に入っています。

**3\. server primitive**  
その後で、listen、accept、connection lifecycle、timeout など、サーバーを書くための lower-level primitive を整理する。

**4\. high-level API**  
最後に、その上に `createServer()` のような convenience API を置くべきか評価する。

Node.js の `http.createServer()` は便利な入口に見えますが、実際には `http.Server` を返すサーバー実行モデルの入口であり、request handling や timeout、keep-alive など広い責務を背負っています。
だから PHP でも、いきなり同じ形を目指すより、まず公共部品をどの順で整備するかを決めるほうがよいはずです。

現時点の整理としては、  
**polling → handle → server primitive → `createServer()`**  
の順番で議論するのが、もっとも空中戦になりにくいと思います。
