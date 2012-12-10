---
layout: default
title: box2d の JS ポートがたくさんある
categories: []
---

2D の物理演算エンジンを探していて、JavaScript で有名なのは、もともと C++ で書かれた [Box2D](http://box2d.org/) を JavaScript に変換したライブラリなのですが、似ているライブラリーがたくさんあったのでメモ。ちなみに Box2D は Angry Birds でも使われている。

##[box2d.js](http://box2d-js.sourceforge.net/)

おそらくはじめて JS に変換されたライブラリー。あんどうやすし氏によって作成され、世界中で使われているが...prototype.js 依存なのと、多数のファイルに分かれており、今使うにはすこし気が引けてしまう。長い間アップデートもされていない。とはいえ、ドキュメントは豊富なので勉強しやすい。

##[box2d.js (Mr.doob 氏版)](http://mrdoob.com/projects/chromeexperiments/ball_pool/)

あんどうやすし氏が作成した box2d.js を Mr.doob 氏がワンファイルにまとめ、prototype.js の呪縛から解き放ったバージョン。ただし、prototype.js の機能を切り出した別添ライブラリーが必要で、そのライブラリーは prototype.js のごとく、ネイティブオブジェクトを拡張している。とはいえ、使い方は box2d.js と共通なのでいいかもしれない。ライブラリーとしてどこかで公開されているという感じではなさそう。

##[box2dweb.js](http://code.google.com/p/box2dweb/)

単一ライブラリで、1ファイル。box2d.js にあった欠点はないように見える。

##[box2d.js Emscripten 版](https://github.com/kripken/box2d.js)

Emscripten で C で作られたオリジナルの Box2D を JavaScript でダイレクトに動かしているライブラリー。Chrome や Firefox など Emscripten が動くブラウザーでないとつかえない。ちなみに Emscripten は、JavaScript 内で C 言語を動かすための仕組み。

##まとめ

どれも一長一短...安定して動かすなら box2dweb.js でしょうか。

##3Dの物理演算エンジン

3D だと C のライブラリー由来の ammo.js がいいと思います。ただし、ファイルサイズは 1MB ほど...。ammo.js の欠点を取り除いた cannon.js というライブラリーもあり、こちらは、スクラッチで開発されている途中です。また、別で jiglib.js というライブラリーも有名です。