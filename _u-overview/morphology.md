---
layout: base
title:  'Morphology'
permalink: u/overview/morphology.html
udver: '2'
---

# Morphology: General Principles

UDは通言語的に適用される形態統語論の標示を備えている．これは，文法的概念が語形 (形態論) もしくは依存関係 (統語論) によって示されることを意味しており，UDのスキームにおいて，語の形態論的な指定は3つの表示レベルから成る:

* _基本語 (lemma)_ は語の意味内容を表示する．
* _品詞 (part-of-speech tag)_ 語に関連した抽象的な語彙カテゴリを表示する．
* _素性 (features)_ の集合は，特定の語形に関連した語彙的・文法的性質を表示する．


基本語は典型的に当該言語の辞書や語彙目録から決定される．対照的に．品詞 (part-of-speech) タグと文法的な性質は，以下に定義される2つの普遍的なリストから示される．

言語特有のさまざまなタグセットとは異なり，普遍的なタグと素性は _融合語 (fused words)_ (2語を合成することで作らる，統語的に独立していて異なる品詞に属するような語) を表示する方法を持たない:
チェコ語  _dělals (dělal + jsi_ ... 主動詞 + 助動詞); _proň (pro + něj_ ... 前置詞 + 代名詞);
ドイツ語 _zum (zu + dem_ ... 前置詞 + 冠詞);
スペイン語 _dámelo (da + me + lo_ ... 動詞 + 接語 (clitics) ) など.
UDで融合を扱う唯一のアプローチは，トークンと (統語的) 語を区別づけ，必要ならばトークンを統語的な語に分割するような言語特有の処理を適用することである．
全ての統語的な語は，それ自身の品詞タグと素性を持つ．詳しくは，<a href="tokenization.html">トークン化 (Tokenization)</a> および
<a href="../../format.html">フォーマット (Format)</a> を参照のこと.

## Lemmas

`LEMMA` フィールドでは，辞書に典型的に記載されている形式である，標準的もしくは基体 (base) の語形を含むべきである．
膠着語 (agglutinative) であれば，これは屈折接辞 (inflectional affixes) を持たない形式である; 融合語では，基本語は言語固有の慣習に従う.
基本語が存在しない場合，アンダースコア ("`_`") を用いて存在しないことを表す．

現在のところ，ツリーバンクには何が"標準形式"であるかを表すための余地が多く残っている．
補充形 (suppletion) の稀な例を除き，一つの形式は動詞，名詞，限定詞，もしくは前置詞のパラダイムにおける基本語として選択されることが望ましい．
形容詞と副詞の基本語は原級 (positive) の形式を表す (比較級 (comparative) や最上級 (superlative) の形式を持つ言語において)．
基本語は派生形態論 (derivational morphology) を除外しないため，[英] _organizations_ の基本語は _organization_ であって，_organize_ (もしくは _organ_) とはならない．
一般的に，標準の形式は屈折や書記/綴りによる変異 (格，アクセント/発音区別記号やタイポなど) をひとまとめにしている． 
語彙素フィールドにおいて，いくつかのツリーバンクでは方言や書き手の文体を示す綴りの変異を厳密に標準化することがある．

基本語の綴りの標準化に加えて，ツリーバンクでは任意の形態論的素性 `Typo=Yes` を採用することが推奨され，語の意図的でない綴り間違い (e.g. *ltake*で*take*を，*too*で*to*を) を明示するのに用いられる．
語のタイポの閉じたクラスは，それぞれのクラスについて，語の頻度をコーパスで検出することで明らかになる．
ツリーバンクの管理者は，方言，文体や非母語話者の文法といった，実際の言語変異を反映するような語に対して`Typo=Yes`を使用しないように注意されたい．

省略された (abbreviated) 形式は，完全な綴りが単一の語である場合，素性 [`Abbr=Yes`](../feat/Abbr.html) を伴って，基本語の完全な綴りと対応づけられる．
複数の語を省略したものについては，それを基本語として保持すべきである．

時には，タイポや省略語が屈折した語 (e.g. *hadd*で*had*を) にも適用されることがあり，その基本語では綴りを標準化して，屈折要素を除去する．
ツリーバンクでは，`MISC`フィールドを用いて，標準化されつつも基本語の形式が存在しないような形式を収納しておくように．

(明らかに間違いである臨時語，不足している語，誤って分割された語の基本語に関する，UD全体のポリシーは現在のところ存在しない．)

`LEMMA`フィールドは素性や，類似した語の特性をエンコードするのに用いるべきではない (代わりに `FEATS` や `MISC` を使うように; [format](../../format.html) を参照)．

いくつかのコーパスでは同音意義の基本語を区別するのに数字を付与することがある (e.g. [en] _can-1_ vs. _can-2_).
UDでは，そのような記法は標準形式の一部ではないことから，`LEMMA`フィールドに現れない．
そのような記法で基本語を特定できる場合，`MISC`のカラムである任意の属性`LId`に数字を付与する (`LId=can-1`)．

## Part-of-Speech Tags

