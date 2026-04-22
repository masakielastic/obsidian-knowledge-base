---
title: WordPressアーキテクチャ入門
subtitle: Gutenberg・ブロックテーマ・Headless・DDD・テストで読み解く
status: draft
type: book-outline
tags:
  - wordpress
  - gutenberg
  - block-theme
  - architecture
  - ddd
  - testing
  - zenn-book
---

# WordPressアーキテクチャ入門
## Gutenberg・ブロックテーマ・Headless・DDD・テストで読み解く

## この本の狙い
- WordPress の使い方ではなく、設計思想とアーキテクチャを理解する
- Gutenberg 以降の WordPress を、PHP / JavaScript / CMS / テストの横断で説明できるようにする
- WordPress を「古い CMS」としてではなく、別の最適化を持つシステムとして再評価する
- 小規模制作会社や受託開発の現場で、今後必要になる構造理解への橋をかける

## 想定読者
- WordPress のテーマやプラグインに少し触れたことがある
- PHP と JavaScript の基本構文は読める
- MVC や DDD という言葉に抵抗がない
- ただし Gutenberg 以降の WordPress を構造として説明できない

## この本で扱わないこと
- WordPress 完全初心者向けの操作説明
- テーマ開発の手順そのもの
- プラグイン開発入門
- DDD の一般理論の詳説
- React や Web Components の入門チュートリアル

---

# 目次

## はじめに
- なぜいま WordPress をアーキテクチャとして学ぶのか
- 「使える」と「説明できる」の違い
- この本の読み方

## 第1章 この本の前提
- 読者のスタート地点
- PHP と JavaScript の両方が必要な理由
- WordPress 実務経験と構造理解のギャップ
- 求人要件とアーキテクチャ理解の距離

## 第2章 なぜ Gutenberg 以降の WordPress は説明しにくいのか
- 従来のテーマ開発知識だけでは足りない
- PHP だけでは見えないもの
- JavaScript だけでは見えないもの
- 「プラグインが書ける」と「WordPress を理解している」は同じではない

## 第3章 クラシック WordPress のメンタルモデル
- テンプレート階層
- `functions.php`
- フックによる拡張
- 投稿・固定ページ・テンプレートの関係
- 「ページを組み立てる CMS」という理解
- クラシックテーマ時代の責務分界

## 第4章 Gutenberg は何を変えたのか
- ブロックという抽象単位
- エディタのアプリ化
- コンテンツと UI 構造の接近
- 保存 HTML の位置づけの変化
- 従来テーマとの断絶と連続性

## 第5章 ブロックテーマは何を追加したのか
- Site Editor
- テンプレートとテンプレートパーツ
- patterns
- `block.json`
- `theme.json`
- 「画面を描く」から「状態空間を設計する」への変化

## 第6章 Web Components と比べると何が見えるか
- Custom Elements
- Shadow DOM
- HTML Template
- ブロックとの対応関係
- なぜ WordPress は Shadow DOM を採らないのか
- 「閉じた部品」ではなく「開いた部品」としてのブロック

## 第7章 なぜ Gutenberg は React を必要としたのか
- 状態管理の必要性
- 再描画と UI 更新
- editor state の重要性
- エディタアプリとしての要件
- React による編集体験と HTML 保存のハイブリッド構造

## 第8章 なぜ WordPress は SPA になりきらないのか
- HTML を資産として扱う思想
- 長期互換性
- SEO と配信の都合
- JavaScript 依存を避ける理由
- 編集は SPA、公開は HTML という構造

## 第9章 Headless CMS と WordPress の違い
- JSON を真実とする世界
- HTML を真実とする世界
- Write 最適化と Read 最適化
- 表示責任をどこに置くか
- Headless WordPress の利点と限界
- EmDash のような新世代 CMS をどう見るか

## 第10章 Gutenberg を CQRS 的に読む
- editor state と saved HTML
- Write モデルと Read モデル
- WordPress に CQRS の視点を持ち込む意味
- ただし純粋な CQRS ではない理由
- 固定 Projection 型としての理解

## 第11章 Event Sourcing 的に見ると何が見えるか
- Undo / Redo
- 内部イベント列
- 編集状態と状態遷移
- Event Sourcing との類似
- ただし永続化は Snapshot である
- Event Sourcing 風 UI としての Gutenberg

