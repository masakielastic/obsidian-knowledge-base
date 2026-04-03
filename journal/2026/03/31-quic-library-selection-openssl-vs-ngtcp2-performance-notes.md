---
title: "QUIC ライブラリの選び方メモ：OpenSSL 単独と ngtcp2 をどう使い分けるか"
date: 2026-03-31
tags:
  - QUIC
  - HTTP3
  - OpenSSL
  - ngtcp2
  - Networking
  - Performance
  - TLS
  - AsyncIO
  - SystemDesign
  - LearningNotes
---

# QUIC ライブラリの選び方メモ：OpenSSL 単独と ngtcp2 をどう使い分けるか

QUIC を学ぶとき、ライブラリ選びは「何を理解したいか」でかなり変わる。整理して腑に落ちたのは、OpenSSL 単独と ngtcp2 は、同じ QUIC 対応でも役割がかなり違うということだ。Debian のパッケージ事情を見ると、その違いがかなりはっきり見える。Debian 12 では ngtcp2 系は GnuTLS バックエンド中心だが、Debian 13 では ngtcp2 の OpenSSL バックエンドも加わり、比較しながら学びやすくなっている。

学習目線で言えば、OpenSSL 単独は「最短で QUIC 接続を触る」ための道具で、ngtcp2 は「QUIC トランスポートの中身を学ぶ」ための道具だと考えるとわかりやすい。OpenSSL の QUIC API は libssl の延長で扱えるので、接続や暗号の手触りをつかみやすい。一方 ngtcp2 は、ACK、輻輳制御、ストリーム多重化、送受信制御といった QUIC 本体の論点を正面から観察しやすい。curl の HTTP/3 サポートでも、非実験扱いなのは ngtcp2 バックエンドだけで、OpenSSL-QUIC は experimental とされている。

ここで重要なのが、OpenSSL 単独には学習用としてのわかりやすさがある一方、性能面では課題があることだ。curl 開発者の説明では、OpenSSL-QUIC は ngtcp2 より同条件で概ね 2 倍前後遅く、アップロードでは 2〜4 倍差が出ることがある。さらにメモリ使用量も大きく、テストによっては ngtcp2 の 20〜25 倍に達すると報告されている。

しかも問題は単なる速度差だけではない。curl 側は、OpenSSL の QUIC API が十分に poll しやすい形ではなかったため、しばらく busy-loop 回避が未解決課題になっていた。2025年1月には修正が入ったものの、curl 開発者はその後も OpenSSL QUIC の性能を “abysmal” と表現しており、性能改善そのものは十分ではなかったとしている。つまり OpenSSL 単独は「とりあえず触る」には便利でも、性能検証や本格運用の基盤として見ると慎重さが必要だ。

この整理をすると、学習順序も見えやすい。1周目は OpenSSL 単独で QUIC 接続の雰囲気をつかむ。2周目で ngtcp2 に進み、QUIC トランスポートの内部を見る。Debian 13 以降なら、さらに ngtcp2 + GnuTLS と ngtcp2 + OpenSSL 系 API 対応ライブラリの比較に進める。つまり OpenSSL 単独は入門用の近道としては有力だが、性能まで含めて QUIC 実装を理解したいなら ngtcp2 系を避けて通りにくい。

要するに、最短で「つながった」を味わうなら OpenSSL 単独、QUIC を本気で理解するなら ngtcp2、性能も含めて現実を見るなら OpenSSL 単独の弱点を最初から意識しておく、という整理になる。学習の入口としては OpenSSL 単独は魅力的だが、そこで見えた世界が QUIC 実装の全体像だと思い込まないことが大事そうだ。
