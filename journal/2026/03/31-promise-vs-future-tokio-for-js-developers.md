# Rust/Tokio の Future は Promise と何が違うのか

date: 2026-03-31
tags: #Rust
#Tokio
#AsyncProgramming
#Future
#Promise
#JavaScript
#Concurrency
#EventLoop
#SystemsProgramming
#ProgrammingModel

JavaScript ユーザーとして Tokio を学ぶとき、私はまず **Future は Promise の別名ではない** と整理したほうがよいと思っています。

JavaScript の Promise は、利用者が受け取る「将来わかる値」の API です。`then()` や `await` で結果を待つ側の抽象としてかなり完成されています。これに対して Tokio の glossary では、Future は「ある操作の現在状態を保持する値」であり、`poll` によって処理を進めるものだと説明されています。さらに executor は、その Future を `poll` し続けて実行する側です。

この違いはかなり大きいです。Tokio の async チュートリアルでも、`async fn` を呼んだだけではまだ何も起こらず、返ってくるのは「進行中の非同期計算を内包する値」であり、それを `.await` して初めて進む、と説明されています。Rust の Future は、他言語の「裏で勝手に走るもの」というより、**計算そのもの** です。所有者が polling によって進める責任を持つ、というのが Rust 的な発想です。

ここが JavaScript の Promise 感覚と一番ずれるところです。

JavaScript では、たとえば `fetch()` が返す Promise は、普通はもう処理が始まっています。`await` は開始ボタンではなく完了待ちです。  
一方 Rust/Tokio では、`async fn` を呼ぶと Future は返りますが、その時点では **まだ実行結果は前に進んでいない** と考えたほうが近いです。Tokio の glossary でも、async block や async function が作る Future は、それを spawn するか `.await` しない限り何もしない、と明記されています。

私はこれを次のように覚えると理解しやすいと思います。

- **Promise**: 将来値を扱うための利用者向け抽象
    
- **Future**: runtime に進めてもらう非同期計算
    
- **executor / runtime**: Future を `poll` して進める実行基盤
    
- **task**: Tokio 上で走る実行単位
    

この task も重要です。Tokio の glossary では、task は `tokio::spawn` や `Runtime::block_on` によって runtime 上で動く操作だと説明されています。そして `.await` や `join!` は Future を組み合わせる道具ですが、それだけで新しい task を作るわけではありません。

つまり、JavaScript ユーザー向けにかなり乱暴に言い換えると、

- JS の Promise は「結果オブジェクト」に見えやすい
    
- Rust の Future は「まだ動ききっていない計算」
    
- Tokio の task は「runtime に載せた仕事」
    

という三層構造で見るとわかりやすいです。

たとえば見た目は似ています。

```js
const p \= fetch("/api/data");  
const res \= await p;
```

```rust
let fut \= client.get("/api/data");  
let res \= fut.await;
```

しかし意味は少し違います。JavaScript では `fetch()` の時点で処理は始まっていると考えてよい場面が多いですが、Rust では `client.get()` が返す Future は「そのままではまだ進まない非同期計算」です。`.await` はその Future を現在の task の中で進めて完了を待つ構文です。Rust の `Future` trait 自体も、`poll(self: Pin<&mut Self>, cx: &mut Context) -> Poll<Self::Output>` という形で定義されており、Future が polling される対象であることがはっきり出ています。

さらに Tokio らしさを理解するには `spawn` まで見るとよいです。

```rust
let handle \= tokio::spawn(async {  
    client.get("/api/data").await  
});  

let res \= handle.await?;
```

ここでは `spawn` が task を作り、`JoinHandle` を返します。これは JavaScript の「Promise を作ったら勝手に走る」という感覚より、**runtime に仕事を登録した** という感覚に近いです。

学習メモとしての結論はこうです。

**JavaScript の Promise は、利用者が受け取って待つための抽象として強い。Rust/Tokio の Future は、runtime によって poll される非同期計算そのものとしての性格が強い。**  
そのため Tokio を学ぶときは、Promise との表面的な類似だけで覚えるよりも、**Future・poll・executor・task・await** の関係を先に押さえたほうが、後で `join!`、`select!`、キャンセル、共有状態、`spawn_blocking` の話までつながりやすいです。
