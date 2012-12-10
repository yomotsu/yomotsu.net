---
layout: default
title: CSS Shaders はじめの 4歩目、簡単な変形 2
categories: [webgl]
---

[GraphicalWeb Advent Calendar 2012](http://www.adventar.org/calendars/10) の 10日目の記事です。

この記事では、変形するための行列を CSS 側から入力する方法について見ていきます。

## 前回のおさらい

前回は vertex shader 内で変換行列 (transform matrix) を用意し、それを乗算することで移動や回転を行いました。しかし実は、この変換行列はもっと簡単に用意することもできます。

CSS Shaders において、変換行列を用意する方法は、shader 内に用意する方法とは別に CSS で用意する方法もあります。この方法では、CSS transforms プロパティーの値のシンタックスを応用します。

CSS で行列を用意し、回転する方法を見てみましょう。

## 回転する

前回までは、custum 関数にはの引数として

* 第一引数に vertex shader と fragment shader
* 第二引数に 分割数

のみ利用していました。

	.sample {
		width:500px;
		background: url( bg.png );
		-webkit-filter: custom(
			url( shader.vs ) mix( url( shader.fs ) ),
			32 32
		);
	}

これに加えて第三引数以降も利用してみましょう。custom の第三引数以降には任意の値を渡すことができます。Z 軸で 45 度回転させるために rotateZ( 45deg ) を第三引数としてみましょう。変数名はなんでもいいのですが、ここでは「u_transform」とします。

.sample {
	width:500px;
	background: url( bg.png );
	-webkit-filter: custom(
		url( shader.vs ) mix( url( shader.fs ) ),
		32 32,
		u_transform rotateZ( 45deg )
	);
}

vertex shader では u_transform を mat4 として受け取ることができます。vertex shader で受け取った u_transform を宣言し、この行列を乗算してみましょう。

	precision mediump float;

	attribute vec4 a_position;
	uniform mat4 u_projectionMatrix;

	uniform mat4 u_transform;

	void main() {
	    gl_Position = u_projectionMatrix * u_transform * a_position;
	}

この CSS と vertex shader の組み合わせにより、前回の記事と同じように要素を回転することができます。

![](http://yomotsu.net/blog/assets/2012-12-09-cssshaders4/3.png)

[demo](http://yomotsu.net/blog/assets/2012-12-09-cssshaders4/3.html)(2012年 12月現在では CSS Shader を有効にした Chrome 25以降である必要があります。)

## 複雑な変換

CSS からの入力を利用すれば、少し複雑な変換行列を用意することもできます。例えば、perspective() rotateX() で消失点、回転を同時に入力してみましょう。CSS 側では次のように指定します。

.sample {
	width:500px;
	background: url( bg.png );
	-webkit-filter: custom(
		url( shader.vs ) mix( url( shader.fs ) ),
		32 32,
		u_transform perspective( 1000 ) rotateX( 45deg )
	);
}

vertes shader, fragment shader は先ほどと同様です。


### shader.vs

	precision mediump float;

	attribute vec4 a_position;
	uniform mat4 u_projectionMatrix;

	void main() {
	    gl_Position = u_projectionMatrix * a_position;
	}

![](http://yomotsu.net/blog/assets/2012-12-09-cssshaders4/1.png)

[ここまでの状態の demo](http://yomotsu.net/blog/assets/2012-12-09-cssshaders4/1.html)(2012年 12月現在では CSS Shader を有効にした Chrome 25以降である必要があります。)

このように CSS Shader は CSS と Shader を組み合わせて利用することができるというわけです。次回は CSS Shader でしかできない変換を試します。