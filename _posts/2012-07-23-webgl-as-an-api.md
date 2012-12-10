---
layout: default
title: WebGLは冗長だけど別にいいと思う
categories: [WebGL]
---

WebGLは冗長だ。使うためには

<ol>
<li>VertexShaderのソース書く</li>
<li>FragmentShaderのソース書く</li>
<li>VertexShaderのソースをコンパイルしてVertexShaderをつくる</li>
<li>VertexShaderのソースをコンパイルしてVertexShaderをつくる</li>
<li>空のシェーダープログラムを作って、2つのシェーダーを設定し、互いをリンクする</li>
<li>作ったシェーダープログラムを使うことを宣言する</li>
<li>空のバッファーを用意する</li>
<li>描画するモデルの頂点をきめてバッファーに書き込む</li>
<li>clearColor()する</li>
<li>drawArrays()する</li>
</ol>

のような手順を踏まなくていはいけない。これはまるで JavaScript を書いていることとは全く別の言語を書いているような感覚になるはずだ。

これは WebGL が OpenGL 由来というのがひとつの理由だ。OpenGL は歴史ある言語だし、安定している。そして、WebGL はゼロからつくられたのではなく、安定している言語である OpenGL を元につくられた。

![](http://yomotsu.net/blog/assets/2012-07-23-webgl-as-an-api/img_01.png)

WebGL はまるで OpenGL のサブセットのような API として存在している。そしてこの API を利用することで、ブラウザーから直接 GPU にアクセスできる。

WebGL は冗長なコードが必要であるが、あくまでも API なのだ。

とはいえ、普段 JavaScript を主に扱うであろうフロントエンドエンジニアが WebGL が難しく、異世界の言語であると感じるだろう。であれば、WebGL をもっとフロントエンドエンジニアの知識 (Javacript) に近づけたラッパーライブラリーを使えばいい。例えば THREE.js だ。

つまり、WebGL はあくまでも API であり、それをもっと簡単に使うこともできるし、そのまま使うこともできる。そして結果的に表示ができるというふうに理解しておくのがいいのではないと考えている。

***

ちなみに THREE.js について、[CodeGrid](http://www.codegrid.net/)で、この先少し基本を紹介するよなのでよかったらお読みください。