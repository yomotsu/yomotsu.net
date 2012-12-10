---
layout: default
title: レスポンシブ・ウェブデザインでの CSS コードの書き方
categories: [css]
---

いわゆるレスポンシブ・ウェブデザイン、つまりメディアクエリーを採用した Web サイトを構築する際の一番管理しやすいと感じるコードの書き方をまとめました。ただし、あくまでも個人的な、経験から感じた意見ですので絶対ではありません。

## CSS のコードの配置

広く知られている方法はいくつかあります。

* CSS ファイル自体を分ける方法
* @media 規則で 1ファイル内にメディア特性単位に書く方法
* @media 規則で 1ファイル内にパーツ単位で書く方法

それぞれをコードで表すなら以下の書き方が該当します。

### 方法1 : CSS ファイル自体を分ける方法

この方法は管理が大変でおすすめできません。これでファイルごとコード分割されてしまったらコード検索しづらいわけです。

	<link rel="stylesheet" href="desktop.css" media="(min-width:769px)">
	<link rel="stylesheet" href="tablet.css"  media="(max-width:768px) and (min-width: 321px)">
	<link rel="stylesheet" href="mobile.css"  media="(max-width:320px)">

### 方法2 : @media 規則で 1ファイル内にメディア特性単位に書く方法

この方法も管理が大変でおすすめできません。コード探す (検索する) 手間がわけた分だけ発生してしまいます。

	@media(min-width:769px){
		.pageHeader{...}
		.pageMain{...}
		.pageSub{...}
		.pageFooter{...}
	}
	@media(max-width:768px) and (min-width: 321px){
		.pageHeader{...}
		.pageMain{...}
		.pageSub{...}
		.pageFooter{...}
	}
	@media(max-width:320px){
		.pageHeader{...}
		.pageMain{...}
		.pageSub{...}
		.pageFooter{...}
	}

### 方法3 : @media 規則で 1ファイル内にパーツ単位で書く方法

この方法が最も管理しやすくおすすめです。ある程度巨大な Web サイトであってもこの方法で運用できています。さらに併せて、Sass などの機能である程度パーツのグループ単位でファイル分割すればより管理しやすいでしょう。

	/* pageHeader */
	.pageHeader{...}
	@media(max-width:768px){
		.pageHeader{...}
	}

	/* pageMain */
	.pageMain{
		float:left;
		...
	}
	@media(max-width:768px){
		.pageMain{
			float:none;
			...
		}
	}
	...

## パーツ単位で書く方法のコツ

特性の単位でコードをまとめてしまうと、おなじ箇所 (class や id など) をコントロールするスタイルが離れた場所に配置されることになります。例えば、ある一つのパーツのスタイルを調整するとき、複数のファイルにまたがり該当箇所を探す必要がありますし、場合によっては調整漏れが発生する可能性もあります。

一方で、パーツ単位でまとめておけばそれはありません。上手くコメントを使い、ある程度パーツ単位で CSS のコードをまとめておくといいです。

もう一つのコツとして、デスクトップ向けの記述は @media 規則の外に出しておき、その上で、その他のデバイス (タブレットやモバイル) 向けに**上書き**をしてデスクトップ向けのスタイルを流用することです。これは次に紹介する書き方の流れを通してみてきましょう。

具体的には次のような書き方です。なお、[実際に動く完成版](/blog/assets/2012-10-01-mediaquery-goodpractice/index.html)も用意しています。書き方の手順とコード見比べながら参考にしてみてください。

### 手順1 : ひな形の HTML を用意する

viewport 設定を忘れずに書きましょう。併せて、とりあえず HTML5Shiv 目的で modernizr も読み込ませています。

	<!doctype html>
	<html lang="ja" class="no-js">
	<head>
	<meta charset="utf-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>sample</title>
	<link rel="stylesheet" href="all.css">
	<script src="modernizr.custom.js"></script>
	</head>
	<body>

	</body>
	</html>

### 手順2 : パーツの HTML 用意する

body 内にパーツの HTML を書きます。ここではサンプルとして見出しと画像とテキストがセットになっているパーツの HTML を書きました。

	<section class="mod-sampleBlock">
		<header class="mod-sampleBlock-header">
			<h1>見出し</h1>
		</header>
		<div class="mod-sampleBlock-image">
			<img src="img.png" alt="" width="240" height="180">
		</div>
		<p>テキストテキストテキストテキストテキストテキスト</p>
	</section>

### 手順3 : デスクトップ用の CSS を書く

