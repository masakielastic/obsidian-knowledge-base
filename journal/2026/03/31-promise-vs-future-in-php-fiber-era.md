---
title: "PHP ユーザー視点では Promise と Future は何が違うのか"
date: 2026-03-31
tags:
  - PHP
  - AsyncProgramming
  - Promise
  - Future
  - Fiber
  - API-Design
  - Concurrency
  - EventLoop
  - LibraryDesign
  - SoftwareArchitecture
---

# PHP ユーザー視点では Promise と Future は何が違うのか

PHP の非同期を考えるとき、私はまず **Fiber と Promise / Future を同じ棚に置かない** ようにしています。添付メモでも、両者を競合ではなく別レイヤーとして整理し、さらに eager 実行、cancel、責務混在、PHP の実行モデルの違いを押さえる構成になっていました。

学習の要点は次の通りです。

まず、**Future は利用者が受け取る将来値**です。まだ結果は出ていないが、あとで成功か失敗かが確定する値のハンドルです。  
一方で、**Promise や Deferred は実装側が完了させる側**です。結果を resolve / reject する責務はこちらにあります。  
JavaScript では Promise が広く普及したため、この二つの役割が一つのオブジェクトに統合されて見えやすいですが、設計としては分けて考えたほうが理解しやすいです。

次に、**Fiber は結果の箱ではなく実行機構**です。Fiber は処理を suspend / resume する仕組みであって、Future のように「将来値」を表す抽象ではありません。  
そのため Fiber だけでは、完了通知、失敗、合成、timeout、cancel までをきれいに扱う公開 API にはなりません。  
私の理解では、**Fiber はエンジン、Future は利用者向け API**です。

ここで JavaScript の Promise をそのまま PHP に持ち込むと混乱しやすい理由が出てきます。

1つ目は **eager 実行**です。JavaScript の Promise は通常、生成した時点で処理が始まります。`then` や `await` は開始命令ではなく、待機や購読です。これはブラウザや Node.js の文化では自然ですが、PHP のライブラリ設計では「作っただけで走り出す」ことが必ずしも望ましくありません。

2つ目は **cancel の置き場**です。Promise が結果オブジェクトなのに、そこへ cancel を直接持たせると、「値の表現」と「実行の制御」が混ざります。JavaScript が AbortController のような外部トークンに寄せたのは、この分離の都合が大きいはずです。PHP でも CancellationToken 的な分離のほうが筋がよいと思います。

3つ目は **責務混在**です。利用者が受け取る側と、実装者が完了させる側を同じ型に押し込むと、公開 API が曖昧になります。PHP では Future / Deferred / Task を分けたほうが、ライブラリ利用者にも実装者にも説明しやすいです。

4つ目は **ランタイム事情の違い**です。PHP はブラウザ内 JS と同じではありません。CLI の長寿命プロセス、ワーカー、HTTP クライアント、DB、ストリーム、拡張モジュールなど、前提がかなり多様です。だから JS の歴史的に合理的だった設計を、そのまま標準解として移植するのは危ういです。

私なら、PHP での整理はこう考えます。

- **内部実装**: Fiber + Scheduler / EventLoop
    
- **公開 API**: Future / Task
    
- **完了させる側**: Deferred / Promise
    
- **補助要素**: CancellationToken、timeout、`all()`、`race()`
    

擬似コードで書くなら、利用者にはこう見せたいです。

```php
$future = $client->get("https://example.com");  
$response = await($future);
```

実装側はこうです。

```php
$deferred = new Deferred();  
  
$scheduler->spawn(function () use ($deferred) {  
    try {  
        $deferred->resolve(fetch());  
    } catch (Throwable $e) {  
        $deferred->reject($e);  
    }  
});  
  
return $deferred->future();
```

この形なら、**Fiber を内部に閉じ込めつつ、利用者には将来値としての Future を見せられる**ので、責務がかなり整理されます。

まとめると、Promise と Future の違いは名前の好みではなく、**誰がその値を受け取り、誰がそれを完了させるのか**という責務の違いです。Fiber はその土台になる実行機構ですが、Future の代わりではありません。  
PHP で非同期 API を考えるなら、JavaScript の Promise を真似るより、**Fiber を実装に使い、公開面は Future / Task / CancellationToken で分けて設計する**ほうが、長く保守しやすいと思います。
