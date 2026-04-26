---  
title: 画像生成プロンプト公開サイト 企画・仕様メモ  
created: 2026-04-26  
type: project-spec  
tags:  
  - prompt-management  
  - image-generation  
  - obsidian  
  - astro  
  - cloudflare-workers  
  - cloudflare-r2  
status: draft  
---  
  
# 画像生成プロンプト公開サイト 企画・仕様メモ  
  
## 1. 目的  
  
画像生成AIで作成した公開用画像とプロンプトを、後から探しやすく、再利用しやすい形で管理・公開する。  
  
このサイトは、単なる作品ギャラリーではなく、画像生成AIの試行錯誤から得られた「制作知」を蓄積するための個人用アーカイブである。  
  
公開サイトに載せることで、作業履歴や保存場所を覚え続けなくてもよい状態を作る。  
  
「あの画像がお気に入りだったけれど、プロンプトはどこにあったか」という状況になったとき、探す場所をこのサイトひとつに絞る。  
  
## 2. 背景  
  
AI以前は、一定期間に作成できる作品数が自然に限られていたため、作品管理は大きな問題になりにくかった。  
  
しかし画像生成AIでは、短時間で大量の画像候補が生まれる。問題は「作れるか」ではなく、「どれを残すか」「どう見直すか」「どう再利用するか」に移っている。  
  
そのため、作品管理そのものが制作能力の一部になっている。  
  
従来のブログや note.com は、解説記事を読ませるには向いているが、短時間で大量の画像を見直す用途には文章がノイズになりやすい。  
  
必要なのは、長い記事中心のサイトではなく、サムネイル・タグ・短いメモ・プロンプト本文を中心とした、視覚的なカード型データベースである。  
  
## 3. 基本方針  
  
管理対象は、公開する画像と公開するプロンプトに絞る。  
  
非公開画像、作業中画像、原本画像、試行錯誤中の素材は、従来どおり Google Drive で管理する。  
  
Cloudflare R2 には、公開してよい WebP 画像のみを置く。  
  
Obsidian には、公開するプロンプト記事だけを置く。  
  
GitHub では、Obsidian の Markdown 記事と Astro プロジェクトを管理する。  
  
Astro で静的サイトを生成し、Cloudflare Workers にデプロイする。  
  
## 4. このサイトで解決したい問題  
  
### 4.1 探す場所の分散  
  
画像生成AIの作業では、保存場所が分散しやすい。  
  
- Google Drive  
- ローカルフォルダ  
- Obsidian  
- ChatGPT / Grok / Gemini などの履歴  
- note.com  
- SNS  
- ダウンロードフォルダ  
  
この状態では、あとから画像やプロンプトを探すコストが高くなる。  
  
公開サイトを「確定済みインデックス」として使うことで、公開済み・再利用したいものはこのサイトを見る、というルールにする。  
  
### 4.2 解説記事が見直しのノイズになる問題  
  
note.com のような記事サービスでは、導入文、背景説明、本文、まとめが自然に増える。  
  
しかし画像生成プロンプトの見直しでは、まず必要なのは次の情報である。  
  
- 画像の雰囲気  
- プロンプト本文  
- モデル名  
- タグ  
- 再利用できるか  
- 短いメモ  
  
したがって、長文記事よりもカード型の一覧と短い詳細ページを重視する。  
  
### 4.3 生成物が量産される問題  
  
画像生成AIでは、候補画像が大量に生まれる。  
  
すべてを管理対象にすると破綻するため、公開する価値があるものだけを選別してサイトに載せる。  
  
公開サイトに載っていないものは、未整理、作業中、または捨ててよいものとして扱う。  
  
## 5. サイトの役割  
  
このサイトは次の役割を持つ。  
  
1\. 公開画像のギャラリー  
2\. プロンプト本文の保管場所  
3\. 画像からプロンプトへ戻るための索引  
4\. タグ・月別アーカイブによる検索補助  
5\. 自分の制作判断を確定する場所  
6\. 将来的なプロンプト管理アプリの原型  
  
