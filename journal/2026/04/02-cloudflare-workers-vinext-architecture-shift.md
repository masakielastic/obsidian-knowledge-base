---
title: "Cloudflare Workers ユーザー視点で vinext は Next.js の代替実装として何を変えるのか"
date: 2026-04-02
tags:
  - cloudflare-workers
  - vinext
  - nextjs
  - vite
  - architecture
  - runtime-model
  - reimplementation
---

# Cloudflare Workers ユーザー視点で vinext は Next.js の代替実装として何を変えるのか

Cloudflare の vinext は、「Next.js を Cloudflare Workers へデプロイしやすくするアダプター」ではなく、**Next.js の API surface を Vite 上で再実装する**という発想のプロジェクトだ。Cloudflare の記事では、`app/`、`pages/`、`next.config.js` を大きく崩さず、`next` を `vinext` に置き換えるだけで使い始められる drop-in replacement として紹介されている。`vinext dev`、`vinext build`、`vinext deploy` という形で、開発・ビルド・Workers へのデプロイまでを一続きで扱えるのが特徴だ。
Workers ユーザーにとって重要なのは、ここで解いている問題が単なる「デプロイ手順の面倒さ」ではないことだ。従来の Next.js は独自のビルド出力を持ち、Cloudflare・Netlify・AWS Lambda のような serverless 環境に載せるには、その出力を各環境向けに変形する必要がある。OpenNext はその課題に取り組んでいるが、Cloudflare はこの方式を、Next.js の出力を後追いで reverse-engineer する壊れやすい構造だと整理している。さらに `next dev` は Node.js 前提なので、Workers 固有の実行環境を開発段階からそのまま試しにくい。

vinext の強みは、**Workers を第一級の実行環境として扱いやすい**ことにある。Cloudflare は、開発時もデプロイ時も app 全体が workerd 上で動くため、Durable Objects、KV、AI bindings などの Cloudflare 固有機能を回避策なしで扱えると説明している。これはかなり大きい。Workers を CDN の延長として使うだけなら既存構成でもよいが、Durable Objects や KV をアプリの設計要素として使うなら、「本番だけ Workers、開発時は別物」という構造はつらい。vinext はその差分を減らす。

Cloudflare Workers ユーザー目線では、もう一つの利点は**キャッシュ戦略との相性**だ。vinext は Cloudflare KV 向けの cache handler を用意しており、ISR をそのまま使える。しかも caching layer は差し替え可能で、R2 など別の保存先も選べる設計になっている。Workers で SSR を回しつつ、Cloudflare 側のストレージやキャッシュを自然に組み合わせたいなら、これはかなり筋がよい。単なるホスティングではなく、Cloudflare の基盤機能を前提にした React アプリ運用へ寄せやすい。

ビルド面の数字も無視しにくい。Cloudflare の初期ベンチマークでは、33 ルートの App Router アプリで、Next.js 16.1.6 の Turbopack に対し、vinext は Vite 7 / Rollup で平均 4.64 秒、Vite 8 / Rolldown で平均 1.67 秒、クライアントバンドルも 56〜57% 小さいという結果だった。もちろん Cloudflare 自身の条件付き比較なので一般化はできないが、少なくとも「Workers での実行」だけでなく、「開発ループや成果物サイズ」まで含めて改善余地を示している点は重要だ。

一方で、vinext はまだ experimental だ。Cloudflare 自身が、公開からまだ日が浅く、大規模トラフィックで十分に鍛えられた状態ではないと明記している。ただし、1,700 超の Vitest、380 の Playwright E2E、Next.js 由来のテスト移植、Cloudflare 向け conformance suite を含み、Next.js 16 API surface の 94% をカバーしているとも説明している。つまり、勢いだけのデモではなく、かなりテスト駆動で前に進めているプロジェクトと見たほうがよい。

注意点もある。vinext は ISR には対応しているが、**ビルド時の static pre-rendering は未対応**だ。`generateStaticParams()` で大量ページを事前生成するような設計では、現時点では Next.js の完全な代替ではない。Cloudflare も、100% 静的サイトなら Astro のような Vite 系フレームワークのほうが自然だと書いている。逆に言えば、vinext が狙っているのは「静的サイト全部入り」ではなく、**Workers 上で動的アプリを素直に動かすための Next.js 互換環境**だと理解するとよい。

さらに面白いのは、Cloudflare が Traffic-aware Pre-Rendering という方向を出していることだ。これは、ビルド時に全ページを焼く代わりに、Cloudflare のトラフィック分析を使って、実際に読まれる上位ページだけを deploy 時に pre-render し、残りは on-demand SSR と ISR に任せる考え方だ。Workers を reverse proxy と analytics の両面から持っている Cloudflare らしい発想で、静的生成の常識そのものを Workers 前提で組み替えようとしている。

結論として、Workers ユーザーにとっての vinext は、**Next.js 風の開発体験を保ちながら、実行環境・開発環境・キャッシュ戦略を Cloudflare ネイティブに寄せるための選択肢**だ。静的サイト専用の切り札ではないし、完全互換を最優先する段階でもない。しかし、Durable Objects や KV を使う動的アプリ、あるいは Workers を本気でアプリ基盤として使いたいチームにとっては、かなり注目度の高い実験だと感じる。
