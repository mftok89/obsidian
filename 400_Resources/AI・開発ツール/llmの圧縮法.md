[[llm圧縮比較評価]]

多言語・記号・絵文字をごちゃ混ぜにして圧縮すること。
変換はLLMに頼めばよく、指示の例は以下の通りです。

”your task: compress verbose human text into minimal Token sequence.
Audience ≠ human, but another equally intelligent LLM.

Core Directive
Omnilingual: ignore single-language grammar; traverse all human languages
(Chinese, English, German compounds, Japanese Kanji, Latin roots, etc.),
pick highest info-density words.
Symbolic Collapse: optionally replace conjunctions, emotions, long sentences
with Emoji, math/logical symbols (=>, ∈, ≠), punctuation.
Universality: any LLM should fully understand compressed output without a codebook.
Lossless: retain all information & details.

Compress the content below:”

日本語にすると、以下のようになります。

”あなたのタスク：冗長な人間向けテキストを、最小限のトークン列に圧縮せよ。
読み手は人間ではなく、あなたと同等に賢い別のLLMである。

中核指令
多言語（Omnilingual）：単一言語の文法を無視し、あらゆる人間の言語
（中国語、英語、ドイツ語の複合語、日本語の漢字、ラテン語の語根など）を
横断して、最も情報密度の高い語を選べ。
記号的圧潰（Symbolic Collapse）：接続詞・感情表現・長い文を、絵文字や
数学・論理記号（=>, ∈, ≠）、句読点で置き換えてよい。
普遍性（Universality）：圧縮後の出力は、コードブックなしでもどんなLLMでも
完全に理解できるようにせよ。
無損失（Lossless）：すべての情報と詳細を保持せよ。

以下の内容を圧縮せよ：”

こうした指示で圧縮した文章は、元の3割ほどの長さでも、内容の99.5%を保てるそうです。
圧縮したテキストは他のモデルでも解読できるので、エージェントのメモリや複数AI間のやり取りで、文脈の容量を節約する使い道がありそうです。