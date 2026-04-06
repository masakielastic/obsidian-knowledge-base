# 第2冊の企画案

## 書名候補

- **php-fpm の外に出るPHPランタイム入門**
    
- **ReactPHP・Swoole・FrankenPHPの基礎知識**
    
- **PHP非同期ランタイムの責務分解**
    

おすすめのフォルダ名はこれです。

**フォルダ名候補**

- `php-async-runtime-beyond-fpm`
    

---

## この本で扱うべきこと

この本は、  
**「非同期そのもの」** というより  
**「長時間型の PHP 実行モデル」** を扱う本です。

中心論点は次の通りです。

- event loop は何を担当するのか
    
- select / poll / epoll / kqueue とライブラリの関係はどうなっているのか
    
- libevent / libuv は何者か
    
- ReactPHP や AMPHP/Revolt は php-fpm と何が違うのか
    
- PHP-PM のようなプロセスマネージャーは何を担当するのか
    
- Swoole は何を拡張側で抱え込むのか
    
- FrankenPHP は mod\_php の単純復活ではなく、どう新しいのか
    
- embed PHP や php-node のような事例はどう位置づければよいのか
    
- Polling API RFC はどの層の話なのか
    

---

## 第2冊の章立て案

### 00-introduction.md

**はじめに — php-fpm の外に出ると何が増えるのか**

導入です。  
この本は「高速化テクニック集」ではなく、責務がどう増えるかを学ぶ本だと宣言します。

### 01-from-short-lived-to-long-lived.md

**短命リクエスト型から長時間型へ**

php-fpm 的な世界と、長時間生存プロセスの世界の違いを整理。  
第1冊からの接続章として重要です。

### 02-what-an-event-loop-does.md

**event loop は何をしているのか**

I/O 待機、タイマー、コールバック実行の管理。  
この本の中核になる章です。

### 03-select-poll-epoll-kqueue-revisited.md

**select・poll・epoll・kqueue をもう一度見る**

第1冊より一歩踏み込み、  
「多数の待機対象をどう捌くか」という観点で再整理します。

### 04-libevent-and-libuv.md

**libevent と libuv は何者か**

OS の polling API の上に、なぜ中間層が必要になるのか。  
タイマー、シグナル、コールバックキューなどをどう束ねるかを説明します。

### 05-reactphp-basics.md

**ReactPHP 入門 — 1プロセスの中で待ちをさばく**

ReactPHP の責務を整理。  
非同期ランタイムとしての位置づけを説明します。

### 06-amphp-and-revolt.md

**AMPHP / Revolt 入門**

ReactPHP との違いというより、event loop を土台にした別の整理として説明。  
道具比較より責務比較を重視します。

### 07-reactphp-and-php-pm.md

**ReactPHP と PHP-PM は何が違うのか**

ここは独立章推奨です。  
event loop の責務と、worker 管理・再起動・負荷分散の責務を分けて説明します。

### 08-supervisors-and-service-ops.md

**supervisord / systemd など外側の監督者**

長時間型プロセスの運用では、アプリ内の process manager だけでは完結しないことを説明。  
サービス継続性の責務を扱います。

### 09-swoole-as-runtime-extension.md

**Swoole — 拡張で実行基盤を広げる**

ユーザーランドライブラリとは違い、拡張でどこまで土台を持ち上げるのかを整理します。

### 10-polling-api-rfc.md

**Polling API RFC はどの層の話なのか**

event loop そのものではなく、より基礎的な polling 基盤の話として位置づけます。  
internals とユーザーランドの橋渡しの章です。

### 11-frankenphp-and-the-return-of-nearness.md

**FrankenPHP — 「近くで動かす」思想の再編成**

ここで mod\_php の前史が効いてきます。  
単なる復古ではなく、worker mode・app server・embed 的発想の再構成として説明します。

### 12-embed-php-and-php-node.md

**embed PHP と php-node の周辺事例**

PHP を別ホストへ埋め込む発想を短く整理。  
終盤の視野拡張として使えます。

### 13-review-runtime-responsibilities.md

**まとめ — PHP ランタイムの責務を地図として見直す**

最後に全体を回収。  
I/O 待機、イベントループ、worker 管理、サービス監督、サーバー統合の責務を再配置します。

### 14-license.md

**ライセンス**
