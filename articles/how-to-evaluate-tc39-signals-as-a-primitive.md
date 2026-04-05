---
title: "TC39 Signals を primitive としてどう評価するか"
aliases:
  - "Signals 評価軸"
  - "TC39 Signals primitive メモ"
tags:
  - javascript
  - tc39
  - signals
  - reactivity
  - design
status: "draft"
created: 2026-04-06
updated: 2026-04-06
type: "note"
---

# TC39 Signals を primitive としてどう評価するか

## このメモの目的

TC39 Signals proposal を読むとき、単に「便利そうな API かどうか」で判断すると、本質を見失いやすい。

この提案は、完成済みの高級 API を配るよりも、**複数フレームワークの下に敷ける shared primitive を切り出せるか**を検証する性格が強い。README でも、共通の developer-facing API より **underlying signal graph の precise core semantics** を重視する立場が示されている。 

したがって、評価軸も「使いやすいか」だけでは足りない。  
**相互運用可能な primitive として堅実か**を問う必要がある。

---

## 結論

TC39 Signals を primitive として評価するときは、少なくとも次の5軸で見ると整理しやすい。

1. **表面 API ではなく core semantics を共有できるか**
2. **フレームワーク固有層を切り離せているか**
3. **相互運用性の問題設定が具体的か**
4. **多実装で検証可能か**
5. **非目標と未確定点が明示されているか**

README と TC39 ノートを見る限り、Signals proposal はこの5軸をかなり意識して設計されている。だから「標準化は遅いが、primitive 候補として読む価値が高い」という評価は妥当である。  
ただし、現状はまだ **Stage 1 の長期検証段階**であり、確立済みの標準基盤とまでは言わないほうが正確である。 

---

## 評価軸1: 表面 API ではなく core semantics を共有できるか

primitive を評価するとき、まず見るべきなのは「この API が気持ちいいか」ではない。  
重要なのは、**異なる実装が同じ意味論を共有できるか**である。

Signals proposal README は、Signals の標準化対象を、フレームワークごとの表面 API よりも **underlying signal graph の precise core semantics** に置いている。さらに Promises/A+ との類比を使って、共通の基礎モデルを先に整理したいという方向を示している。 

この点は primitive としてかなり重要である。  
なぜなら、表面 API は好みや流儀に依存しやすいが、core semantics は相互運用性の基礎になるからである。

### 見るポイント

- state / computed / dependency tracking の意味論が共有可能か
- framework ごとの差分が表面 API に閉じているか
- semantic core と sugar が分離されているか

---

## 評価軸2: フレームワーク固有層を切り離せているか

README では、Signals proposal が最初から **effects、scheduling、ownership/disposal** のようなフレームワーク固有性の強い領域を標準化対象の中心に置いていないことが説明されている。 

これは非常に重要である。  
primitive が primitive であるためには、**各フレームワークの上位戦略まで抱え込まない**ほうがよい。

もし scheduling や ownership まで一気に標準化しようとすると、各実装の差異が大きすぎて shared primitive になりにくい。  
逆に、state / computed / graph invalidation のような下位層に集中すれば、共通部分を抽出しやすい。

### 見るポイント

- 提案が framework policy を抱え込みすぎていないか
- core と policy の境界が見えているか
- 「何を標準化しないか」が明示されているか

---

## 評価軸3: 相互運用性の問題設定が具体的か

README の Interoperability 節は、この提案の強さを示す部分である。  
そこでは、各 Signal 実装が独自の auto-tracking を持つため、モデル・コンポーネント・ライブラリ・non-UI code の共有が難しく、結果として view engine との不必要な結合が生まれやすいと説明されている。 

つまり、この proposal は「Signals が人気だから標準化したい」のではなく、**既存エコシステムの分断をどう減らすか**という具体的な問題設定を持っている。

primitive proposal が信頼できるかどうかは、  
**抽象的な美しさではなく、現実の分断をどれだけ具体的に説明しているか**  
でかなり変わる。

### 見るポイント

- 何が今共有できず困っているのかが具体的か
- 単なる流行追従ではなく、実際の分断が示されているか
- UI フレームワークをまたいだ reuse の話が成立しているか