## 6. 非目的  
  
このサイトでは、以下を初期段階の目的にしない。  
  
- 非公開画像の管理  
- 作業中画像の管理  
- 原本画像の保存  
- 生成履歴の完全な再現  
- すべてのボツ画像の保存  
- 複雑なユーザー管理  
- SNS 的な交流機能  
- 署名付きURLによる非公開画像配信  
- ブラウザー内での画像変換  
- 高度な全文検索  
  
初期段階では、公開する画像とプロンプトだけを扱う。  
  
## 7. 全体構成  
  
```
Obsidian  
  公開するプロンプト記事を書く  
  
GitHub  
  Markdown と Astro プロジェクトを管理する  
  
Astro  
  Markdown / frontmatter を読み取り、静的サイトを生成する  
  
Cloudflare Workers  
  Astro で生成した公開サイトを配信する  
  
Cloudflare R2  
  公開用 WebP 画像とサムネイルを保存する  
  
Google Drive  
  非公開画像、作業中画像、原本画像、採用前の素材を保存する
```

## 8\. ドメイン構成

### 公開サイト

prompts.example.com

または

prompt.example.com

公開プロンプト集としては `prompts.example.com` が自然。

管理アプリ寄りにするなら `prompt.example.com` もよい。

### 画像配信

assets.example.com

Cloudflare R2 の公開バケットにカスタムドメインを割り当てる。

画像URLの例:

https://assets.example.com/images/2026/04/001.webp  
https://assets.example.com/thumbs/2026/04/001.webp

## 9\. コンテンツ構造

Obsidian / Astro プロジェクトの構成案。

prompt-site/  
  src/  
    content/  
      prompts/  
        2026/  
          04/  
            001-fuji-couple-grid.md  
            002-red-blazer-fashion.md  
            003-hair-medium-layered.md  
  
    pages/  
      index.astro  
      prompts/  
        \[slug\].astro  
      tags/  
        \[tag\].astro  
      archive/  
        \[year\]/  
          \[month\].astro  
  
    layouts/  
      PromptLayout.astro  
  
    components/  
      PromptCard.astro  
      PromptGallery.astro  
      TagList.astro  
  
    content.config.ts  
  
  public/  
    favicon.svg  
  
  .obsidian/  
  astro.config.mjs  
  wrangler.jsonc  
  package.json

## 10\. ファイル命名規則

### Markdown

月単位でフォルダを分ける。

src/content/prompts/2026/04/001-fuji-couple-grid.md  
src/content/prompts/2026/04/002-red-blazer-fashion.md

ファイル名は次の形式にする。

NNN-short-slug.md

例:

001-fuji-couple-grid.md  
002-red-blazer-fashion.md  
003-hair-medium-layered.md

### 方針

- 秒単位の日時はファイル名に入れない
    
- 月単位で見直しやすくする
    
- 連番は月内の登録順とする
    
- 途中挿入のために番号を詰め直さない
    
- 正確な生成日時が必要な場合は frontmatter に入れる
    

## 11\. R2 の画像構成

原本は保存しない。

公開用 WebP 画像のみを R2 に置く。

images/  
  2026/  
    04/  
      001.webp  
      002.webp  
  
thumbs/  
  2026/  
    04/  
      001.webp  
      002.webp

複数画像がある場合:

images/  
  2026/  
    04/  
      001-01.webp  
      001-02.webp  
      001-03.webp  
  
thumbs/  
  2026/  
    04/  
      001-01.webp  
      001-02.webp  
      001-03.webp

### 画像形式

公開画像は WebP に統一する。

images/2026/04/001.webp  
thumbs/2026/04/001.webp

### 原本の扱い

原本画像は R2 に保存しない。

理由:

- プロンプトがあればある程度再生成できる
    
- 原本まで残すと管理が複雑になる
    
