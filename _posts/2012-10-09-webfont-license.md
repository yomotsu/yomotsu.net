---
layout: default
title: Web フォントとライセンス
categories: [css]
---

Web フォントを利用する際、技術として理解するだけでなく、フォントに適用されたライセンスを理解しておくことも大切です。

特に和文フォントにおいては

* [SIL Open Font License (OFL)](http://scripts.sil.org/OFL)
* [mplus Font License](http://mplus-fonts.sourceforge.jp/mplus-outline-fonts/#license)
* [IPAフォントライセンスv1.0](http://ossipedia.ipa.go.jp/ipafont/#LicenseTOP)

あたりを適用されるケースが多いようです。また、これらのいずれかのライセンスが適用されていれば Web フォントとして利用することができます。ただし、ライセンスによる指示に従う必要があります。

##各ライセンスの要点

###SIL Open Font License (OFL)
OFL であることを明示すればほぼ自由に Web フォントに利用できます。ライセンス明示方法は後述。

###mplus Font License
ほぼ自由に Web フォントに利用できます。ライセンス明示方法は後述。

###IPA フォントライセンス v1.0
IPA フォントライセンスであることを明示し、かつ、オリジナルのフォントファイルを同梱すれば Web フォントに利用できます。ライセンス明示方法は後述。

##各ライセンスの明示方法

各ライセンスともに明確なライセンス明示方法は示されていません。しかし、これは [exljbris Font Foundry Free Font License](http://www.exljbris.com/eula.html) で指示されている方法が参考になります。

exljbris Font Foundry Free Font License では、

> You may use this font for Font-Face embedding, but only if you put a link to www.exljbris.nl on your page and/or put this notice /* A font by Jos Buivenga (exljbris) -> www.exljbris.com */ in your CSS file as near as possible to the piece of code that declares the Font-Face embedding of this font.

のように、Web フォントを想定したライセンスとなっており、Web ページにフォントを埋め込む際には

* Web ページ内に配布元へのリンクを用意する
* CSS ファイル内にコメントで配布元の URL を用意する

のいずれかで明示する必要があることを定めています。

ですのでこれを参考に、OFL, M+, IPA ライセンスにおいても、例えば次のような形式で CSS ソースコード内にライセンス情報明示しておく方法がよさそうです。

	/*!
	 * "fontname" is lisenced under the SIL Open Font License 1.1
	 * by http://font-foundry.com/fontname/
	 */
	@font-face {
	font-family: 'MyFontID';
	src: url('fontname.eot?') format('oldIE'),
	     url('fontname.woff') format('woff'),
	     url('fontname.ttf') format('truetype');
	}

IPA フォントライセンスについては、これに追加して、オリジナルのフォントファイルをダウンロードできる URL を用意しておけばよさそうです。[要出典]

##その他のライセンス

[武蔵システム](http://opentype.jp/)さんが公開しているフォントなどにはパブリックドメインが適用されています。パブリックドメインは、何も気にせず自由に利用していいライセンスです。

##その他の独自ライセンス

また一方で、個人でフォントを作成しているかたの場合、上記のような一般的なライセンスを明示していないケースも多いです。その場合は、それぞれの作者さんに確認する必要があります。

2010年の時点で私が作者さんに連絡を取り、いただいた回答から確認した限りでは

* アームド・レモンなどの[海沿いカリグラ邸！](http://calligra-tei.oops.jp/)さん
* あんずもじなどの[京風子](http://www8.plala.or.jp/p_dolce/)さん
* ふい字などの[ふい](http://hp.vector.co.jp/authors/VA039499/)さん

が Web フォントとして利用可能という回答をいただきました。ただし ふい字置き場。さんの公開するフォントについてはサブセットを承認していません。

とはいえ、専門のフォントファウンダリ等が作成したフォントに比べると、個人の方が作成したフォントはグリフのアウトラインが複雑なものも多く、Web フォントとして利用する場合にはサブセット化するなどして容量を節約するなどの工夫が必要そうです。

##CDN を利用する

Web フォントの分野においてそれらの CDN の多くは有償です。例えば、[FONT+](http://webfont.fontplus.jp/) や [TypeSquare](http://www.typesquare.com/) が該当します。

しかし、これらのサービスを利用するメリットとしては、サービス側でライセンスの問題を吸収してくれていますし、また、ダイナミックサブセッティングに対応している場合もあります。またその他様々な手間がかかる処理を CDN 側で引き受けてくれます。

![](http://yomotsu.net/blog/assets/2012-10-09-webfont-license/img_01.png)

費用はかかるものの、現時点においてはこうした CDN を利用するという選択肢が最良かもしれません。

##関連情報

* [SIL 日本語訳](http://sourceforge.jp/projects/opensource/wiki/SIL_Open_Font_License_1.1)
* [IPAフォント(or IPAフォントの派生フォント)はWebフォントとして使用できるか?](http://slashdot.jp/journal/552099)
* [5.3 AcrobatやFlashなどのコンテンツ作成用サーバーでの利用（ASP的なフォントの利用）について](http://ossipedia.ipa.go.jp/ipafont/faq.html#q005-3)