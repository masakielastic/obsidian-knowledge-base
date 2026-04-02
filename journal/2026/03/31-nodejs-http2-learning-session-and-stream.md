# Node.js の `http2` モジュールで HTTP/2 を学ぶ

— **`Http2Session` と `Http2Stream` を入口に、「接続の上の複数通信」を理解する**

date: 2026-03-31
tags: #NodeJS
#HTTP2
#Networking
#Backend
#WebProtocols
#AsyncProgramming
#EventDriven
#Streams
#SoftwareArchitecture
#TechNotes

HTTP/2 を学びたいとき、私は最初から仕様書のフレーム一覧に突っ込むより、Node.js の `http2` モジュールを見るのがよいと思っています。理由は単純で、HTTP/2 の核心である「1本の接続の上に複数のストリームが載る」という構造が、API の形にかなり素直に現れているからです。添付の方針でも、`Http2Session` と `Http2Stream` を入口に、接続と個別通信を分けて理解することが重視されていました。

HTTP/1.1 の感覚だと、どうしても request/response を1回ごとの往復として見てしまいます。keep-alive があっても、その感覚はかなり残ります。でも HTTP/2 は違います。まず接続全体があり、その上で複数の会話が同時進行する。私はここを「道路が1本で、その上を複数の車線の通信が流れる」と考えるようにしています。重要なのは、HTTP/2 を「速い HTTP」ではなく、「接続とストリームの設計」として見ることです。

Node.js の `http2` モジュールでは、この構造が `Http2Session` と `Http2Stream` に分かれています。Session は接続全体の器、Stream はその中を流れる個別の通信です。だから私は、入門時には Compatibility API より Core API を先に見たほうがよいと感じます。互換 API は便利ですが、HTTP/1 に似た書き味になるぶん、HTTP/2 固有の構造が見えにくくなるからです。

最初に注目したいのは、サーバー側の `stream` イベントです。

```js
server.on('stream', (stream, headers) => {  
  stream.respond({ ':status': 200 });  
  stream.end('hello');  
});
```

このコードを私は「リクエストを受けた」と読むより、「新しい stream が開かれた」と読むようにしています。`headers` には `:path`、`:method`、`:authority`、`:scheme` のような pseudo-header が入り、HTTP/2 が従来と少し違う形でメタデータを持っていることも見えてきます。

さらに `Http2Stream` を見ると、HTTP/2 の本体がかなりわかります。`respond()` はヘッダー送信、`write()` と `end()` はデータ送信と終了です。`close`、`error`、`aborted` などがストリーム単位で起きるので、「1つの通信が終わっても、接続全体はまだ生きている」という感覚が自然に身につきます。しかも `Http2Stream` は `Duplex` として扱えるので、読み書き可能な個別チャネルとして理解しやすいです。

一方で `Http2Session` 側を見ると、`connect`、`close`、`error`、`goaway`、settings など、接続全体の lifecycle が見えてきます。私はここで初めて、「接続全体の都合」と「個別ストリームの都合」は別物なのだと腹落ちしました。HTTP/2 を理解するとは、この分離を理解することでもあります。

結局、Node.js の `http2` モジュールは、単なる API ではなく HTTP/2 の教材です。入門としては、接続、ストリーム、多重化、pseudo-header、ストリーム単位の終了、Core API と Compatibility API の違いが見えれば十分です。その先にフレーム、フロー制御、HPACK、gRPC、HTTP/3/QUIC が続きます。まずは `stream` イベントを追いながら、「1本の接続の上に複数の通信が同時にある」という構造をつかむ。それが HTTP/2 学習のよい入口だと思います。