## 第12章 `block.json` は何者なのか
- Command ではない理由
- schema / capability / 契約としての理解
- ブロック型の定義
- できることを定義する仕組み
- editor action との役割分担

## 第13章 `theme.json` は何者なのか
- policy / constraint としての理解
- グローバルスタイルと制約
- できることと、やっていいこと
- Command 空間を制御する仕組み
- デザインシステムとの接続

## 第14章 テーマ設計は UI 設計ではなく制約設計である
- ブロックテーマ時代のテーマの責務
- ハード制約・ソフト制約・誘導
- patterns の役割
- `theme.json` の設計方針
- 壊れない編集環境をどう作るか
- 小規模サイトでの現実解

## 第15章 WordPress でテストするとはどういうことか
- 何を守るためにテストするのか
- コードではなく契約を守るという発想
- 保存 HTML・編集体験・制約・表示の関係
- DDD 的に見たテスト対象の整理
- WordPress 固有のテストの難しさ

## 第16章 テストピラミッドを WordPress 向けに読み替える
- なぜ古典的なピラミッドをそのまま使いにくいのか
- pure unit / integration / E2E / manual
- 境界の契約を守るという視点
- ダイヤモンド型・砂時計型としての理解
- E2E の比重が上がる理由

## 第17章 ブロックテーマ時代の現実的なテスト配分
- テーマだけのケース
- カスタムブロックありのケース
- 受託サイトのケース
- Theme Unit Test Data の位置づけ
- Playwright の意味
- 実務での最小セット

## 第18章 小規模制作会社の現場から見る WordPress 学習
- 求人票に書かれないが重要な理解
- 「プラグインが書ける」の先にあるもの
- PHP と JavaScript をまたぐ責務分界
- Gutenberg 以降に必要な学習の順番
- 将来に向けた見取り図としてのアーキテクチャ理解

## 第19章 WordPress は古いのか、それとも別の最適化なのか
- 古い / 新しいという見方の限界
- WordPress が最適化してきたもの
- EmDash など新世代 CMS との違い
- 同じ需要か、重なるが別の需要か
- WordPress を過小評価しないための整理

## おわりに
- WordPress を説明できるようになるとは何か
- 断片知識を構造に変える意味
- 次に学ぶべきこと

---

# 章ごとの一行要約

## はじめに
- 本書全体の問題設定と読み方を示す

## 第1章
- 読者の前提と、この本が埋めるギャップを明確にする

## 第2章
- Gutenberg 以降の WordPress が説明しにくい理由を整理する

## 第3章
- クラシック WordPress の設計前提を確認する

## 第4章
- Gutenberg が導入した変化の本質を押さえる

## 第5章
- ブロックテーマが WordPress の責務をどう変えたかを確認する

## 第6章
- Web Components との比較でブロックの特徴を明らかにする

## 第7章
- なぜ React が必要だったのかを状態管理の観点から説明する

## 第8章
- WordPress が完全な SPA にならない理由を整理する

## 第9章
- Headless CMS との比較で WordPress の立ち位置を明確にする

## 第10章
- CQRS 的に Gutenberg を再解釈する

## 第11章
- Event Sourcing 的に editor state を読み直す

## 第12章
- `block.json` を契約・能力定義として理解する

## 第13章
- `theme.json` を制約・方針定義として理解する

## 第14章
- ブロックテーマ時代のテーマ設計を制約設計として再定義する

## 第15章
- WordPress におけるテストの意味を再定義する

## 第16章
- テストピラミッドを WordPress 向けに読み替える

## 第17章
- 実務で使える現実的なテスト配分を示す

## 第18章
- 制作会社の現場とアーキテクチャ理解の距離を埋める

## 第19章
- WordPress を「古い」と切って捨てないための評価軸を示す

## おわりに
- 本書全体のまとめと今後の学習への接続を行う

---

# メモ
- 流れは「現象 → 比較 → 抽象化 → 実務 → 評価」
- DDD は導入ではなく再解釈のレンズとして使う
- WordPress 入門書にはしない
- 制作会社の実務に接続しつつ、求人票より一段深い理解を目指す
- 対話形式にする場合は各章を「誤解 → 観察 → 比較 → 抽象化 → 実務」で展開しやすい
