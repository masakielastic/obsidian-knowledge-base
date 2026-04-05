# Promise 再学習 ― 結果待機・失敗・キャンセルを整理する

# ファイル名候補

 * 01-why-relearn-promise.md
 * 02-what-promise-does.md
 * 03-what-promise-does-not-do.md
 * 04-what-abort-controller-and-signal-are.md
 * 05-cancellation-is-responsibility-propagation.md
 * 06-rethinking-all-race-timeout-and-retry.md
 * 07-broken-promise-designs-that-still-work.md
 * 08-review-promise-signal-and-scope.md

# 01章　なぜ Promise を学び直すのか

ファイル名: `01-why-relearn-promise.md`

## この章の目的

- Promise を毎日使っていても、責務は混線しやすいことを示す
    
- この小冊子の中心テーマを明示する
    

## 扱う内容

- `await` は書けるが、Promise の責務は説明しにくい
    
- Promise が非同期処理そのものに見えやすい
    
- 実務では「動く」だけでは足りない
    
- この本では Promise を結果待機・失敗伝播・キャンセル・責任スコープの地図で学び直す
    

## この章の核心

- Promise を API としてではなく責務として見直す必要がある
    

---

# 02章　Promise は何を引き受けているのか

ファイル名: `02-what-promise-does.md`

## この章の目的

- Promise の守備範囲を明確にする
    

## 扱う内容

- Promise は将来値として見る
    
- 成功 / 失敗の流れを統一的に扱う
    
- `then` / `catch` / `await`
    
- `Promise.try()` は何者か
    
- `Promise.withResolvers()` は何者か
    
- Promise は「結果待機の窓口」を整える道具である
    

## 整理したい論点

- `try...catch` と `Promise.try()` の違い
    
- `new Promise(...)` と `Promise.withResolvers()` の違い
    
- Promise は結果待機と失敗伝播には強い
    

## この章の核心

- Promise は結果待機と失敗伝播の中心だが、責務はそこに限定される
    

---

# 03章　Promise は何を引き受けていないのか

ファイル名: `03-what-promise-does-not-do.md`

## この章の目的

- Promise の限界を明示する
    

## 扱う内容

- Promise はキャンセルを単独で完結しない
    
- Promise は timeout / retry / cleanup の責務を持たない
    
- Promise は責任スコープそのものではない
    
- `Promise.all` / `Promise.allSettled` は何をまとめるのか
    
- 結果集約と責任スコープの違い
    

## 整理したい論点

- `Promise.all` は待機集約
    
- `Promise.allSettled` は成功 / 失敗を保持した待機集約
    
- どちらも強い責任スコープまでは表さない
    

## この章の核心

- Promise は強力だが、責任スコープや中止伝播まで一人では担えない
    

---

# 04章　AbortController / AbortSignal は何者か

ファイル名: `04-what-abort-controller-and-signal-are.md`

## この章の目的

- AbortSignal の棚を Promise と分ける
    

## 扱う内容

- AbortSignal は結果ではない
    
- 中止の意思を表す
    
- 中止の伝播経路に関わる
    
- `fetch` と signal
    
- Promise と Signal は別軸
    
- `Promise.withResolvers()` との対比
    
- `Promise.try()` との対比
    

## 整理したい論点

- Promise は結果待機
    
- AbortSignal は中止伝播
    
- 似た便利APIに見えても責務が違う
    

## この章の核心

- AbortSignal は Promise の代用品ではなく、中止の責務を扱う別の道具である
    

---

# 05章　キャンセルは責任の伝播である

ファイル名: `05-cancellation-is-responsibility-propagation.md`

## この章の目的

- キャンセルを「止める命令」以上のものとして再定義する
    

## 扱う内容

- 親が不要と判断したら、子へどう伝えるか
    
- signal を下位関数へ渡す意味
    
- timeout と手動キャンセルの競合
    
- cleanup の必要性
    
- 「止めたつもり」と「実際に止まる」の違い
    

## 整理したい論点

- キャンセルは責任の伝播
    
- cleanup は別責務
    
- signal を受け取らない下位関数は責任の線を切る
    

## この章の核心

- キャンセル設計は所有関係と伝播経路の設計である
    

---

# 06章　Promise.all・race・timeout・retry を責務で見直す

ファイル名: `06-rethinking-all-race-timeout-and-retry.md`

## この章の目的

- よく使う道具を責務マップに戻す
    

## 扱う内容

- `Promise.all` は何をするか
    
- `Promise.allSettled` は何をするか
    
- `Promise.race` は何をするか
    
- timeout は制御ポリシー
    
- retry は失敗処理ポリシー
    
- signal と組み合わせると何が起きるか
    
- どこまでを関数に押し込んでよいか
    

## 整理したい論点

- 待機集約
    
- 成功 / 失敗の観測
    
- 制御ポリシー
    
- 後始末
    
- 中止伝播
    

## この章の核心

- 似た場所に出てくる道具でも、待機集約・制御ポリシー・中止伝播は別責務である
    

---

# 07章　動くのに壊れる Promise 設計

ファイル名: `07-broken-promise-designs-that-still-work.md`

## この章の目的

- ケーススタディで責務混線を読む
    

## 扱う内容

- signal を受け取らない下位関数
    
- timeout だけあって cleanup がない
    
- retry がキャンセルを無視する
    
- `Promise.all` でまとめたつもりが責任スコープになっていない
    
- UI 側はキャンセルしたつもりだが通信は続いている
    
- `withResolvers()` で窓口を作ったが責任設計は空のまま
    
- `Promise.try()` で失敗を Promise 化したが cleanup は別問題
    

## 整理したい論点

- 動くことと責任が正しいことは別
    
- Promise 周辺の破綻は責務の誤配置として読める
    

## この章の核心

- Promise 設計の破綻は、便利API不足ではなく責務混線として起きる
    

---

# 08章　復習 ― Promise・Signal・責任スコープをつなぎ直す

ファイル名: `08-review-promise-signal-and-scope.md`

## この章の目的

- 小冊子全体を一枚の地図に戻す
    

## 扱う内容

- Promise は何を担当するか
    
- AbortSignal は何を担当するか
    
- timeout / retry / cleanup はどこに置くか
    
- `all` / `allSettled` / `race` の棚
    
- 持ち帰る問い
    

## 最後に置くチェックリスト

- これは結果待機か
    
- これは失敗伝播か
    
- これは中止伝播か
    
- これは制御ポリシーか
    
- これは cleanup か
    
- これは責任スコープを作っているか
    

## この章の核心

- Promise の再学習とは、便利な書き方を覚え直すことではなく、責務を置き直すことである
    

---

## 補助ファイルの内容案

### `book-overview.md`

- 小冊子の目的
    
- 想定読者
    
- 既刊本との違い
    
- この本で得られること
    
- この本で扱わないこと
    

### `promise-book-outline.md`

- 各章の中心命題
    
- 前後のつながり
    
- 章ごとの主役概念
    

### `writing-guidelines-promise-book.md`

- Promise を過小評価も過大評価もしない
    
- AbortSignal を万能停止APIとして書かない
    
- timeout / retry / cleanup / signal を混ぜない
    
- コード例より責務説明を優先する
    
- 実務例を出しても最後は一般化して回収する
