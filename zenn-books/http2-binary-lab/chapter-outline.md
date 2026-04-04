# 実習編アウトライン案

## 00 はじめに――この実習編で何を訓練するのか

**ファイル名:** `00-how-to-use-this-book.md`

### 目的

本編と実習編の役割分担を明確にし、読者の読み方をそろえる章です。

### 内容

- 本編は観点を作る本、実習編は観点を使う本
    
- この教材で一貫して使う観点
    
    - 境界
        
    - 分岐
        
    - 妥当性
        
    - 状態
        
- iterator / parser / validator / state 管理の再確認
    
- この教材の到達目標
    
- 各実習をどう読むか
    
    - まず境界
        
    - 次にヘッダ
        
    - 次に payload
        
    - 最後に妥当性と状態
        

### この章の意義

ここで「仕様を暗記する本ではない」と明言しておくことで、以降の実習がぶれにくくなります。

---

## 第1章　UTF-8 をウォームアップにする

**ファイル名:** `01-utf8-as-a-warmup.md`

### 目的

HTTP/2 に入る前に、可変長データの境界・分岐・妥当性を小さな題材で練習します。

### 内容

- UTF-8 を 1〜4 バイト分類だけで見る限界
    
- 実装上は 9 種類の開始条件を扱う必要があること
    
- 先頭バイトが長さと制約を決めること
    
- parser と validator の分離
    
- 正常系と不正系の比較
    

### 置く実習例

- `41`
    
- `C3 A9`
    
- `E3 81 82`
    
- `F0 9F 98 80`
    
- `E0 9F 80` と `E0 A0 80`
    
- `ED A0 80` と `ED 9F BF`
    
- `F0 8F 80 80` と `F0 90 80 80`
    

### この章の意義

後続の HTTP/2 フレーム読解に入る前に、「長さが合っているだけでは正しくない」という感覚を作れます。

---

## 第2章　HTTP/2 フレームの境界を読む

**ファイル名:** `02-reading-http2-frame-boundaries.md`

### 目的

HTTP/2 フレーム共通ヘッダ 9 バイトを確実に切れるようにします。

### 内容

- 接続プレフェースはフレームではない
    
- 共通ヘッダ 9 バイトの構造
    
- Length / Type / Flags / Stream ID の読み方
    
- まだ payload の意味に深入りしない
    
- 最初の parser が何をすべきか
    

### 置く実習例

- 接続プレフェース
    
- 空の SETTINGS
    
- SETTINGS ACK
    
- 長さ 1 の簡単な例
    
- Stream ID 0 と 1 の比較
    

### この章の意義

「まず境界」という訓練を最も純粋な形で行える章です。

---

## 第3章　最小フレームを読む

**ファイル名:** `03-reading-minimal-frames.md`

### 目的

最小の正常系フレームを使って、ヘッダから payload への入り方を学びます。

### 内容

- DATA の最小例
    
- SETTINGS の最小例
    
- SETTINGS ACK
    
- GOAWAY の最小例
    
- Qiita の最小 HTTP/2 クライアントに出てくる定数の読解
    
- どこまでが共通処理で、どこから種別ごとの処理か
    

### 置く実習例

- Qiita 記事の SETTINGS
    
- Qiita 記事の SETTINGS ACK
    
- Qiita 記事の GOAWAY
    
- 1 バイト payload の DATA
    

### この章の意義

「コード全体」ではなく「中に埋まった定数」を単独で読めるようにする章です。

---

## 第4章　壊れたフレームを読む

**ファイル名:** `04-reading-broken-frames.md`

### 目的

validator の役割を、実際の不正データで理解します。

### 内容

- Length と実データ長の不整合
    
- ACK 付き SETTINGS に payload があるケース
    
- stream 0 と非 0 の文脈
    
- 切り出せることと正しいことの違い
    
- parser と validator の責務分離
    

### 置く実習例

- 長さが長すぎる例
    
- 長さが短すぎる例
    
- 不正な SETTINGS ACK
    
- stream 0 が不適切なフレーム
    
- reserved bit を含む例
    

### この章の意義

本編の「妥当性」を、生データに着地させる中心章です。

---

## 第5章　HEADERS フレームの入口に立つ

**ファイル名:** `05-entering-headers-frame.md`

### 目的

HTTP/2 フレームとしての HEADERS と、その中身としての HPACK を切り分けます。

### 内容

- HEADERS フレーム共通ヘッダ
    
- Flags の読み方
    
- payload の先に HPACK があること
    