<a href="../../u/pos/index.html">普遍的なPOSタグ</a> のリストは，17つのタグを含む固定のリストである．
いくつかの言語では，いくつかのタグが使用できない場合がある．しかし，このリストは言語特有のものに拡張することはできない．代わりに <a href="../../u/feat/index.html">素性</a> を用いることで，より詳細な語の分類が可能となる．

さらに，<a href="../../format.html">CoNLL-U形式</a> は追加のXPOSタグを許容し，これは言語固有 (もしくはコーパス特有) のタグセットから取ったものである．
そのような言語固有のXPOSタグは固有のデータカラムを有するため，普遍的なPOSタグとは混じらない．
普遍的なPOSタグは，英字の`[A-Z]` で記されたものに限られる．
一つの語につき一つのタグが想定される場合，タグを空白にすべきではない (他のタグが使用可能でない場合，アンダースコアを用いる代わりに `X` タグを使用せよ)．

### Using a word vs. mentioning it

普遍的なPOSタグは，可能であれば形態論的特性と同様に規則的な統語的ふるまいを捉え，文に特有の例外的なふるまいを反映すべきではない．特に，POSタグでは語の使用 (use) と言及 (mention) を区別しない．そのため，以下の例にある_yes_は両方とも間投詞 (interjection) としてタグ付けされる

* _Yes, I think so._
* _I am waiting for his ‘yes’ on the matter._

類似したものとして，以下の例では_precede_が両方とも動詞としてタグ付けされる:

* _Such discussion must precede every decision._
* _He pronounced ‘precede’ in a funny way._

### Pronominal words

代名詞句 (pronominal words)は，[pronouns](/u/pos/PRON.html), [determiners](/u/pos/DET.html) (冠詞と代名詞的形容詞)， 代名詞的 [adverbs](/u/pos/ADV.html) _(where, when, how)_であり，，いくつかの伝統文法では代名詞的 [numerals](/u/pos/NUM.html) _(how much)_ も代名詞句となる．


* 大抵の場合では，ある語が代名詞句かどうかを決定するのは容易である (素性 [PronType](/u/feat/PronType.html) も参照)．
  しかし，不定の決定詞と形容詞の境界はやや不明瞭である．関連する言語では代名詞句として扱う語のリストを統一するべきである． 
  ガイドラインの残りで，代名名詞句グループの境界を設定する．
* 代名詞的副詞は`ADV`とタグ付けされる．その代名詞性は`PronType`の素性によってエンコードされる．典型的な統語機能は動詞を修飾することである．
* 冠詞_(the, a, an)_は常に`DET`である; その`PronType`は`Art`である.
* 代名詞的数詞 (量化詞) は`DET`である; `PronType`に加えて，素性 [NumType](/u/feat/NumType.html) が用いられる.
* 形容詞と似たふるまいを行う語は`DET`である.
  ( `DET`クラスは擬似的な形容詞 (pro-adjectives) だと考えており, 英語で限定詞として通常みなされるものよりも広義である．)
  特に，一つの名詞句は一つより多くの限定詞によって修飾することが可能である．) Similar behavior means:
  * それらは，実詞として (substantibely) 用いられる (名詞句と置き換える) よりも，属性として (attributively) 用いられる (名詞句を修飾する) ことのほうが多いが，単独で現れる場合もある．
    単独で用いられるとき，それは省略 (ellipsis) によるものか，修飾される仮の (hypothetical) 名詞が_All [visitors] must pay._のように，未指定もしくは一般的だからである．
  * それらの屈折は形容詞のものに近く，名詞の屈折とは区別される．それらは修飾する名詞と一致する (agree) からである.
    特に，性 (gender) に関して屈折をする能力は形容詞と限定詞に典型的である．
    (名詞の性は語彙的に決定され，限定詞は名詞の性と一致することが求められる; よって，限定詞は性に関して屈折する必要がある．)
* 所有格でない人称代名詞，再帰代名詞もしくは相互代名詞 (reciprocal pronouns) は常に`PRON`のタグが付与される．
* 所有代名詞は言語間で変異がみられ，いくつかの言語では`DET`カテゴリに代入するという上記のテストが用いられる．
  他方で，特定の格 (多くは属格) において通常の人称代名詞もしくは，後置詞を伴う人称代名詞に近い; それらは`PRON`のタグが付与される.
* 上記の規則がうまくいかない場合，そのカテゴリは当該言語の伝統文法に依拠すべきである．
* 理想的には．言語特有のドキュメンテーションは代名詞類の語とそれらのカテゴリをリスト化することが望ましい．代名詞は閉じたクラス (closed class) であるため，リストの作成に困ることはないと考えられる．

### See also

以下の特別な事項に関するガイドラインは．特定のPOSタグについて記した補足ページに記録してある:

* 省略語 (abbreviations) および頭文字語 (acronyms): [SYM]() として記載される

## Features

素性 (features) は語，品詞および形態統語論的特性についての追加の情報である．あらゆる素性は`Name=Value`の形式をとり，全ての語はいずれかの素性を持っており，`Gender=Masc|Number=Sing`のように縦棒によって区別が示される．

