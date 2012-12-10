---
layout: default
title: three.js の PlaneGeometry で地形を作る
categories: [webgl]
---

[GraphicalWeb Advent Calendar 2012](http://www.adventar.org/calendars/10) の 1日目の記事です。

この記事では、three.js の PlainGeometry の頂点の z 方向にノイズを加えて、少し凸凹とした地形を作ってみるという内容をまとめています。完成は以下のような表示です。

![](http://yomotsu.net/blog/assets/2012-12-01-create-terrain-with-threejs/1.png)

なお、demo は WebGL が有効なブラウザー (Chrome, Firefox と Flag を有効にした Opera と Safari) でしか再生できません。

順を追ってみていきます。

##手順 1

まずは three.js のお決まりに沿って、renderer, scene, camera, light を設置していきます。

scene に fog を指定しておくと遠方が指定した色にぼやけさせることができます。

	scene.fog = new THREE.FogExp2( 0xAA9966, 0.015 );

...この時点では何も表示されません。

[手順 1 までの状態の demo](http://yomotsu.net/blog/assets/2012-12-01-create-terrain-with-threejs/1.html)

![](http://yomotsu.net/blog/assets/2012-12-01-create-terrain-with-threejs/2.png)

##手順 2

THREE.PlaneGeometry でジオメトリーを作っておきます。

	var geometry = new THREE.PlaneGeometry( 150, 150, 64, 64 );

これで、大きさ 150 * 150 で 64 分割 * 64 分割の四角形ができます。

このジオメトリーにテクスチャだけ適用してメッシュにし、scene に add すると以下の状態になります。あわせて、90 度傾けて上向きにしておきましょう。この時点では真っ平らです。

	var map1 = THREE.ImageUtils.loadTexture( 'map1.jpg' );
	//(中略)
	ground = new THREE.Mesh(
	    geometry,
	    new THREE.MeshLambertMaterial( { map: map1 } )
	);

	ground.rotation.x = Math.PI / -2;
	scene.add( ground );

[手順 2 までの状態の demo](http://yomotsu.net/blog/assets/2012-12-01-create-terrain-with-threejs/2.html)

![](http://yomotsu.net/blog/assets/2012-12-01-create-terrain-with-threejs/3.png)

##手順 3

THREE.PlaneGeometry の頂点にパーリンノイズを加えて凹凸させてみます。パーリンノイズは格子状になだらかな変化を加えることができて、Photoshop でいう「雲模様」を想像するとわかりやすいでしょう。

パーリンノイズの生成には、three.js とは別に、[SimplexNoise クラス](https://gist.github.com/304522)を利用しました。

	<script src="perlin-noise-simplex.js"></script>

	var simplexNoise = new SimplexNoise;

パーリンノイズ付与を PlaneGeometry の頂点 (vertices) 分 for でまわし、z 方向に結果を付与していきます。

	for ( i = 0; i < geometry.vertices.length; i++ ) {
	    var vertex = geometry.vertices[ i ];
	    vertex.z = simplexNoise.noise( vertex.x / 20, vertex.y / 20 );
	}

[手順 3 までの状態の demo](http://yomotsu.net/blog/assets/2012-12-01-create-terrain-with-threejs/3.html)

![](http://yomotsu.net/blog/assets/2012-12-01-create-terrain-with-threejs/4.png)

##手順 4

PlaneGeometry の各頂点の z を凹凸させても法線の方向は変わっていないので、このままでは凹凸に応じた影ができません。便利なことに、 geometry.computeVertexNormals(); でこれを正すことができます。手順 3 の demo と比べると地形に応じた影ができているのがわかります。

	geometry.computeFaceNormals();
	geometry.computeVertexNormals();

[手順 4 までの状態の demo](http://yomotsu.net/blog/assets/2012-12-01-create-terrain-with-threejs/4.html)

![](http://yomotsu.net/blog/assets/2012-12-01-create-terrain-with-threejs/5.png)

ということで、地形を作りながら、three.js のプリミティブなオブジェクトの頂点を操作するときに小技として覚えておくとよさそうなことまとめ...でした。(three.js でゲーム作ってみようと思ったのですが時間がなく、それに使おうと思った地形のみの記事となってしまいました...)