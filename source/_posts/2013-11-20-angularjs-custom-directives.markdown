---
layout: post
title: "AngularJS Directive なんてこわくない（その１）"
date: 2013-11-20 12:08:23 +0900
comments: true
categories: [AngularJS, directive]
---
---

独自（カスタム）directive の話は複雑でボリュームがあるので、何回かに分けることにして、まずは[前回の ng-include](http://angularjsninja.com/blog/2013/11/19/angularjs-nginclude/) の流れを受けて、とりあえず HTML を分割することから始めてみよう。

## 単にテンプレート部分を Directive にするには

繰り返し出てくるコード部分を、テンプレートとして directive で宣言するサンプルコード。

``` html
<div ninja-profile></div>
```
``` javascript
angular.module('Ninja', [])
  .directive('ninjaCustomer', function() {
    return {
      templateUrl: 'partials/ninja-customer.html'
    };
  });
```
こんだけなので、まあどってことない。これだけなら ng-include のほうがラクでいいやんってことになるかな。でもまあ、とりあえずこんだけしか書かなくても directive として動作するのかってことを見ておく。

<!-- more -->

## 要素として使う Directive としたければ

デフォルトでは属性（`restrict: 'A'`）として使う directive として作られる。なので、要素として HTML で指定する directive にしたければ、`restrict` オプションが必要になる。

``` html
<ninja-customer></ninja-customer>
```
``` javascript
angular.module('Ninja', [])
  .directive('ninjaCustomer', function() {
    return {
      restrict: 'E',
      templateUrl: 'partials/ninja-customer.html'
    };
  });
```
`restrict`オプションを指定する場合の選択肢として、覚えておけばいいのは以下の３種類。`A`は Attribute（属性）で、`E`は Element（要素）。

``` javascript
restrict: 'A' // 属性のみ（デフォルト）
restrict: 'E' // 要素のみ
restrict: 'AE' // 属性または要素
```

属性と要素、どちらを選択するかについては、まとまったコンポーネントとして directive を位置付けるケースでは要素とし、既存の要素に機能を足すようなケースでは属性とするのがいいぽい。

Directive の定義で return するオブジェクトに、オプションとして記述していく記述スタイルで、`restrict`や`templateUrl`の他に、`link`、`replace`、`transclude`、`scope`、`controller`など使えるオプションがあるんだけれど、ひとまず後回し。

## どんなときにカスタム Directive を作るか

Controllers と AngularJS 標準の各種 directive を使えば大抵のことはできてしまうのと、directive の仕様が複雑すぎてわかりにくということで、AngularJS を使っていてもカスタム directive は手付かずだったり、作ってみてもこれでいいのか自信ないなって感じになりがちかなと。

カスタム directive は、例えば jQuery（jqLite）などで直接 DOM を操作したいときや、繰り返し出てくるコードをリファクタリングしてまとめたいときに利用するのがいい。

なお、AngularJS のアプリケーションでも jQuery を利用できることについては、過去エントリ「[jQuery と AngularJS](http://angularjsninja.com/blog/2013/10/05/jquery-to-angularjs/)」に記載しているように、jQuery を先読みすればその jQuery が使え、jQuery 無しでも AngularJS が持つ jQuery のサブセット jqLite で DOM 操作のコードが同じように記述できる。

さらに、複数のプロジェクトでの再利用を目指してコンポーネント化するなら [UI Bootstrap](http://angular-ui.github.io/bootstrap/) のコードを参考に、複雑な directive の作成に取り組んでいきたいところ。

## Directive とは何か

AngularJS で一番強力な機能と言え、HTML が持っていない意味合いや振る舞いを加えるように使うことができる。AngularJS 標準の`ng-bind`や`ng-model`、`ng-view`とかはすべて directive であって、controllers や services に記述したアプリケーションロジックを、属性や要素として DOM 要素にバインドできるようになる。

簡単に言うと、データバインドするモデルやイベントハンドリングするロジックを、DOM に紐付けるためのものということ。

## 標準 Directive

AngularJS には標準（ビルトイン）の directives がたくさん存在し、公式サイトの [ng (core module) ページ Directive](http://docs.angularjs.org/api/ng#directive) には、`ng-include`、`ng-controller`、`ng-click`などよく使う`ng`で始まるものだけでなく、普通の HTML のように見える`a`、`form`や`input`なども directive として記載されている。

たとえば`a`については href 属性値が空のときにはデフォルトの動作が防止されるよう処理されたり、`form`だと name 属性があれば scope のほうでその名前で参照できるように処理されたりする。

form には HTML で form を入れ子にできるよう`ng-form`があったり、`ng-submit`の機能、`ng-invalid`や`ng-dirty`などの機能についても触れたくなるけれど、その辺のことはまた別の機会に。

## Directives の記述

Directives は JavaScript では camelCase で宣言し、HTML では小文字ハイフン区切りの lower-case で参照する。

HTML では４種類の参照方法があるものの、基本的には要素か属性として記述する以下の２種類。

``` html
<my-directive></my-directive>
<div my-directive></div>
```

HTML の validation ツールを使いたければ`data-`を付けておくといい。

``` html
<span ng-bind="name"></span>
<span data-ng-bind="name"></span>
```

## Directives の接頭辞

カスタム directive の名前には、念のため接頭辞（ng を避ける）を付けておくほうがいいかもしれない。将来、偶然 HTML 標準として追加される要素名と重なってしまうケースや、AngularUI や AngularStrap などサードパーティ製の directives と重なってしまうケースもありえるので。

## 次回へ向けて

え、そんなちょっとしか記述しなくても directive として成立してるんだ、ってことを見てもらうのが初回の目的なので、ひとまずここで終了。

こんなとこで終わったら、directive こわいままになっちゃうので、なんとしてでも次回も続けないと。
