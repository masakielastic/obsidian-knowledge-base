---
title: "TC39 Signals と非同期の基礎"
aliases:
  - "TC39 Signals 入門"
  - "Signals と AbortSignal の違い"
tags:
  - javascript
  - tc39
  - signals
  - async
  - abortsignal
  - reactivity
status: "draft"
created: 2026-04-06
updated: 2026-04-06
type: "note"
---

# TC39 Signals と非同期の基礎

## はじめに

TC39 の **Signals** は、JavaScript にリアクティブな状態管理のための共通 primitive を持ち込めるかを検討する提案です。現時点では **Stage 1** で、まだ標準仕様として固まったものではありません。提案の README でも、これは早い段階の提案であり、大規模な実験やプロトタイピングを重視する方針が示されています。

このメモでは、Signals を「非同期処理そのものの仕組み」としてではなく、**非同期処理の結果をどう状態として扱うか** という観点から整理します。

---

## まず結論

Signals を学ぶときは、次の三つを分けて考えるとわかりやすいです。

- **Promise** は、いつか終わる処理の**完了結果**
- **AbortSignal** は、その処理を**中止するための制御信号**
- **TC39 Signals** は、その結果を受けて更新される**状態や派生値を整合的に保つ仕組み**

つまり、

- Promise = completion
- AbortSignal = cancellation control
- Signals = reactive state / readiness

という役割分担です。TC39 Signals の README は、Signals を state や computed を含むリアクティビティ基盤として説明しています。一方で AbortSignal は、非同期操作を中止するための signal object として定義されています。

---

## Signals は何をしたいのか

Signals は、UI フレームワークやリアクティブライブラリで広く使われている「状態セル」と「派生計算」の考え方を、JavaScript の共通基盤として整理できないか、という問題意識から出てきています。README では、フレームワーク間で使える共通の signal モデルを探ること、そして API を急いで固定するのではなく、まずは実験を重視することが述べられています。

ここでいう signal は、雑に言えば次のようなものです。

- ある値を持つ state
- その state から導かれる computed
- 依存元が変わったら、依存先が古くなったことを追跡する仕組み

このため、Signals は「イベントを流す仕組み」よりも、**現在値の整合性を保つ仕組み**として理解したほうが近いです。

---

## 非同期処理とどう関係するのか

Signals は Promise の代わりではありません。

Promise は、`fetch()` のような処理が最終的に成功したか失敗したかを表します。Signals は、その `fetch()` の結果を受けて作られる `user` や `items`、`isLoading`、`visibleCount` のような状態を扱うほうが得意です。TC39 Signals は state/computed のための提案であり、AbortSignal は別に存在する中止制御の仕組みです。

たとえば検索画面なら、役割はこう分けられます。

- `fetch()` の返り値 → Promise
- 途中キャンセル → AbortSignal
- 取得したデータ、表示件数、フィルタ結果 → Signals

つまり Signals は、**非同期 I/O の実行器**ではなく、**非同期 I/O の結果が流れ込む先の状態モデル**です。

---

## Push / Pull で見るとどうか

Signals は、単純な push モデルでも単純な pull モデルでもなく、**push/pull ハイブリッド**として理解するとよいです。

直感的にはこうです。

1. state signal が更新される
2. 依存している computed は「古くなった」とマークされる
3. 実際の値は、次に読まれたときに必要なら再計算される

この意味で、

- 変更通知や無効化は **push**
- 値の確定や再評価は **pull**

と考えると整理しやすいです。proposal 周辺の説明でも、Signals は単なる push 型の Observable ではなく、dirty 管理と lazy な計算を含むモデルとして語られています。

---

## readiness と completion で見る

Signals を非同期の基礎として学ぶときは、**completion** と **readiness** を分けると理解しやすくなります。

### Promise は completion

Promise の中心は、ある仕事が**完了したかどうか**です。

- まだ終わっていない
- 成功で終わった
- 失敗で終わった

という終端が重要です。

### Signals は readiness

Signals の中心は、**今この値が読める状態か、古くなっていないか**です。

- 依存元が変わった
- 派生値は古くなった
- 次に読まれるなら再計算する

つまり Signals は、「仕事が終わったか」よりも「**今の値が妥当か**」に関心があります。README が state と computed を中核に置いているのも、そのためです。

かなり短く言うと、

- Promise は **completion の箱**
- Signals は **readiness のセル**