- 公開サイトでは軽量な WebP 画像だけあればよい
    
- R2 は公開用キャッシュ兼アセット置き場として扱う
    

非公開画像や原本画像は Google Drive に残す。

## 12\. frontmatter 仕様

### 単一画像の場合

YAML

\---  
id: 2026-04-001  
title: Fuji couple grid  
slug: fuji-couple-grid  
created: 2026-04-26  
updated: 2026-04-26  
provider: xai  
model: grok-imagine-image  
tags:  
  - portrait  
  - couple  
  - fujifilm  
  - grid  
thumbnail\_key: thumbs/2026/04/001.webp  
image\_key: images/2026/04/001.webp  
reuse: high  
\---

### 複数画像の場合

YAML

\---  
id: 2026-04-001  
title: Fuji couple grid  
slug: fuji-couple-grid  
created: 2026-04-26  
updated: 2026-04-26  
provider: xai  
model: grok-imagine-image  
tags:  
  - portrait  
  - couple  
  - fujifilm  
  - grid  
images:  
  - key: images/2026/04/001-01.webp  
    thumb: thumbs/2026/04/001-01.webp  
  - key: images/2026/04/001-02.webp  
    thumb: thumbs/2026/04/001-02.webp  
reuse: high  
\---

## 13\. frontmatter フィールド一覧

### 必須

| フィールド | 内容 |
| --- | --- |
| `id` | 月単位の管理ID |
| `title` | 表示タイトル |
| `slug` | 公開URL用スラッグ |
| `created` | 作成日 |
| `provider` | 生成サービス提供元 |
| `model` | 使用モデル |
| `tags` | タグ配列 |
| `thumbnail_key` または `images[].thumb` | サムネイル画像 |
| `image_key` または `images[].key` | 表示画像 |

### 任意

| フィールド | 内容 |
| --- | --- |
| `updated` | 更新日 |
| `reuse` | 再利用度 |
| `note` | 短いメモ |
| `source` | 元になったプロンプトや記事 |
| `category` | 大分類 |
| `visibility` | 公開状態。ただし初期は公開前提なので省略可 |

## 14\. プロンプト記事の本文構成

個別ページは長文記事にしない。

最小構成:

Markdown

\## Prompt  
  
ここにプロンプト本文を書く。  
  
\## Memo  
  
短いメモを書く。  
何がよかったか、再利用時の注意点、失敗しやすい点など。

必要に応じて追加:

Markdown

\## Variation  
  
派生プロンプトや改善案を書く。  
  
\## Notes  
  
詳細な検討メモを書く。

## 15\. サイト画面

### 15.1 トップページ

目的:

- 最近追加したプロンプトを一覧する
    
- 画像から記憶を呼び戻す
    

表示内容:

- サムネイルグリッド
    
- タイトル
    
- タグ
    
- モデル名
    
- 作成月
    

### 15.2 ギャラリーページ

目的:

- サムネイル中心で画像を探す
    
- note.com 的な長文ノイズを避ける
    

表示内容:

- 画像グリッド
    
- タグフィルタ
    
- 月別フィルタ
    
- モデルフィルタ
    

初期段階では静的なタグリンクで十分。

### 15.3 プロンプト詳細ページ

目的:

- 画像からプロンプトへ戻る
    
- 再利用に必要な情報を確認する
    

表示内容:

- メイン画像
    
- タイトル
    
- タグ
    
- モデル名
    
- プロンプト本文
    
- 短いメモ
    
- 関連タグへのリンク
    

### 15.4 タグページ

URL 例:

/tags/fujifilm/  
/tags/portrait/  
/tags/grid/

目的:

- 用途や表現から探す
    

表示内容:

- 該当タグを持つプロンプトカード一覧
    

### 15.5 月別アーカイブ

URL 例:

/archive/2026/04/

目的:

- 月単位で作業を見直す
    
- ファイル構成とサイト構成を対応させる
    

表示内容:

