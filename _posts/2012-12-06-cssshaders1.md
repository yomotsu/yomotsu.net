---
layout: default
title: CSS Shaders はじめの一歩、CSS Filter を知る
categories: [webgl]
---

[GraphicalWeb Advent Calendar 2012](http://www.adventar.org/calendars/10) の 6日目の記事です。

CSS Shaders は、利用することでこれまでの CSS の表現ではできなかった、要素に対する複雑な変形を行うことができます。例えば、サイン波を利用して次のような変形が可能です。

![](http://yomotsu.net/blog/assets/2012-12-06-cssshaders1/1.png)

まずは CSS Shaders の書き方を知る前に CSS Filters についても少し知っておきましょう。

## CSS Filters とは

SVG にあった Filter 機能を整理した新しい仕様、[Filter Effects 1.0](https://dvcs.w3.org/hg/FXTF/raw-file/tip/filters/index.html)の草案が現在 W3C に提出されています。この中には CSS のインターフェイスとして用意された filter プロパティーも含まれています。 CSS Filter とは特にこの filter プロパティーを指します。

CSS の filter プロパティーには値として指定できる関数がいくつか用意されており、これらを利用すると簡単に表現効果を HTML の要素に与えることができます。

例えば、半径を 5px ぼかしたいのであれば

	selector{
		-webkit-filter: blur( 5px );
	}

のように記述します。

![](http://yomotsu.net/blog/assets/2012-12-06-cssshaders1/2.png)

[demo](http://yomotsu.net/blog/assets/2012-12-06-cssshaders1/blur.html)

コントラストを強めたいのであれば

	selector{
		-webkit-filter: contrast( 5 );
	}

のように指定します。

![](http://yomotsu.net/blog/assets/2012-12-06-cssshaders1/3.png)

[demo](http://yomotsu.net/blog/assets/2012-12-06-cssshaders1/contrast.html)

この他に filter プロパティーの値として[指定できる関数は様々あり](https://dvcs.w3.org/hg/FXTF/raw-file/tip/filters/index.html#FilterFunction)、その引数も、関数に応じて様々です。これらの filter 効果により、これまでの Web ではできなかった表現が可能となるわけです。

|効果|関数名|引数に指定できる値
|---|---|---
|グレースケール|grayscale()|数値 or パーセンテージ
|セピア|sepia()|数値 or パーセンテージ
|彩度|saturate()|数値 or パーセンテージ
|色相|hue-rotate()|角度
|階調の反転|invert()|数値 or パーセンテージ
|透明度|opacity()|数値 or パーセンテージ
|明度|brightness()|数値 or パーセンテージ
|コントラスト|contrast()|数値 or パーセンテージ
|ぼかし|blur()|長さ
|ドロップシャドウ|drop-shadow()|影
|SVGを参照する|url()|URL
|CSS Shaders!|custom()|shaderや分割数など

さて、こうした filter プロパティーの関数の一つに custom() が存在します。CSS Shaders は、CSS Filters の中でも特に custom 関数のことを指します。そして、CSS Shaders、つまり CSS Filters の custom 関数利用することにより、冒頭に示したキャプチャーのような複雑な変形が可能なのです。

次回は、custom() の使い方についてを解説します。

---

余談ですが、Firefox では、SVG の Filter を CSS 経由で HTML の要素にも適用することができます。これにより、現在 WebKit に実装されている CSS Filters とほぼ同じ表現が Firefox でも可能です。ここで紹介した関数もおそらく[近いうちに実装される](https://developer.mozilla.org/en-US/docs/CSS/filter)でしょう。

加えて余談ですが、冒頭で解説した通り、Internet Explorer は 4.0 から 9 まで独自に filter プロパティーを実装していました。これは今回紹介した W3C に提出されている filter プロパティーとシンタックスは別物ではありますが、よく似ているものも多数ありました。IE は Web 標準に足りない表現を昔からわかっていたというわけですね。