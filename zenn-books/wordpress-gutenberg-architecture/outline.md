---
title: WordPressアーキテクチャ入門
subtitle: Gutenberg・ブロックテーマ時代を構造で理解する
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
## Gutenberg・ブロックテーマ時代を構造で理解する

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

## 第1章 なぜ Gutenberg 以降の WordPress は説明しにくいのか
- 読者のスタート地点
- PHP と JavaScript の両方が必要な理由
- 従来のテーマ開発知識だけでは足りない理由
- WordPress 実務経験と構造理解のギャップ
- 「プラグインが書ける」と「WordPress を理解している」は同じではない

## 第2章 クラシック WordPress から Gutenberg へ
- テンプレート階層
- `functions.php`
- フックによる拡張
- 「ページを組み立てる CMS」というクラシックな理解
- ブロックという抽象単位
- エディタのアプリ化
- 保存 HTML の位置づけの変化
- 従来テーマとの連続性と断絶

## 第3章 ブロックテーマは何を設計対象にしたのか
- Site Editor
- テンプレートとテンプレートパーツ
- patterns
- `block.json`
- `theme.json`
- 「画面を描く」から「状態空間と制約を設計する」への変化

## 第4章 比較で見る WordPress の位置づけ
- Web Components と比べると何が見えるか
- なぜ Gutenberg は React を必要としたのか
- なぜ WordPress は SPA になりきらないのか
- Headless CMS と WordPress の違い
- HTML を真実とする世界と JSON を真実とする世界
- 比較を WordPress の責務分界へ回収する

## 第5章 Gutenberg を DDD / CQRS 的に読む
- editor state と saved HTML
- Write モデルと Read モデル
- CQRS 的に Gutenberg を再解釈する意味
- Undo / Redo と内部イベント列
- Event Sourcing と似ている点、違う点
- 純粋な Event Sourcing ではなく Snapshot 永続化であること

## 第6章 `block.json` と `theme.json` は何を定義しているのか
- `block.json` を schema / capability / 契約として理解する
- `theme.json` を policy / constraint として理解する
- できることと、やってよいこと
- Command 空間との関係
- editor action との役割分担
- デザインシステムとの接続

## 第7章 テーマ設計はなぜ制約設計なのか
- ブロックテーマ時代のテーマの責務
- UI 設計と制約設計の違い
- ハード制約・ソフト制約・誘導
- patterns の役割
- 壊れない編集環境をどう作るか
- 小規模サイトでの現実解

## 第8章 WordPress でテストするとは何を守ることか
- 何を守るためにテストするのか
- コードではなく契約を守るという発想
- 保存 HTML・編集体験・制約・表示の関係
- pure unit / integration / E2E / manual の役割整理
- テストピラミッドをそのまま使いにくい理由
- E2E の比重が上がる理由
- 実務での最小セット

## 第9章 小規模制作会社の現場から見る WordPress 学習
- 求人票に書かれないが重要な理解
- 「プラグインが書ける」の先にあるもの
- PHP と JavaScript をまたぐ責務分界
- テーマだけのケース
- カスタムブロックありのケース
- Gutenberg 以降に必要な学習の順番

## 第10章 WordPress は古いのか、それとも別の最適化なのか
- 古い / 新しいという見方の限界
- WordPress が最適化してきたもの
- HTML 資産を長期運用するという思想
- 新世代 CMS との違い
- 同じ需要か、重なるが別の需要か
- WordPress を過小評価しないための評価軸

## おわりに
- WordPress を説明できるようになるとは何か
- 断片知識を構造に変える意味
- 次に学ぶべきこと

---

# 章ごとの一行要約

## はじめに
- 本書全体の問題設定と読み方を示す

## 第1章
- Gutenberg 以降の WordPress が説明しにくい理由と、本書が埋めるギャップを示す

## 第2章
- クラシック WordPress から Gutenberg への変化を連続性と断絶の両面から整理する

## 第3章
- ブロックテーマが設計対象をどう変えたかを明らかにする

## 第4章
- 他技術との比較を通じて WordPress の位置づけを理解する

## 第5章
- DDD / CQRS / Event Sourcing を再解釈のレンズとして使い、Gutenberg の構造を読む

## 第6章
- `block.json` と `theme.json` を契約と制約の観点から整理する

## 第7章
- ブロックテーマ時代のテーマ設計を制約設計として再定義する

## 第8章
- WordPress におけるテストを、保存契約・制約・表示資産を守る実務として読み直す

## 第9章
- 制作会社の現場で必要になる学習と責務分界を整理する

## 第10章
- WordPress を「古い CMS」と雑に片付けないための評価軸を示す

## おわりに
- 本書全体のまとめと今後の学習への接続を行う

---

# メモ
- 流れは「現象 → 比較 → 抽象化 → 実務 → 評価」
- DDD は導入ではなく再解釈のレンズとして使う
- WordPress 入門書にはしない
- 制作会社の実務に接続しつつ、求人票より一段深い理解を目指す
- 対話形式では各章を「誤解 → 観察 → 比較 → 抽象化 → 実務」で展開しやすいようにする