---

## 評価軸4: 多実装で検証可能か

README の development plan は、Signals を急いで Stage 2 へ進めるより、**production-grade polyfill、framework integration、tests、benchmarks** を積み上げる方針を示している。 

さらに 2024年6月の TC39 meeting notes では、Daniel Ehrenberg がこの提案を **slow project** と位置づけ、今後12か月以内の Stage 2 提案を想定していないと述べている。理由は、まだ多くのことを実地で確かめる必要があるからである。 

これは進捗が遅いという意味でもあるが、primitive の評価としてはむしろ健全である。  
shared primitive は、**一つの実装で動くこと**より、**複数の実装でズレずに成立すること**のほうが重要だからである。

### 見るポイント

- 複数実装で検証する計画があるか
- benchmark と test が設計判断に組み込まれているか
- 単一ベンダー都合で前進していないか

---

## 評価軸5: 非目標と未確定点が明示されているか

README と meeting notes を読むと、Signals proposal は「できること」だけでなく、**まだ入れていないこと、合意できていないこと、omit していること**をかなり正直に書いている。README は omission があること自体を認め、framework 間の consensus が不足している部分や、上位レイヤで対処可能かもしれない部分は保留にしている。 

また 2024年4月の TC39 notes でも、方向性への支持はある一方で、Stage 2 に進む前に解くべき問題が残っていると整理されている。 

primitive proposal で危険なのは、万能感を出して境界を曖昧にすることである。  
その意味で、**未確定点を未確定点として扱う姿勢**はむしろ信頼材料になる。

### 見るポイント

- omissions が書かれているか
- unresolved questions が隠されていないか
- 「全部解けた」ふうに見せていないか

---

## この proposal が primitive 候補として有望に見える理由

以上の5軸で見ると、Signals proposal はかなり真面目である。

特に強いのは次の点である。

- 最初から **shared semantics** を狙っている
- **framework-specific policy** を抱え込みすぎない
- **interoperability problem** を具体的に説明している
- **multi-implementation validation** を重視している
- **未確定点を隠していない**

このため、Signals proposal は「便利そうだから推したい提案」より、**shared primitive の境界を慎重に探っている提案**として読める。README の膨大さも、その慎重さの反映と見てよい。 

---

## ただし、まだ言い切りすぎない

有望なのは確かだが、現時点での言い方は調整が必要である。

妥当なのは次の表現である。

- 「TC39 Signals は、相互運用性を目指す堅実な基礎概念**候補**である」
- 「README と FAQ は、その候補としての設計判断をかなり丁寧に示している」
- 「ただし、まだ Stage 1 の長期検証中であり、確立済みの標準基盤ではない」

2024年6月時点でも、TC39 側はこの proposal を slow project と見ており、短期で Stage 2 へ進める前提ではない。 

したがって、「堅実な基礎概念である」と断定するより、  
**「堅実な基礎概念候補として読む価値が高い」**  
と言うほうが現状に合っている。

---

## 自分用の短い判定基準

Signals proposal を読むときは、次の問いを置くと判断しやすい。

- これは API の好みの話か、それとも semantics の話か
- それは framework policy か、それとも shared primitive か
- その相互運用性の問題は現実に存在するか
- 複数実装で検証できる形になっているか
- 何をまだ決めていないかが見えるか

この5問にかなり前向きに答えられるなら、その proposal は primitive 候補として評価しやすい。

---

## まとめ

TC39 Signals proposal は、標準化の進行こそ遅いが、primitive proposal として見るとかなり筋がよい。

評価すべき点は、表面 API の派手さではなく、

- core semantics を狙っていること
- framework 固有層を切り離そうとしていること
- interoperability の問題設定が具体的であること
- 多実装検証を重視していること
- 未確定点を隠していないこと

にある。

したがって、現時点でのもっとも自然な評価はこうである。

**TC39 Signals は、相互運用性を可能にする shared primitive を目指す、堅実な基礎概念候補として非常に読む価値が高い。**  
ただし、**まだ完成した標準基盤ではなく、長めの Stage 1 で検証されている途中**である。
