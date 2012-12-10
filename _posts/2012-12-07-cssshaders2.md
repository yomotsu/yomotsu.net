---
layout: default
title: CSS Shaders はじめの 2歩目、custom() で何が行われるのか
categories: [webgl]
---

[GraphicalWeb Advent Calendar 2012](http://www.adventar.org/calendars/10) の 7日目の記事です。

この記事では、CSS Shaders、つまり CSS Filters の custom 関数を利用したとき、何が行われるのかを知ります。

なお、この記事以降に登場する CSS Shader は、2012年 12月現在、Chrome Canary でしか動作せず、かつ、CSS Shader の設定を有効にしておく必要があります。実際の demo を表示したい場合にはこの記事の末尾で解説する手順で Chrome Canary をインストールし、設定を有効にしておいてください。

## CSS Shader を使う

custom 関数は、CSS Filters の他の関数と違い、その引数はすこし複雑です。この記事執筆段階の草案には次のように示されています。

	custom(<vertex-shader> <fragment-shader>? [ , <vertex-mesh> ]? [ , <params> ]?)

上記のそれぞれの意味は次の通りです。

vertex-shader|頂点を扱うシェーダー。基本的に url()で指定する。必須
fragment-shader|色とラスタライズを扱うシェーダー基本的に url()で指定する。オプション。
vertex-mesh|メッシュ分割数。2つの数値で指定され、それぞれは縦分割数と横分割数として扱われる。オプション
params|その他の任意に利用できる引数。オプション

例えば具体的には次のように利用します。

	selector{
		-webkit-filter: custom( url( shader.vs ) url( shader.fs ), 16 16, u_myVal1 10.0, u_myVal2 30.0 );
	}

## CSS Shaders 適用時の流れ

各値の説明だけではわかりづらいと思いますの。なぜこれらを指定する必要があるのか、CSS Shaders 適用時の流れも参考にするとそれらの意味がわかってくるでしょう。

### 1. メッシュ化

HTML の要素はまず分割数 (vertex-mesh) に応じたメッシュとして扱われます。例えばこの図の場合は costum 関数の引数の分割数として 4 4 が指定されていることになります。

![](http://yomotsu.net/blog/assets/2012-12-07-cssshaders2/1.png)

### 2. 頂点シェーダー

メッシュとなった要素は頂点シェーダー (vertex shader) に応じて変形されます。

![](http://yomotsu.net/blog/assets/2012-12-07-cssshaders2/2.png)

### 3. フラグメントシェーダー

変形された要素は、ピクセルに応じた色がつけられ、ラスタライズされます。

![](http://yomotsu.net/blog/assets/2012-12-07-cssshaders2/3.png)

### 4. 表示

ラスタライズされた要素が表示されます。

![](http://yomotsu.net/blog/assets/2012-12-07-cssshaders2/4.png)

次回は実際に簡単な CSS Shader を作成します。

## 付録 : Chrome Canary で CSS Shader を有効にするまでの流れ

Chrome Canary は開発版の Google Chrome で、一般向けにリリースされている正式版の Chrome よりも 1つから 2つほど上のバージョンを試すことができます。

Chrome Canary を利用することで、すこし未来に実装される予定の機能を試すことができます。

インストールしても通常の Chrome と競合することなく、別のブラウザーとして動きます。通常版と Canary を同時に起動することもできます。

インストールするには、[Chrome Canary](https://tools.google.com/dlpage/chromesxs/)からダウンロードします。

![](http://yomotsu.net/blog/assets/2012-12-07-cssshaders2/5.png)

インストールが完了したら、CSS Shaders を有効にしましょう。CSS Shaders はまだ新しい機能のため、デフォルトでは設定がオフとなっています。設定をオンにするには、URL バーに about:flags を入力し設定画面を開きます。

![](http://yomotsu.net/blog/assets/2012-12-07-cssshaders2/6.png)

機能一覧にある Enable CSS Shaders を Enable (有効) に設定します。

![](http://yomotsu.net/blog/assets/2012-12-07-cssshaders2/7.png)

これで CSS Shaders を表示する準備の完了です。このあと投稿する予定の記事では、どうすればこの変形ができるのかを解説していきます。