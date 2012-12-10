---
layout: default
title: CSS Shaders はじめの 3歩目、簡単な vertex shader を適用する
categories: [webgl]
---

[GraphicalWeb Advent Calendar 2012](http://www.adventar.org/calendars/10) の 8日目の記事です。

この記事では、CSS Shaders の基本として、何も変形しない CSS Shader を適用する方法を解説します。特に vertex shader に注目し、fragment shader については後日触れます。

## 1. HTML を用意する

まずは CSS を適用する HTML を用意します。ここでは、テキストと画像のみの HTML を用意しました。あわせて CSS Shader による変化がわかりやすいように幅と背景スタイルを適用しておきます。

	<!DOCTYPE html>
	<html>
	<head>
	<meta charset="UTF-8">
	<title>CSS Shaders!</title>
	<style>
	.sample {
		width:500px;
		background: url( bg.png );
	}
	</style>
	</head>
	<body>

	<div class="sample">
		<p>CSS Shader!! シィーエスエス シェーダー!! </p>
		<img src="nyantocat.gif" alt="">
		<p>the Nyantocat by Cameron McEfee</p>
	</div>

	</body>
	</html>

![](http://yomotsu.net/blog/assets/2012-12-08-cssshaders3/1.png)

[ここまでの状態の demo](http://yomotsu.net/blog/assets/2012-12-08-cssshaders3/1.html)

## 2. CSS Shader を適用する

先ほど用意した div.sample に filter プロパティーで custom() を適用します。

前回、custom 関数のシンタックスは

	custom(<vertex-shader> <fragment-shader>? [ , <vertex-mesh> ]? [ , <params> ]?)

のように紹介しました。それに沿い、ここでは custom() の引数に

1. vertex shader : `url( shader.vs )`
2. fragment shader : `mix( url( shader.fs ) )`
3. 分割数 : `32 32`

を指定していきます。なお、草案によると fragment shader は省略可能なのですが、Chrome 25 時点では明示する必要があったため、内容が空の fs ファイルを mix 関数で指定しています。fragment shader の扱いについては、後々解説できればと。

上記をまとめた CSS のコードをつぎに示します。

	.sample {
		width:500px;
		background: url( bg.png );
		-webkit-filter: custom(
			url( shader.vs ) mix( url( shader.fs ) ),
			32 32
		);
	}

上記の `url()` で参照している shader.fs の内容は空です。

## 3. shader.vs を用意する

各 shader は GLSL という言語で記述する必要があります。ここでは次のようなコードを shader.vs としてみましょう。

	precision mediump float;

	attribute vec4 a_position;

	void main() {
	    gl_Position = a_position;
	}

すると次のような表示がされるはずです。

![](http://yomotsu.net/blog/assets/2012-12-08-cssshaders3/2.png)

[ここまでの状態の demo](http://yomotsu.net/blog/assets/2012-12-08-cssshaders3/2.html)(2012年 12月現在では CSS Shader を有効にした Chrome 25以降である必要があります。)

元の要素の大きさが WebGL のビューポートのように振る舞い、`a_position` で渡された要素の座標を `gl_Position` そのまま代入するとビューポートに対して 1 * 1 の大きさで表示されます。このビューポートのことを filter region box というようです。

vertex shader のコードを 1行ずつ見ていきましょう。

### 1 行目

	precision mediump float;

float の精度を宣言しています。

### 3 行目

	attribute vec4 a_position;

attribute 変数として vec4 型の a_position を宣言しています。`a_position` はオブジェクト (HTML 要素) の座標情報で、その内容は CSS から自動的に vec4 型で渡されています。(WebGL では、これらの変数を自身で入力する必要がありますが、CSS Shader は分割数だけ宣言しておけば座標情報は自動で入力されるのです。) なお、`vec4` とは 4つの小数点数で構成される型です。`a_position` の内容には、x, y, z, w が入っています。

なお、`a_position` のほかにも CSS から Shader へ暗黙に渡される変数は次があります。

変数種|型|変数名
---|---|---
attribute|vec4|a_position
attribute|vec2|a_texCoord
attribute|vec2|a_meshCoord
attribute|vec3|a_triangleCoord
uniform|mat4|u_projectionMatrix
uniform|vec2|u_textureSize
uniform|vec4|u_meshBox
uniform|vec2|u_tileSize
uniform|vec2|u_meshSize

### 5 ~ 7 行目

	void main() {
	    gl_Position = a_position;
	}

main 関数で `gl_Position` への代入をしています。shader は必ず一つの main 関数を持ち、自動的に実行されます。最終的に `gl_Position` に代入された座標が表示されるボックスの座標となります。ここでは、`a_position` をそのまま `gl_Position` に代入しているので、ビューポートの中央に 1 * 1 の大きさで元の要素が配置されます。

## 3. 元の位置に配置する

元の位置に表示するためには、射影変換行列として CSS から自動的に渡された `u_projectionMatrix` を `a_position` に乗算します。`u_projectionMatrix` は `mat4` 型なので vec4 型である `a_position` とは型が違いますがそこは GLSL がよしなにやってくれます。

	precision mediump float;

	attribute vec4 a_position;
	uniform mat4 u_projectionMatrix;

	void main() {
	    gl_Position = u_projectionMatrix * a_position;
	}

すると元の位置に表示されました。

![](http://yomotsu.net/blog/assets/2012-12-08-cssshaders3/3.png)

[ここまでの状態の demo](http://yomotsu.net/blog/assets/2012-12-08-cssshaders3/3.html)(2012年 12月現在では CSS Shader を有効にした Chrome 25以降である必要があります。)

次回は表示結果をもうすこし変形させてみようと思います。