です。

---

## AbortSignal との違い

名前が似ているので混同しやすいですが、**AbortSignal** と **TC39 Signals** はかなり別物です。

### AbortSignal

AbortSignal は、非同期操作に対して「もう続けるな」と伝えるための signal です。MDN では、`AbortSignal` は `AbortController` と組み合わせて、`fetch` などの操作を abort できる signal object と説明されています。`aborted` や `reason` を持ち、`timeout()` や `any()` のような API もあります。

### TC39 Signals

TC39 Signals は、値・派生値・依存関係のグラフを扱います。関心はキャンセルではなく、**状態の変化と再計算**です。README でも、state や computed を中心にしたモデルとして説明されています。

したがって、実務的にはこう覚えるとよいです。

- AbortSignal = **止めろ** の signal
- TC39 Signals = **値が変わった / 古くなった** の signal

---

## Observable との違い

Signals は Observable とも違います。

Observable は、時間とともに流れてくるイベントや値の列を扱う考え方です。値が複数回流れてくること自体が中心です。これに対して Signals は、値の列よりも **現在値の整合性** を重視します。proposal 周辺の説明でも、Signals は dirty/clean の状態や派生計算を持つ、セル状のリアクティブモデルとして扱われています。

ざっくり言えば、

- Observable = 流れ
- Signal = 現在値

です。

---

## 学ぶ順番

Signals を学ぶときは、次の順番がおすすめです。

## 1. Promise と AbortSignal を分けて理解する

まず、非同期処理には最低でも次の二種類の責務があると押さえます。

- 結果を受ける
- 途中で止める

これは Promise と AbortSignal の分担です。AbortSignal は EventTarget ベースで、Promise とは別の仕組みです。

## 2. 状態管理は別の問題だと理解する

非同期処理の結果を UI に載せると、次の問題が出てきます。

- 今の表示値は最新か
- 派生値は再計算が必要か
- ローディング中か
- 古いリクエスト結果をどう無視するか

このあたりが Signals の得意領域です。

## 3. state と computed の関係を見る

Signals の中心は、単独の値ではなく、

- state
- computed
- dependency tracking

の三つです。ここを押さえると、Signal を「ただの通知オブジェクト」と誤解しにくくなります。 

## 4. framework 依存の API と共通 primitive を分ける

TC39 がやろうとしているのは、特定フレームワークの API を丸ごと標準化することではなく、もっと下の共通 primitive を整理することです。README でも、複数フレームワークで共有できるコアを探る姿勢が示されています。

---

## ありがちな誤解

## 誤解1: Signals は Promise の新しい版である

違います。Promise は完了結果、Signals は状態整合性です。役割が違います。

## 誤解2: Signals は AbortSignal と同じ種類のものだ

違います。AbortSignal は cancellation、TC39 Signals は reactivity です。

## 誤解3: Signals は Observable と同じである

近い部分はありますが、Signals はイベント列よりも現在値と派生値の整合性を中心にしています。

## 誤解4: Signals が標準化されたら非同期処理全体がこれで置き換わる

そうではありません。Signals は主に状態モデルの基盤候補であり、I/O 完了やキャンセルや runtime 制御とは別の責務を持ちます。

---

## 現時点での温度感

Signals 提案は TC39 で **Stage 1** です。Stage 1 は「問題として検討する価値がある」段階であって、すぐに仕様が固まる状態ではありません。proposal の README でも、まだ早期段階であり、フレームワーク横断の実験や検証が必要だとされています。

そのため、Signals を学ぶときは「もう決まった標準 API を覚える」というより、

- 何を解決したいのか
- Promise / AbortSignal / Observable と何が違うのか
- なぜ状態管理の primitive が必要なのか

を押さえるほうが大事です。

---

## まとめ

Signals は、JavaScript の非同期処理を直接置き換える提案ではありません。

むしろ、

- Promise が返した結果を
- AbortSignal で必要なら中止しながら
- Signals で現在値と派生値として整合的に扱う

という責務分解の中で理解すると、位置づけが見えやすくなります。

一言で言えば、

- **Promise は終わり方を表す**
- **AbortSignal は止め方を表す**
- **Signals は今の値の保ち方を表す**

です。

Signals を学ぶ価値は、API の暗記というより、**非同期処理と状態管理の責務境界をきれいに見るためのレンズ**を手に入れられるところにあります。
