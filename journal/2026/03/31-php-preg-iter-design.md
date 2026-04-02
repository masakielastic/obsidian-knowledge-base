# preg_iter PHP RFC 構想メモ — str_iter の先に見えてきた regex iterator という primitive

date: 2026-03-31
tags: #php #php-rfc #regex #pcre #iterator #api-design #primitive #string-processing #unicode #software-architecture #oss

`preg_iter` という発想は、単なる「正規表現の便利関数を1個増やしたい」という話ではない。出発点は、`str_iter` のように「文字列を順にたどる primitive」を考えていると、「一致結果も順にたどれたほうが自然ではないか」と見えてくることだ。つまり `preg_iter` は、`str_iter` の延長線上にある構想として整理できる。

最初は、PCRE の `\X` を使った書記素クラスタイテレーターも発想としてはありえた。ただ、そこへ踏み込むと `IntlBreakIterator` とどう役割分担するのか、PCRE と ICU のどちらを基準に考えるのか、という大きな問題にすぐぶつかる。ここは単純に「PCREでできるから採用」とは言いにくい。だからこそ、書記素クラスタ専用 API を急ぐより、もっと汎用的な `preg_iter` を先に考えるほうが筋がよい、という流れになる。

既存 API に不満がないわけではない。`preg_match_all()` は全件取得には便利だが、結果が配列中心で、`PREG_SET_ORDER` や offset capture が入ると構造が急に重くなる。全件を集める用途には合っていても、「一致をひとつずつ処理したい」primitive としては少し遠い。`preg_replace_callback()` は逐次処理に近いものの、あくまで置換 API であって列挙 API ではない。「マッチを順番に見たいだけ」という用途に対して、責務がずれている。

ここで `preg_iter` を考える意味が出てくる。配列を返すのではなく、一致結果を順番に yield する。`str_iter` がコードポイントをたどる primitive なら、`preg_iter` は match をたどる primitive だ。これは単なる糖衣構文ではなく、「一致結果を列挙しながら処理する」というプログラミングモデルそのものを core に置く発想だと言える。数字の列挙、トークン抽出、ログ解析、簡易パーサーのような処理にも広く効く。

比較対象としては Python の `re.finditer()` がわかりやすい。`findall()` のような全件回収ではなく、match object を順に返す API があることで、「全部を配列にしてから考える」以外の道が見える。ただし、Python にあるから PHP にも必要、という話ではない。参考になるのは、配列 API と iterator API が別の役割を持っているという点だ。

もちろん、本体は実装論だ。`preg_match_all()` とどこまで内部ロジックを共有するか。zero-length match をどう安全に進めるか。flags、名前付きキャプチャ、offset をどう返すか。どんなテストケースで境界条件を固めるか。こういう地味な論点こそ RFC の中心になる。

`preg_iter` は regex 小技の話ではない。PHP core にどんな primitive を置くべきか、その判断基準を問う話だ。書記素クラスタ専用 API を急ぐより、まずは汎用の regex iterator を考える。そう整理すると、この構想は将来の標準 API 設計を考える手がかりとしてかなり面白い。