- その月に追加したプロンプト一覧
    
- サムネイルグリッド
    

## 16\. URL 設計

### プロンプト詳細ページ

/prompts/fuji-couple-grid/

または、月別構造を URL に含める場合:

/prompts/2026/04/fuji-couple-grid/

初期案としては、月別アーカイブと詳細ページの両方を活かすため、次がよい。

/prompts/2026/04/fuji-couple-grid/

### タグページ

/tags/fujifilm/

### 月別アーカイブ

/archive/2026/04/

### 画像URL

https://assets.example.com/images/2026/04/001.webp  
https://assets.example.com/thumbs/2026/04/001.webp

## 17\. 画像変換パイプライン

公開画像は WebP に変換する。

CLI 前提で変換する。

処理の流れ:

1\. Google Drive またはローカル作業フォルダから採用画像を選ぶ  
2\. CLI で WebP に変換する  
3\. サムネイルを生成する  
4\. R2 にアップロードする  
5\. Obsidian の frontmatter に image\_key / thumbnail\_key を記録する  
6\. Git にコミットする  
7\. Astro でビルドする  
8\. Cloudflare Workers にデプロイする

例:

input.png  
  -> images/2026/04/001.webp  
  -> thumbs/2026/04/001.webp

## 18\. CLI ツールの初期仕様

将来的に用意したい CLI。

### 目的

- 画像を WebP に変換する
    
- サムネイルを生成する
    
- R2 にアップロードする
    
- Markdown frontmatter の作成や更新を補助する
    

### コマンド案

Bash

prompt-publish add image.png \--id 2026\-04-001

実行結果:

R2:  
  images/2026/04/001.webp  
  thumbs/2026/04/001.webp  
  
Markdown:  
  image\_key: images/2026/04/001.webp  
  thumbnail\_key: thumbs/2026/04/001.webp

複数画像:

Bash

prompt-publish add image1.png image2.png \--id 2026\-04-001

実行結果:

images/2026/04/001-01.webp  
images/2026/04/001-02.webp  
thumbs/2026/04/001-01.webp  
thumbs/2026/04/001-02.webp

## 19\. Astro 側の仕様

### Content Collections

Astro Content Collections を使い、frontmatter の型を検証する。

検証したい項目:

- `id` が存在する
    
- `title` が存在する
    
- `slug` が存在する
    
- `tags` が配列である
    
- `created` が日付である
    
- 画像キーが存在する
    
- `provider` と `model` が存在する
    

### 画像URLの組み立て

Markdown にはフルURLを書かない。

frontmatter には R2 object key だけを書く。

Astro 側で以下のように変換する。

ASSET\_BASE\_URL + "/" + image\_key

例:

https://assets.example.com/images/2026/04/001.webp

### 環境変数

ASSET\_BASE\_URL=https://assets.example.com

## 20\. 検索について

初期段階では高度な検索は実装しない。

まずは次で対応する。

- タグページ
    
- 月別アーカイブ
    
- サムネイル一覧
    
- ブラウザーのページ内検索
    
- Astro で生成した静的一覧
    

必要になったら、後から Pagefind などの静的検索を検討する。

## 21\. 公開判定ルール

R2 にアップロードする画像は、公開してよい画像のみとする。

Git 管理下に置く Markdown は、公開してよいプロンプト記事のみとする。

非公開のものは Google Drive に置く。

判断ルール:

R2 にある画像  
  = 公開してよい画像  
  
Git にあるプロンプト記事  
  = 公開してよいプロンプト  
  
Google Drive にある画像  
  = 非公開、作業中、原本、未整理

この単純なルールにより、公開事故を減らす。

## 22\. キャッシュ方針

初期段階では、ブラウザー側の IndexedDB キャッシュは実装しない。

まずは次で対応する。

- WebP 化
    
- サムネイル分離
    
- R2 + Cloudflare の通常配信
    
- 長めの Cache-Control
    

将来的に必要であれば、Service Worker + Cache API を検討する。

