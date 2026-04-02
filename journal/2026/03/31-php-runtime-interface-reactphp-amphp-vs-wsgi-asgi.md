# ReactPHP / AMPHP 時代の PHP と WSGI / ASGI 的な接続規約

date: 2026-03-31
tags: #PHP #ReactPHP #AMPHP #WSGI #ASGI #WebArchitecture #ServerDesign #RuntimeModel #EventLoop #AsyncProgramming #PSR15 #WebFramework #BackendEngineering #SystemDesign #SoftwareArchitecture

Python の WSGI / ASGI を見ていると、PHP にも似たものが必要ではないかと感じる。とくに ReactPHP や AMPHP のように long-running process を前提に PHP アプリを動かそうとすると、その必要性が一気に現実味を帯びる。論点は単なる async 対応ではない。サーバーがアプリをどう呼び出し、アプリがその実行モデルの上でどう生きるか、という接続面の話だ。

WSGI は Python における Web サーバーとアプリケーションの接続規約だ。Django の WSGI アプリ、Flask の WSGI アプリという言い方が自然に成立するのは、HTTP の中身だけでなく、サーバーとアプリの受け渡し方まで共有語彙になっているからだ。ASGI はその async 版というより、WebSocket や長寿命接続、イベント駆動まで扱うために接続規約を拡張したものと捉えるほうが正確だ。WSGI の世界では収まりきらなかった実行モデルを扱うために、接続面の標準がもう一段深くなった。

ここで PHP の PSR-7 / PSR-15 を重ねると混乱しやすい。PSR-7 は HTTP メッセージの形、PSR-15 は middleware / handler の形をそろえる規約であって、WSGI / ASGI と同じ層ではない。こちらはアプリの内側の境界を整える規約であり、サーバーがアプリをどう起動し、どうライフサイクル管理し、どう例外や接続状態をまたぐかまでは標準化していない。つまり PSR-15 があるから WSGI / ASGI 相当がある、とは言えない。

この違いが効いてくるのが ReactPHP や AMPHP の文脈だ。php-fpm + nginx の世界では、リクエストごとに実行して終わる前提が強い。グローバル状態やサービスコンテナも、1 リクエストで消える文化の上なら比較的扱いやすい。ところが event loop の上で常駐させると、メモリ残留、接続共有、例外後の後始末、状態の持ち越しが急に設計課題になる。単に Request と Response の型が合っていれば済む話ではなくなる。サーバーとアプリの間で、どこまでを誰が初期化し、どこまでを誰が掃除し、接続やタスクの寿命をどう切るのかという規約が欲しくなる。

だから ReactPHP / AMPHP を使うときに必要になるのは、「HTTP を受け取って Response を返す関数」だけではない。常駐プロセス上で安全に動かせるアプリの形、言い換えると runtime-aware な接続規約だ。RoadRunner や FrankenPHP でも似た問題は出る。違いは単なるサーバー製品差ではなく、実行モデル差として現れる。FPM では自然に隠れていたライフサイクル問題が、常駐実行では露出するからだ。

もっとも、Python でも WSGI / ASGI があればすべて解決するわけではない。同期コードを async サーバーに載せるにはアダプターが要るし、境界のコストもある。それでも「これは WSGI の話」「これは ASGI の話」と切り分ける共通語彙がある価値は大きい。PHP ではこの層の言葉がまだ弱いため、ReactPHP / AMPHP / FPM / RoadRunner / FrankenPHP の比較が、しばしば道具選びの話と実行モデルの話と運用の話に分裂しやすい。

現時点での実務的な落としどころは、アプリ本体を HTTP サーバーや起動方式からできるだけ分離し、起動コードを薄く保つことだと思う。PSR-7 / PSR-15 はその土台として有効だが、それだけで常駐実行の問題は消えない。今後 PHP で WSGI / ASGI 的な議論をするなら、HTTP メッセージ標準の延長ではなく、SAPI 文化と long-running process 文化の断絶をどう橋渡しするかという、もう一段外側の規約として考える必要がある。そこにこそ、ReactPHP や AMPHP を本格運用するときの本当の難しさがある。
