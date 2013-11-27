---
layout: post
title: "AngularJS Directive なんてこわくない（その２）"
date: 2013-11-22 13:37:02 +0900
comments: true
categories: [AngularJS, directive]
---
---

## compile と link

[前回](/blog/2013/11/20/angularjs-custom-directives/)に引き続き、今回もカスタム directive について。

２回目の今回は、`compile`と`link`の使い分けについて。

## compile のサンプルコード

まずは`compile`を利用するサンプルコードを、『[Mastering Web Application Development with AngularJS](http://www.amazon.co.jp/dp/B00EQ67J30)』から抜粋して見ていく。

このコードでは、Bootstrap の理由を前提として、以下に示すように Bootstrap でのボタンデザインに必要な CSS class の値を追加する処理を、`compile`で実装している。

``` javascript
myModule.directive('button', function() {
  return {
    restrict: 'E',
    compile: function(element, attrs) {
      element.addClass('btn');
      if (attrs.type === 'submit') {
        element.addClass('btn-primary');
      }
      if (attrs.size) {
        element.addClass('btn-' + attrs.size);
      }
    }
  };
});
```

* `<button>`の class 属性に`btn`を追加
* `<button type="submit">`のように `type="submit"`がある場合には、class 属性に`btn-primary`を追加
* `<button size="...">`のように size 属性がある場合には、`btn-large`など、`btn-` と size 属性で指定した文字列を組み合わせた文字列を class 属性に追加

コードからわかるように、compile オプションの function 第１引数にある`element`は、jQuery（jqLite）オブジェクトになっているので、直接 jQuery の API を利用できる。つまり`$(element)`や、`$(button)`などとしてやる必要はない。

第２引数の`attrs`では`attrs.size`のようにドット区切りで属性値にアクセスできるので、HTML テンプレート側の値を簡単に参照することができる。

<!-- more -->

## compile の流れ

AngularJS が HTML テンプレートをコンパイルするために、標準の directives とカスタム定義した directives のすべてを、DOM の要素・属性・コメント・CSS class から探し出す。各 directive の`compile`function を呼び出し、`compile`function から返される`link`function を後で呼び出すために集めていく流れとなる。

なお、directives をコンパイルしていく処理の順序は`priority`の大きい順となるが、これについてはまた別の機会に先送り。

重要なポイントとしては、`compile`の処理は scope ができる前の処理であり、`compile`function では scope を利用できない。

すべてのコンパイル処理が終わった後、生成した scope を付けて`link`function を呼び出す流れとなり、この時点で`link`function の scope を利用した DOM との間での双方向バインドが効くことになる。

## compile と link の使い分け

`ng-repeat`の内側にある directive というケースなど、同じ directive が繰り返し使われることになるような場合では、`compile`の function は`ng-repeat`の繰り返しに関係なく一度だけ呼び出されるのに対し、`link`の function はイテレーションのたびに呼び出されることになる。

これは、scope のデータや双方向バインドに依存ぜずに処理できる上記のサンプルコードのようなケースであれば、`compile`を使うことで同じ処理を繰り返す無駄を省くように最適化できることになる。

`compile`と`link`の両方を指定した場合には`link`が無視される仕様になっているため、両方を利用した実装をしたい場合には`compile`function から`link`function を return するよう実装することになる。

## link のサンプルコード

ここから、最もよく利用することになる`link`の書き方について見ていく。

``` javascript
myModule.directive('myDirective', function () {
  return {
    link: function (scope, element) {
      scope.$watch('xxxVar', function () {
        // ...
      });
      scope.$on('xxxEvent', function () {
        // ...
      });
    }
  };
});
```
このように、`link`では、`scope`にあるモデルの変更を検知して処理することや、イベントに応じた処理を記述して、DOM や controller とのやり取りを記述していく。

## いろいろある link の書き方

Directives では`link`の書き方にバリエーションがあるので、ざっと眺めておく。

``` javascript
myModule.directive('returnLinkFunction', function () {
  return function(scope, element, attrs) {
    // ...
  };
});
```
まず、単に`function`を return するだけという書き方ができて、これは`link`プロパティだけを持つオブジェクトを返しているのと同じことになる。`link`以外のプロパティを指定する必要が無ければ、こう書くことでシンプルなコードにできる。

``` javascript
myModule.directive('usingLinkOption', function () {
  return {
    link: function(scope, element, attrs) {
      // ...
    }
  };
});
```
これはオブジェクト（DDO: Directive Definition Object）を返す書き方で、`compile`プロパティを使わない場合の書き方。

``` javascript
myModule.directive('usingCompileOption', function () {
  return {
    compile: function(element, attrs) {
      // ...
      return function(scope, element, attrs) {
        // ...
      };
    }
  };
});
```
最後に、`compile`プロパティを使う場合の書き方で、`compile`function で return している`function`が`link`function として扱われることになる。

## ３回目へ向けて

この２回目で compile と link の使い分けができるようになったので、ここまでの知識でとりあえず directive を使っていろいろ実装していけるんじゃないかと思う。

けれども、`replace` `transclude` `scope` `controller` `require`といった大事なプロパティを抑えてないので、ここで終わったら directive こわいままになっちゃうので、[３回目](/blog/2013/11/27/angularjs-custom-directives/)に続く。