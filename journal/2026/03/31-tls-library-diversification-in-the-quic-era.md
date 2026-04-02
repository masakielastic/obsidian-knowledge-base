# TLS ライブラリはなぜ乱立したのか — QUIC 時代に変わった責務の置き場

date: 2026-03-31
tags: #TLS
#QUIC
#HTTP3
#Networking
#Security
#OpenSSL
#BoringSSL
#rustls
#ngtcp2
#SystemDesign
#ProtocolDesign
#SoftwareArchitecture

かつて TLS ライブラリは、かなり長い間「OpenSSL を使っておけばだいたい済む」という世界だった。Apache、Nginx、curl、多くの言語ランタイムが OpenSSL を前提にしていて、TLS はアプリの差別化要素というより、OS やミドルウェアに近い基盤部品だった。ところが TLS 1.3 と QUIC が、この前提を大きく崩した。

転機のひとつは TLS 1.3 だ。ハンドシェイクは簡素化された一方で、0-RTT や key schedule などの導入によって、内部構造はむしろ高度になった。単に「暗号化のスイッチを入れる部品」ではなく、接続開始時の状態管理や鍵導出をかなり精密に扱う必要が出てきた。TLS 実装そのものが、以前よりアプリケーションに近い設計課題になったと言っていい。

さらに大きかったのが QUIC だ。従来は TCP の上に TLS が載り、その上に HTTP が載っていた。けれど QUIC では UDP の上に QUIC transport があり、その内部で TLS handshake を動かし、その上に HTTP/3 が載る。つまり TLS は、外付けの保護層ではなく、トランスポート内部の構成要素になった。ここで TLS ライブラリには、ストリーム前提ではなくパケット前提の世界にどう適応するかが求められるようになった。

この変化が、TLS ライブラリの多様化を生んだ。Google は BoringSSL、Amazon は s2n、Cloudflare 周辺では rustls、Microsoft は Schannel という具合に、各社が自前の実装や強い選好を持つようになった。理由はわかりやすい。性能、API の使いやすさ、セキュリティ設計、そしてメモリ安全性だ。QUIC 時代の TLS は、単なる互換部品ではなく、製品全体の性能特性や実装戦略に直結する。

しかも QUIC では、QUIC ライブラリと TLS ライブラリが組で選ばれやすい。quiche + BoringSSL、quinn + rustls、ngtcp2 + OpenSSL、msquic + Schannel という形だ。QUIC 側が求める API やデータの受け渡し方が違うので、QUIC ライブラリを選ぶと TLS 側もほぼ決まってしまう。ここが HTTP/2 以前よりややこしい。ライブラリ選定が「TLS だけ」「QUIC だけ」で完結しないからだ。

OpenSSL の QUIC 対応が難航した理由も、この構造を見ると理解しやすい。OpenSSL は長年、TCP ストリームの上で TLS record layer を流す設計を前提に進化してきた。だが QUIC では record layer をそのまま使わず、パケット単位で TLS の情報を扱う必要がある。既存 API との互換性を保ちながら、この前提を崩すのはかなり難しい。Google が BoringSSL を作った背景にも、この種の実装自由度の確保がある。

そのため、現在も Web の現場では ngtcp2 + OpenSSL という分業構成がよく使われる。ngtcp2 が QUIC transport、loss recovery、congestion control、stream 管理を担当し、OpenSSL が TLS handshake、key schedule、暗号処理を担当する形だ。Nginx や curl の HTTP/3 実装でも、この分担はかなり象徴的だ。OpenSSL 単独で全部を自然に処理するというより、QUIC transport を外部ライブラリに任せるほうが現実的だったわけだ。

整理すると、TLS ライブラリが乱立しているのは、好みの問題ではない。TLS が OS 的な共通基盤から、QUIC 時代のアプリケーション寄りコンポーネントへ変化したからだ。QUIC は TLS の責務の置き場を変え、実装の前提を変え、結果としてライブラリの多様化を促した。今の群雄割拠は混乱ではなく、責務分解の再編成として見るとかなり納得しやすい。