普通に書きます。media queries も使いません。コードは長いですが、特別なことはしていません。特別なことをしていないので旧 IE でも意図したとおりに表示されるでしょう。ちなみに自分は普段、コメントの書き方は [idiomatic-css](https://github.com/necolas/idiomatic-css) を参考にしています。


	/* mod-sampleBlock
	   ========================================================================== */
	/* [ 1 ] */
	.mod-sampleBlock{
		padding:10px;
		background:#eee;
		*zoom:1;
	}
	.mod-sampleBlock:after{
		content:'';
		clear:both;
		display:table;
	}
	/* [ 2 ] */
	.mod-sampleBlock .mod-sampleBlock-header{
		border-bottom:1px solid #000;
		margin:0 0 .5em;
	}
	.mod-sampleBlock .mod-sampleBlock-header h1{
		font-size:1.25em;
		margin:0;
	}
	/* [ 3 ] */
	.mod-sampleBlock .mod-sampleBlock-image {
		float:right;
		margin:0 0 0 10px;
	}

* &#91; 1 &#93; パーツの大枠である .mod-sampleBlock を装飾しつつ clearfix を適用
* &#91; 2 &#93; パーツ内のヘッダーである .mod-sampleBlock-header に装飾
* &#91; 3 &#93; パーツ内の画像コンテナーである .mod-sampleBlock-image を float して右に配置する

これでデスクトップ向け画像が右に浮いたパーツ (モジュール) ができます。

### 手順4 : 小さい画面のデバイス向けの CSS を書く

続けて、小さい画面向けの CSS を**追記**します。この際、分けて書く必要はありません。先ほどのコードの続きとしてあくまでも追記するのです。

追記する内容は以下のとおり。

	@media(max-width:600px){
		.mod-sampleBlock .mod-sampleBlock-image {
			float:none;
			margin:0 10px;
		}
		.mod-sampleBlock .mod-sampleBlock-image img{margin:0 auto;}
	}

小さい画面用の CSS のコードは先ほどのコードに比べるとだいぶ少ないですがこれでいいのです。なぜなら先程のコードは @media 規則の外にでておりグローバルな状態になっています。なので、小さい画面では、先程の状態を継承しつつ必要な部分だけを上書きしています。

つまり、**特定のデバイスに向けたスタイルのみ上書きして使う**わけです。

メリットとして以下があります。

* 追記で書かれているので、コードの位置が近いため把握しやすい
* コードが最小限で済む
* グローバル (デスクトップ) の装飾を変更すればそれが自動で小さい画面にも反映される

なお、旧 IE は media queries に対応していないため @media 規則でくくった内側を無視します。

### 補足 : Media Queries のポリーフィルライブラリは使わない

Media Queries のポリーフィルライブラリは使わないこともとても大切です。ポリーフィルライブラリは例えば、[Respond.js](https://github.com/scottjehl/Respond) が該当します。

IE6,7,8 は Media Queries に対応しておらず、これらの旧ブラウザーでも**変形**を行うためには、ポリーフィルライブラリーを使うことになるわけですが、果たしてそれは必要でしょうか。

**否!**

メリットは少なく、デメリットが大きいです。なので使うべきではありません。

* メリット : 旧ブラウザーを使っていても変形してかっこいい...かっこいいだけ。
* デメリット : media queries で振り分けられた @media 規則の内側にも旧 IE 基準の CSS を書かなくてはいけない。

よく考えてみてください。レスポンシブ・ウェブデザインは誰のための実装でしょうか。モバイル版がほぼ存在しない旧 IE はデスクトップ向けの表示だけで十分でしょう。無理にポリーフィルする必要はありません。

旧 IE には先ほど書いたグローバルなスタイルが適用されます。一方で旧 IE は @media 規則でクロージャのごとく区切られたコードは無視します。なので、これまでの書き方で旧 IE には対応できるし、@media 規則を用いて**追記**したコードで、さまざまなサイズのデバイスにも対応できるというわけです。

なお、これはつまり、@media 規則の内側では IE9 を基準に CSS3 や CSS2.1 を利用することができます。例えばそう、display:table; です。ちなみにもうすぐ発売する WebDesigning 11月号に display:table; を活用した小技を寄稿していますのでよろしければぜひ。

## 追記

ここで解説した方法では、@media 規則がたくさん登場することになり、もし特性、つまりブレークポイントの幅など変更する場合至る箇所を書き換えないといけないのでは? と、はてブでコメントいただきましたが、Sass 3.2 を使えば予め特性を変数にしておくこともできます。

例えば以下のように。

	$break-small: 320px;
	$break-large: 1200px;

	.profile-pic {
	  float: left;
	  width: 250px;
	  @media screen and (max-width: $break-small) {
	    width: 100px;
	    float: none;
	  }
	  @media screen and (min-width: $break-large) {
	    float: right;
	  }
	}

via. [Responsive Web Design in Sass: Using Media Queries in Sass 3.2](http://thesassway.com/intermediate/responsive-web-design-in-sass-using-media-queries-in-sass-32)