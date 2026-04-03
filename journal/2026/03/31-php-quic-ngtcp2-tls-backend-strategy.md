---
title: "PHP の準公式 QUIC 拡張は ngtcp2 + OpenSSL 本命、GnuTLS 補完で考える"
date: 2026-03-31
tags:
  - php
  - quic
  - http3
  - ngtcp2
  - openssl
  - gnutls
  - tls
  - networking
  - phpinternals
  - oss
  - softwarearchitecture
---

# PHP の準公式 QUIC 拡張は ngtcp2 + OpenSSL 本命、GnuTLS 補完で考える

結論から言うと、PHP の準公式 QUIC 拡張モジュールを今後目指すなら、ngtcp2 を土台にしつつ、TLS backend は **OpenSSL 3.5 以上を本命、GnuTLS を fallback** として考えるのが現実的だと整理している。理由は、macOS や Homebrew では OpenSSL が扱いやすい一方、Linux の LTS 系ではまだ OpenSSL 3.5 が十分に普及しておらず、GnuTLS を外すと対応範囲が狭くなるからだ。

まず重要なのは、ngtcp2 は TLS を全部自前で持つライブラリではないという点だ。QUIC の中には TLS 1.3 が深く組み込まれているが、実際の TLS 処理は backend として外部ライブラリに委譲する。つまり、`libngtcp2` の上に `libngtcp2_crypto_ossl` や `libngtcp2_crypto_gnutls` のような helper を重ねる構造になる。準公式拡張を考えるなら、この時点で「QUIC core」と「TLS backend」を分けて設計する必要がある。

本命候補の OpenSSL は、普及度と開発体験の両面で強い。PHP 利用者にとっても馴染みがあり、macOS では Homebrew 経由で導入しやすい。さらに OpenSSL 3.5 で QUIC API が加わったことで、将来的な標準路線としても見通しが立てやすい。準公式という立場では、「まず多くの開発者が触りやすいか」はかなり大きい。OpenSSL backend を第一候補に置くのは自然だと思う。

ただし、ここで課題が出る。OpenSSL 3.5 はまだ移行途中だ。Debian 12、Ubuntu 24.04、RHEL 9 では 3.0 系が中心で、Debian 13 や Ubuntu 26.04 あたりでようやく 3.5 系が見えてくる。つまり、OpenSSL を本命にしたくても、現時点ではそれだけで Linux の現実を全部は拾えない。ここで GnuTLS fallback が必要になる。Linux ディストリビューションとの整合を考えると、GnuTLS は好みではなく移行期の実務対応として意味がある。

設計としては、`Connection` の中を「QUIC core」と「TLS backend」に分け、backend 差分を `tls_backend_ops` のような抽象化で吸収する形がよさそうだ。これなら OpenSSL を基本路線にしつつ、GnuTLS を補完的に支えやすい。逆に最初から OpenSSL 直結で書くと、後で fallback を足すときに責務分解が崩れやすい。

今の整理としてはこうなる。**開発者体験では OpenSSL、本番の配布現実では GnuTLS fallback**。そして GnuTLS は恒久主役というより、OpenSSL 3.5 普及までの橋渡しだと考えるのがよい。2026年以降に Debian 13 や Ubuntu 26.04 が広がれば、OpenSSL backend を中心にした構成はさらにやりやすくなる。

QUIC 時代の TLS backend 選択は、単なるライブラリ選択ではなく、ディストリビューション戦略でもある。