IndexedDB は、オフライン検索やローカルメタデータ管理が必要になった場合に検討する。

## 23\. 設計思想

### 23.1 すべてを管理しない

AI時代は生成物が大量に生まれるため、すべてを保存・分類しようとすると破綻する。

管理対象は、公開する価値があるものに絞る。

### 23.2 画像は検索補助である

画像は単なる成果物ではなく、プロンプトを探すための視覚的な索引である。

サムネイル一覧を見ることで、過去の作業をすばやく思い出せるようにする。

### 23.3 公開サイトは忘れるための仕組みである

公開サイトに載せたものは、探す場所が確定する。

これにより、Google Drive、Obsidian、チャット履歴、ローカルフォルダを毎回探し回らなくてよくなる。

管理の目的は、覚え続けることではなく、覚えなくてもよい状態を作ることである。

### 23.4 解説と索引を分ける

画像生成プロンプトの見直しでは、長い解説記事はノイズになりやすい。

まずはカード型の索引を作る。

まとまった考察や読み物は、別の記事として扱う。

## 24\. 今後の拡張候補

### 24.1 静的検索

- Pagefind
    
- タグ横断検索
    
- モデル別検索
    
- 月別検索
    

### 24.2 CLI 強化

- Markdown テンプレート生成
    
- R2 アップロード
    
- WebP 変換
    
- サムネイル生成
    
- frontmatter 自動更新
    
- 連番採番
    

### 24.3 Obsidian 連携

- テンプレート整備
    
- Dataview による一覧
    
- R2 画像プレビュー補助
    
- 必要なら専用プラグイン
    

### 24.4 ギャラリー機能

- タグフィルタ
    
- モデルフィルタ
    
- 月別フィルタ
    
- 再利用度フィルタ
    
- 類似プロンプト表示
    

### 24.5 将来的な Web アプリ化

- Workers API
    
- D1 によるメタデータ管理
    
- R2 画像管理
    
- ブラウザー拡張機能
    
- MCP / コーディングエージェント連携
    

## 25\. 初期実装の優先順位

### Phase 1: 最小公開サイト

- Astro プロジェクト作成
    
- Content Collections 設定
    
- プロンプト詳細ページ作成
    
- トップページのサムネイルグリッド
    
- R2 画像URL表示
    
- Cloudflare Workers デプロイ
    

### Phase 2: 探しやすさの改善

- タグページ
    
- 月別アーカイブ
    
- モデル別一覧
    
- カード表示の改善
    

### Phase 3: 運用自動化

- WebP 変換 CLI
    
- R2 アップロード CLI
    
- Markdown テンプレート生成
    
- 連番採番
    
- frontmatter 更新
    

### Phase 4: 検索と拡張

- 静的検索
    
- 関連プロンプト
    
- ギャラリー表示強化
    
- Obsidian 連携強化
    

## 26\. 最小構成の完成条件

最初の完成条件は以下とする。

- Obsidian でプロンプト記事を書ける
    
- Markdown は Git 管理されている
    
- 公開用 WebP 画像が R2 にある
    
- Astro でトップページと詳細ページが生成される
    
- Cloudflare Workers で公開される
    
- 画像からプロンプト本文へ戻れる
    
- タグまたは月別で最低限探せる
    

## 27\. まとめ

このサイトは、画像生成AI時代の制作物を管理するための個人ホームページである。

従来のブログや SNS では、プロンプトと画像を組み合わせて、短時間で見直し、再利用する用途に十分対応しづらい。

そのため、Obsidian、Astro、Cloudflare Workers、Cloudflare R2 を組み合わせて、自分の制作知を管理するための小さな公開サイトを作る。

重要なのは、すべてを保存することではない。

公開する価値のある画像とプロンプトだけを選び、見直しやすい形で残すことである。

公開サイトは、人に見せる場所であると同時に、自分が作業履歴を忘れるためのインデックスでもある。
