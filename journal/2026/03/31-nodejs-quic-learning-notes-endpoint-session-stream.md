---
title: "Node.js の QUIC 文書をどう読むか — `Endpoint` / `Session` / `Stream` でつかむ学習メモ"
date: 2026-03-31
tags:
  - Nodejs
  - QUIC
  - Networking
  - HTTP3
  - TransportLayer
  - AsyncIO
  - WebDevelopment
  - SoftwareArchitecture
  - LearningNotes
---

# Node.js の QUIC 文書をどう読むか — `Endpoint` / `Session` / `Stream` でつかむ学習メモ

Node.js の `node:quic` 文書は、安定 API の完成版というより、**QUIC をどういう部品に分けて理解するか**を見る教材として読むのがよさそうです。文書冒頭でも `Stability: 1.0 - Early development`、`--experimental-quic` 前提とされています。

まず最初に覚えることは、QUIC は「HTTP/3 のための機能」ではなく、**UDP の上に作られた接続型 transport** だということです。その感覚は、API が `QuicEndpoint`、`QuicSession`、`QuicStream` に分かれていることによく表れています。

**1\. `QuicEndpoint` で学ぶこと**  
`connect()` や `listen()` は毎回新しい endpoint を作れますが、同じ local port に複数 session を載せるために endpoint を共有することもできます。ここで重要なのは、QUIC では「接続の前に UDP の受け口がある」と意識できることです。TCP 的に「ソケット = 接続」と考える癖を外す入口になります。

`endpoint.busy`、`maxConnectionsPerHost`、`maxConnectionsTotal`、`validateAddress`、retry token 系の設定を見ると、QUIC サーバーは request を受ける前から、**入口で負荷制御やアドレス検証を行う**ことがわかります。HTTP レイヤーより一段下の責務を意識する練習になります。

**2\. `QuicSession` で学ぶこと**  
`QuicSession` には `createBidirectionalStream()` と `createUnidirectionalStream()` があります。ここだけ見ると HTTP/2 の session に近いですが、それだけではありません。`onhandshake`、`onversionnegotiation`、`onsessionticket`、`onpathvalidation` があり、QUIC 接続が **TLS handshake・version negotiation・経路確認まで抱える**存在だとわかります。

さらに `sendDatagram()` があるのも重要です。QUIC は stream だけではなく、**unreliable datagram も同居する**設計です。HTTP/2 の延長として見ると見落としやすい部分なので、ここは要チェックです。

**3\. `QuicStream` で学ぶこと**  
`QuicStream` では `direction` が `bidi` / `uni` に分かれ、`onblocked` や `onreset` が用意されています。ここから、stream は単なる読み書きオブジェクトではなく、**個別にフロー制御され、個別に詰まり、個別に reset される論理チャネル**だと理解できます。接続全体の失敗と stream 単位の失敗を分けて考える感覚が大事です。

**4\. Stats で学ぶこと**  
`QuicEndpoint.Stats`、`QuicSession.Stats`、`QuicStream.Stats` が分かれているのもかなり教育的です。特に session stats には `cwnd`、`latestRtt`、`smoothedRtt`、`bytesInFlight`、`datagramsLost` があります。つまり Node.js は QUIC を黒箱にせず、**RTT、輻輳制御、損失回復を観測する transport** として見せています。 

**自分用の読み順メモ**  
最初は `Endpoint` で UDP の受け口を理解し、その次に `Session` で handshake・path・datagram を確認し、最後に `Stream` で多重化と flow control を読むのがよさそうです。Node.js の QUIC 文書は、API リファレンスであると同時に、**QUIC をどの粒度で理解すべきかを示す見取り図**になっています。