<a href="../../u/feat/index.html">素性の目録</a> は複数のコーパスで観察されたものであるため，それに従い統一的にエンコードすることが望ましい．
リストはおそらく完全でなく，後のバージョンでは，新たな言語，コーパスやタグセットで検出した素性や値 (value) を含むことがある．
必要に応じて，利用者が普遍的な素性のリストを拡張したり言語特有の素性を加えることができる．
そのような素性は言語特有のドキュメンテーションに記載してあり，ここでの一般原則に従うこととなる．
語の普遍的もしくは言語特有の素性はFEATSカラムにリストしてある．

* 識別子 (identifier) は2種類ある:
  - 素性の名称 = _features_
  - 素性の値 = _values_
* 全ての識別子 (素性と値の両方) は英字もしくは，時には0-9の数字によって記される．先頭の字は常に大文字である．
  他の字は通常小文字であるが，可読性を優先して複数語の内部にある語の先頭には例外的に大文字で記す (e.g. `NumType`)．
  これは，素性について，<a href="../../u/pos/index.html">普遍的なPOSタグ</a> (全て大文字) と<a href="../../u/dep/index.html">普遍的な依存関係 (dependency relations)</a> (全て小文字) から区別するためである．
* 語の素性は常にデータ内で完全に指定されるべきであり，すなわち素性の名称と値の両方が特定される: `PronType=Prs`.
  ただし，ある値が他の素性間において異なる値を取るという保証はない．例えば，`Sup`は補充形 (superessive case)，比較の最上級 (superlative degree of comparison)，もしくは 動詞の分詞 (supine) を表す.
* 言うまでもなく，データ中の素性は空の値を持ち，これは，素性が当該の品詞にとって無関係であったり，語の値が言語特有の事情から決定できないことを意味する．
* ある語が2つ以上の素性を持つと言うことも可能である:`Case=Acc,Dat`. その場合，いずれか一つの値を持つが，どれかを決定できないと解釈される．_多値 (multivalues)_ は使用を避けるべきである.
  多値は，値のリストが値空間 (value space) 全体をカバーする場合や，当該言語で値の部分空間 (subspace) が有効である場合には用いてはならない．そのような状況では，当該の語についての素性が何も判明していないことに等しいため，そもそも素性を付与しないことが好ましい．
* 正規順序: 語の素性 (同じ行に現れる) は常にアルファベット順に並ぶ; 素性が多値であるときにも，アルファベット順である．この規則は，2語の素性を比較する必要がある際に便利である．
  アルファベット順のソートでは，大文字と小文字を同一とみなす．
  例えば，`Number`は`NumType`に先行する.
* 個々の素性の記述は，それらがどの品詞に現れるかを示唆する．この情報は素性の典型的な使用法を理解するのに寄与しうる; しかし，それは_厳密な規則ではない!_
  品詞に対する素性の適用可能性は大いに言語特有のものであって，ある素性が特定のPOSタグと共起しないとは仮定すべきでない．

### Lexical Features

語彙的素性は (個々の語形よりも) 語彙素 (lexeme) や基本語 (lemmas) の属性と考えられ，語の詳細な下位分類を表示する．

### Inflectional Features

屈折素性は，大抵の場合基本語ではなく語形の素性であるが，例外もある: 例えば，名詞の性は語彙的素性であることが通常である (一つの基本語における全ての語形は同一の性を持つ)．
しかし，他の品詞 (形容詞，代名詞，動詞) では名詞との一致によって性が屈折する場合がある．

### Layered Features

いくつかの言語では，いくつかの素性が同一の語に複数回付与されることがある．
そのとき，素性には_レイヤー (layers)_があると言え，個々のレイヤーの正確な意味は言語に依存する．

例えば．所有形容詞，限定詞や所有代名詞は素性 [u-feat/Gender]() および [u-feat/Number]() に関して2つの異なるレイヤーを持つことがある．値の一つは修飾された (所有される) 名詞との一致から決定され，これは名詞の性と数に関して一致する，他の (所有を表さない) 形容詞や限定詞と並列に扱うことが可能である．
他方の値は語彙的に決定され，これは，所有者の特性を反映している．


レイヤー化した素性の詳細な例については，<a href="feat-layers.html">レイヤー化した素性</a> を参照されたい．

ある言語で素性がレイヤー化した場合，素性の名称はレイヤーを示す必要がある．レイヤーを区別するには，例えば所有者の性を`Gender[psor]`と表すように，角括弧が追加の識別子として使用される．
レイヤーの識別子は英字の小文字`[a-z]`か数字の`[0-9]`から構成することが推奨される.
レイヤー，レイヤーの意味および識別子は本ドキュメンテーションにおける言語特有の拡張子によって定義される必要がある．レイヤー化した各々の素性については，一つのレイヤーがデフォルトとして定義され，例えば`Gender=Masc|Gender[psor]=Fem`のように，対応する素性が識別子なしで現れることがある．