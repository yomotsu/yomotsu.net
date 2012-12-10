---
layout: default
title: CSS Shaders はじめの 4歩目、簡単な変形 1
categories: [webgl]
---

[GraphicalWeb Advent Calendar 2012](http://www.adventar.org/calendars/10) の 9日目の記事です。

この記事では、前回の記事サンプルに少し手を加え、簡単な変形を GLSL 内で行います。

## 前回のおさらい

次の HTML と vertex shader を用意しました。fragment shader は内容が空のテキストファイルです。

### HTML と CSS

	<!DOCTYPE html>
	<html>
	<head>
	<meta charset="UTF-8">
	<title>CSS Shaders!</title>
	<style>
	.sample {
		width:500px;
		background: url( bg.png );
		-webkit-filter: custom(
			url( shader.vs ) mix( url( shader.fs ) ),
			32 32
		);
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

### shader.vs

	precision mediump float;

	attribute vec4 a_position;
	uniform mat4 u_projectionMatrix;

	void main() {
	    gl_Position = u_projectionMatrix * a_position;
	}

![](http://yomotsu.net/blog/assets/2012-12-09-cssshaders4/1.png)

[ここまでの状態の demo](http://yomotsu.net/blog/assets/2012-12-09-cssshaders4/1.html)(2012年 12月現在では CSS Shader を有効にした Chrome 25以降である必要があります。)

今回は上記のコードに手を加え、vertex shader 内に、変換行列 (transform matrix) を用意し、要素を移動や回転をしてみます。

## GLSL に登場する型

さて、コードを紹介する前に、今回登場する型についてすこし知っておきましょう。GLSL には JavaScript と違ってたくさんの型があります。

### 変数の種類

attribute|Shader に送られてくる頂点毎に内容が異なる変数
uniform|Shader に送られてくる一定の変数
const|定数

### 変数の型

float|小数点数
vec4|4成分のベクトル (4つの float の組み)
mat4|4x4 の行列 (16この float の組み)

## 移動する

さっそく、vertex shader に手を加え、移動をさせてみましょう。移動や回転などを行う方法の一つに、変換行列を利用する方法があります。ただし、次回もう少し簡単な方法を紹介します。*今回の記事は軽く読み流しておけばいいでしょう。*

移動するための変換行列はわかりやすいです。下に示す行列の Tx, Ty, Tz を移動したい距離に置き換えるだけです。

<math>
<matrix>
<matrixrow>
<cn>1</cn><cn>0</cn><cn>0</cn><cn>Tx</cn>
</matrixrow>
<matrixrow>
<cn>0</cn><cn>1</cn><cn>0</cn><cn>Ty</cn>
</matrixrow>
<matrixrow>
<cn>0</cn><cn>0</cn><cn>1</cn><cn>Tz</cn>
</matrixrow>
<matrixrow>
<cn>0</cn><cn>0</cn><cn>0</cn><cn>1</cn>
</matrixrow>
</matrix>
</math>

ただし、WebGL の場合は、行と列の順番が入れ替わるため、実際のコードでは以下のに当てはめることになります。

	mat4(
	    1.0, 0.0, 0.0, 0.0,
	    0.0, 1.0, 0.0, 0.0,
	    0.0, 0.0, 1.0, 0.0,
	     Tx,  Ty,  Tz, 1.0
);

X 方向に 0.5 移動させるには、次の行列を mat4 変数として用意して乗算します。

	mat4 translate = mat4(
	    1.0, 0.0, 0.0, 0.0,
	    0.0, 1.0, 0.0, 0.0,
	    0.0, 0.0, 1.0, 0.0,
	    0.5, 0.0, 0.0, 1.0
	);

vertex shader に反映してみます。

	precision mediump float;

	attribute vec4 a_position;
	uniform mat4 u_projectionMatrix;

	void main() {
	    mat4 translate = mat4(
	        1.0, 0.0, 0.0, 0.0,
	        0.0, 1.0, 0.0, 0.0,
	        0.0, 0.0, 1.0, 0.0,
	        0.5, 0.0, 0.0, 1.0
	    );
	    gl_Position = u_projectionMatrix * translate * a_position;
	}

結果は次のようになります。CSS Shader による効果で X 方向に 0.5 動いたことがわかります。

![](http://yomotsu.net/blog/assets/2012-12-09-cssshaders4/2.png)

[ここまでの状態の demo](http://yomotsu.net/blog/assets/2012-12-09-cssshaders4/2.html)(2012年 12月現在では CSS Shader を有効にした Chrome 25以降である必要があります。)

## 回転する

回転は移動より少し複雑な行列が必要です。ここでは z 軸に対して回転する例をみてみましょう。z 軸に対して任意の角度分回転するには次の行列を用意します。

<math>
<matrix>
<matrixrow>
<cn>cz</cn><cn>-sz</cn><cn>0</cn><cn>0</cn>
</matrixrow>
<matrixrow>
<cn>sz</cn><cn>cz</cn><cn>0</cn><cn>0</cn>
</matrixrow>
<matrixrow>
<cn>0</cn><cn>0</cn><cn>1</cn><cn>0</cn>
</matrixrow>
<matrixrow>
<cn>0</cn><cn>0</cn><cn>0</cn><cn>1</cn>
</matrixrow>
</matrix>
</math>

上記の行列の

* sz は sin( r )
* cz は cos( r )

です。また r はラジアンです。ラジアンは「角度 * π / 180」で求めることができます。

ここでは、45 度回転してみましょう。移動と同様に、WebGL のコードでは行と列の順番が入れ替わるため、回転のための行列は次のように用意します。

	mat4 translate = mat4(
	     cz,  sz, 0.0, 0.0,
	    -sz,  cz, 0.0, 0.0,
	    0.0, 0.0, 1.0, 0.0,
	    0.5, 0.0, 0.0, 1.0
	);

コードの反映すると vertex shader は次のように書くことができます。

	precision mediump float;

	attribute vec4 a_position;
	uniform mat4 u_projectionMatrix;

	void main() {
	    const float PI = 3.1415;
	    float r = 45.0 * PI / 180.0;
	    mat4 rotate = mat4(
	         cos( r ), sin( r ), 0.0, 0.0,
	        -sin( r ), cos( r ), 0.0, 0.0,
	              0.0,      0.0, 1.0, 0.0,
	              0.0,      0.0, 0.0, 1.0
	    );
	    gl_Position =  u_projectionMatrix * rotate * a_position;
	}

![](http://yomotsu.net/blog/assets/2012-12-09-cssshaders4/3.png)

[ここまでの状態の demo](http://yomotsu.net/blog/assets/2012-12-09-cssshaders4/3.html)(2012年 12月現在では CSS Shader を有効にした Chrome 25以降である必要があります。)

移動や回転をするために行列を用意しましたが、実は CSS からもっと簡単に入力することもできます。次回はその方法をまとめたいと思います。なお、その他の変換するための行列は、[WebGL Basics 5 &#8211; Full transformation matrix &laquo; The Blog-o-Ben](http://blogoben.wordpress.com/2011/06/05/webgl-basics-5-full-transformation-matrix/)が参考になるかもしれません。