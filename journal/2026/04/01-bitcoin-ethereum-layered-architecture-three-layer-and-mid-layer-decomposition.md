---
title: "Bitcoin / Ethereum をレイヤーで見る学習メモ — 3層の地図と、中レイヤーを分解する意味"
date: 2026-04-01
tags:
  - Bitcoin
  - Ethereum
  - BlockchainArchitecture
  - LayeredArchitecture
  - ProtocolDesign
  - DistributedSystems
  - APIDesign
  - DeveloperEcosystem
  - SystemDesign
  - SoftwareArchitecture
---

# Bitcoin / Ethereum をレイヤーで見る学習メモ — 3層の地図と、中レイヤーを分解する意味

暗号資産の技術を理解しようとすると、つい「Bitcoin」「Ethereum」「ノード」「ウォレット」「SDK」「RPC」と個別の名前で覚えがちです。けれども実務や設計の観点では、名前よりも**どの責務がどの層にあるか**で見たほうがわかりやすい。

最初は HTTP/2 をもとに、低レイヤー・中レイヤー・高レイヤーの3層で考えていた。この見方は Bitcoin / Ethereum にも十分使える。ただし、暗号資産は HTTP/2 よりも責務が多いので、3層だけだと中レイヤーが厚くなりすぎる。そこで、**まず3層で全体像をつかみ、そのあと中レイヤーを分解する** という二段構えが有効だと感じた。

まずアプリ視点の3層で整理するとこうなる。

- 低レイヤー: 暗号、署名、ハッシュ、シリアライズ、データ構造 primitive
    
- 中レイヤー: 検証、状態遷移、mempool、同期、コンセンサス、ノード機能
    
- 高レイヤー: SDK、ウォレット、dApp、取引所、分析基盤、業務アプリ
    

この3層の良いところは、**低だけでも高だけでもエコシステムは育ちにくく、中レイヤーが多様性を支える** ことが見える点にある。低レイヤーだけだと職人向けの部品集になり、高レイヤーだけだと完成品依存になりやすい。その間にある中レイヤーが、複数の用途を支える共通基盤になる。

ただし Bitcoin / Ethereum では、この「中レイヤー」の中身が広すぎる。たとえば Ethereum なら、EVM 実行、状態遷移、JSON-RPC、Provider / Signer / Contract abstraction が全部「中」に入ってしまう。これでは設計の議論には粗すぎる。

そこで、クライアント・サーバー視点で中を分けると整理しやすい。たとえば次の6層くらいで考えられる。

1. アプリ層  
    ウォレット、dApp、取引所、監視、会計、分析基盤
    
2. クライアント抽象層  
    SDK、Provider、Signer、Contract abstraction、ABI ラッパー
    
3. 通信境界層  
    RPC client、WebSocket client、request/response のマッピング
    
4. サーバー公開 API 層  
    JSON-RPC、CLI、subscription、各種外部向け API
    
5. サーバー中核ロジック層  
    検証、mempool、状態遷移、同期、ブロック処理、コンセンサス連携
    
6. primitive 層  
    署名、ハッシュ、シリアライズ、Script、opcode、trie など
    

この分け方をすると、Bitcoin と Ethereum の違いも見えやすくなる。

Bitcoin では、低い層に SHA-256、ECDSA / Schnorr、トランザクション形式、ブロック形式、Script、Merkle tree、P2P メッセージがある。その上で、UTXO 検証、mempool、ブロック検証、チェーン追従、peer 管理がノードの中核になる。さらにその外側に Bitcoin Core の RPC や通知機構があり、その上にウォレット、決済連携、エクスプローラが乗る。

Ethereum では、低い層に Keccak、署名、RLP、opcode、state trie がある。その上で、EVM 実行、gas 処理、state transition、mempool、block import などが中核になる。さらに JSON-RPC や各種 API が公開境界になり、その上に ethers.js のような SDK が Provider / Signer / Contract という形で再抽象化を行い、その先に dApp やウォレットがある。

この見方の重要な点は、**RPC は単なる高レイヤー API ではなく、中レイヤーを外部へ公開する境界** だということだ。さらに SDK は単なる便利ライブラリではなく、**高レイヤー開発者向けに中レイヤーを再抽象化する装置** と考えたほうがしっくりくる。

この整理は curl と比較するとさらに理解しやすい。生の socket や TLS だけでは多くの開発者は困るし、完成アプリだけでも自由度が足りない。libcurl のような中レイヤーがあるから、その上に CLI、各種言語バインディング、業務ツール、アプリが育つ。Bitcoin / Ethereum でも同じで、ノード内部、中レイヤー API、SDK 抽象の設計が、エコシステムの厚みを左右する。

結局のところ、暗号資産の学習で大事なのは、個別の製品名や流行語を追うことだけではなく、**どこが primitive で、どこが共通基盤で、どこがアプリの自由度を支える層なのか** を見分けることだと思う。  
3層は入門の地図として有効で、より深く考えるなら中レイヤーを分解する。Bitcoin / Ethereum をそういう視点で見ると、技術の理解だけでなく、開発者エコシステムの見え方もかなり変わってくる。