- 「ここから先は別レイヤー」という境界
    
- HEADERS を一気に理解しようとしない姿勢
    

### 置く実習例

- Qiita 記事の HEADERS 定数
    
- 先頭 9 バイトだけ読む
    
- 残りを HPACK block fragment として切り出す
    

### この章の意義

フレーム層と圧縮層の責務を混同しないための章です。

---

## 第6章　HPACK の最小例を読む

**ファイル名:** `06-reading-minimal-hpack.md`

### 目的

HPACK の最小読解を行います。ここでは圧縮理論よりも境界と分類を優先します。

### 内容

- static table を使う最小例
    
- literal header の最小例
    
- raw-data と headers の対応
    
- Huffman は深追いしない
    
- HEADERS の先にある別の可変長構造として HPACK を見る
    

### 置く実習例

- static table 中心の最小ケース
    
- literal header 1 個のケース
    
- 単発で読める raw-data
    

### この章の意義

HEADERS を「読めない塊」のままにせず、最小限だけ中へ入れる章です。

---

## 第7章　状態が必要になると何が変わるか

**ファイル名:** `07-when-state-becomes-necessary.md`

### 目的

state の必要性を、HPACK の最小ケースで体験します。

### 内容

- dynamic table の最小導入
    
- 単発では読めないデータ
    
- 前のデータに依存して次の解釈が変わること
    
- state manager の必要性
    
- parser / validator / state 管理の責務差
    

### 置く実習例

- dynamic table 追加 → 次の参照
    
- 同じ raw-data でも前提 state により意味が変わる例
    
- `hpack-test-case` に接続しやすい短い story 形式
    

### この章の意義

本編で扱った「状態」を、抽象論ではなく実データで理解する章です。

---

## 第8章　`http2-frame-test-case` を読む

**ファイル名:** `08-reading-http2-frame-test-case.md`

### 目的

公開 test case を自力で読めるようにします。

### 内容

- `http2-frame-test-case` のリポジトリ構造
    
- JSON のどこを見るか
    
- 正常系か不正系かをどう見抜くか
    
- 何を試しているケースなのかを説明する方法
    
- バイト列の読解からテスト意図の読解へ進む
    

### 置く実習例

- 代表的な正常系
    
- 長さ不整合系
    
- flags / stream の不正系
    

### この章の意義

自作問題から公開資産へ移る、最初の到達点です。

---

## 第9章　`hpack-test-case` を読む

**ファイル名:** `09-reading-hpack-test-case.md`

### 目的

HPACK の公開 test case を読み、state を含むテスト意図を追えるようにします。

### 内容

- `headers` と `raw-data` の対応
    
- story の読み方
    
- static / literal / dynamic の見分け方
    
- 単発ケースと状態依存ケースの違い
    
- decoder 視点で何を確認するか
    

### 置く実習例

- static table 中心のケース
    
- literal を含むケース
    
- dynamic table を含む story
    

### この章の意義

実習編の後半の到達点であり、「状態を持つバイナリ」を公開 test case で読めるようにする章です。

---

## 第10章　ツール・ログ・conformance を読む

**ファイル名:** `10-tools-logs-and-conformance.md`

### 目的

これまで手で読んできたものを、ツールの出力や conformance test の結果と結びつけます。

### 内容

- `nghttp` の verbose 出力の見方
    
- `h2i` で低レベルにフレームを送る発想
    
- `nghttpd` を観測対象として使う意味
    
- `h2spec` の位置づけ
    
- `http2-frame-test-case` / `hpack-test-case` と `h2spec` の役割の違い
    
- `h2spec` の失敗結果をどの層の問題として読むか
    

### この章で扱う対象

- `nghttp`
    
- `h2i`
    
- `nghttpd`
    
- `h2spec`
    

### この章の意義

実習編を、実装読解・ログ読解・適合性検査へ接続する章です。

---

## 第11章　この先どこへ進むか

**ファイル名:** `11-where-to-go-next.md`

### 目的

実習編の内容を、今後の実装読解や教材拡張へつなぎます。

### 内容

- 最小クライアント実装に戻るときの見方
    
- frame parser
    
- payload parser
    
- validator
    
- HPACK decoder
    
- dynamic table manager
    
- connection / stream state
    
- 今後の発展課題
    
    - RST\_STREAM
        
    - GOAWAY
        
    - CONTINUATION
        
    - Sans-IO 的分解
        
    - 最小 HTTP/2 クライアント
        

### この章の意義

本編→実習編→実装・設計へ進む導線を閉じる章です。
