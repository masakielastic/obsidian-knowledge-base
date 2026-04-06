# 第1冊の企画案

## 書名候補

- **PHP-FPMユーザーのための実行モデル基礎**
    
- **CGI・mod\_php・php-fpmで学ぶPHP実行環境**
    
- **php-fpmの前提を学び直す本**
    

英語寄りのフォルダ名と合わせるなら、扱いやすいのはこれです。

**フォルダ名候補**

- `php-execution-models-for-fpm-users`
    

---

## この本で扱うべきこと

この本は、非同期ランタイムの本ではありません。  
まずは **Web サーバーと PHP の接続形態を理解する本** として設計したほうが安定します。

中心になる論点は次の通りです。

- CGI は何がつらかったのか
    
- mod\_php はなぜ便利だったのか
    
- mod\_php はなぜ主流でなくなったのか
    
- Apache MPM はどの層の話なのか
    
- FastCGI / php-fpm は何を分離したのか
    
- php-fpm ユーザーが最低限知っておくべきプロセス・スレッド・接続待機の基礎は何か
    

つまり、**歴史の説明そのものが設計比較になる本**です。

---

## 第1冊の章立て案

### 00-introduction.md

**はじめに — PHP はいつも同じ形で動いているわけではない**

導入です。  
読者に「PHP の知識」と「PHP の実行形態の知識」は別だと伝える章です。  
SAPI という見取り図を早めに出します。

### 01-php-is-not-always-the-same.md

**PHP はどう実行されるのか — SAPI の見取り図**

`apache2handler`、`cgi-fcgi`、`fpm-fcgi`、`cli`、`embed` をざっくり整理。  
この本全体の地図になる章です。

### 02-why-cgi-became-a-problem.md

**CGI は何がつらかったのか**

1リクエストごとにプロセスを起動するモデルの分かりやすさと限界を説明。  
FastCGI が必要になった背景をつくる章です。

### 03-why-mod-php-was-powerful.md

**mod\_php はなぜ強かったのか**

Apache の中で PHP を動かすことの自然さ、当時の実用性、導入のしやすさ。  
ここでは単なる旧方式として切らず、ちゃんと強みを書くのが重要です。

### 04-why-mod-php-faded.md

**mod\_php はなぜ主流でなくなったのか**

責務分離、運用、MPM、スレッドまわり、サーバー構成の変化。  
FrankenPHP を後で理解するための前史にもなります。

### 05-apache-mpm-basics.md

**Apache MPM 入門 — prefork・worker・event**

ここは Web サーバー側の並行処理モデルの話。  
PHP 側の実行モデルとは別レイヤーだと明示する章です。

### 06-what-fastcgi-changed.md

**FastCGI は何を変えたのか**

CGI の問題に対する改善として FastCGI を整理。  
「PHP を外に分ける」発想の意味を説明します。

### 07-how-php-fpm-thinks.md

**php-fpm は何を担当しているのか**

プロセス管理、プール管理、Web サーバーとの役割分担。  
php-fpm ユーザーが実務で持つべき基礎知識を置く章です。

### 08-processes-threads-and-connections.md

**プロセス・スレッド・接続の基礎**

ここで OS やサーバー側の並行処理の感覚を整理。  
シングルスレッド、マルチスレッド、プロセス分離の話を入れます。

### 09-select-poll-epoll-kqueue-intro.md

**select・poll・epoll・kqueue への入口**

ここでは詳細実装に入りすぎず、  
「多数の待機をどう扱うか」という感覚だけつくります。  
第2冊への橋渡しです。

### 10-review-why-fpm-made-sense.md

**まとめ — なぜ php-fpm は自然な選択だったのか**

全体を回収する章。  
CGI、mod\_php、Apache MPM、FastCGI、php-fpm がどうつながるかを再整理します。

### 12-license.md

**ライセンス**

---

## 第1冊の狙い

この本は、読者に async ライブラリを使わせることが目的ではありません。  
むしろ、

**「php-fpm が主流になったのは偶然ではなく、実行モデル上の整理の結果だった」**

と理解してもらうのが目的です。  
これがわかると、第2冊で ReactPHP や FrankenPHP を読んだときに、比較の軸ができます。
