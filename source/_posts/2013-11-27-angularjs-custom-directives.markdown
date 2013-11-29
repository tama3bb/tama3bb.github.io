---
layout: post
title: "AngularJS Directive なんてこわくない（その３）"
date: 2013-11-27 12:02:43 +0900
comments: true
categories: [AngularJS, directive]
---
---

## replace, transclude, scope 

[前々回](/blog/2013/11/20/angularjs-custom-directives/)、[前回](/blog/2013/11/22/angularjs-custom-directives/)に引き続き、今回もカスタム directive について。

今回のサンプルコードは、UI Bootstrap の [Alert](https://github.com/angular-ui/bootstrap/tree/master/src/alert) からで、`replace` `transclude` `scope`オプションについて見ていく。

``` javascript alert.js
angular.module("ui.bootstrap.alert", []).directive('alert', function() {
  return {
    restrict:'EA',
    templateUrl:'template/alert/alert.html',
    transclude:true,
    replace:true,
    scope: {
      type: '=',
      close: '&'
    },
    link: function(scope, iElement, iAttrs) {
      scope.closeable = "close" in iAttrs;
    }
  };
});
```
``` html template/alert/alert.html
<div class="alert" ng-class="type && 'alert-' + type">
    <button ng-show="closeable" type="button" class="close" ng-click="close()">&times;</button>
    <div ng-transclude></div>
</div>
```
{% raw %}
``` html
<alert ng-repeat="alert in alerts" type="alert.type" close="closeAlert($index)">
  {{alert.msg}}
</alert>
```
{% endraw %}

<!-- more -->

## replace

`template`または`templateUrl`で指定された HTML のフラグメントは、デフォルトでは directive を指定した要素の内側に append される。

``` html
<alert ...>
  <div class='alert' ...>
    ...
  </div>
</alert>
```

このコード例のように、directive を要素として指定できるよう`restrict`に`'E'`を含めるときには、HTML として不適当な要素名が記述されることになるため、`replace: true`を指定する前提で directive を設計しよう。

`replace: true` を指定することによって、directive を指定した要素自体を置き換えることができるので、directive がコンパイルされた結果は以下のようなコードとなる。

{% raw %}
``` html
<div ng-class="type && 'alert-' + type" class="alert ng-scope" close="closeAlert($index)" type="alert.type" ng-repeat="alert in alerts">
    <button ng-click="close()" class="close" type="button" ng-show="closeable">×</button>
    <div ng-transclude=""><span class="ng-scope ng-binding">Another alert!</span></div>
</div>
```
{% endraw %}

`replace: true` の場合、 directive を指定した要素にある属性は、テンプレート側のルート要素にコピーされる。なので、`ng-repeat="alert in alerts"`や`type="alert.type"`、`close="closeAlert($index)"`も有効となる。

両方に class 属性があるケースでは、両方の class 属性値をいい感じにマージしてくれる。

## transclude

`transclude`には、`true`または`'element'`を指定でき、directive とした要素の内容を、テンプレートの一部として利用できる。

``` javascript
transclude: true // directive 要素の内容（内側）をテンプレートで利用
transclude: 'element' // directive 要素ごとテンプレートで利用
```

テンプレート側に ng-transclude を指定した要素の内側に append できる。上記サンプルコードの例では、{% raw %}`{{alert.msg}}`{% endraw %}が`<div ng-transclude>`の内側に append される。

上記コード例では、`alert`directive の内側部分{% raw %}`{{alert.msg}}`{% endraw %}が、`ng-transclude`に append されていることがわかる（`alert.msg: 'Another alert!'`という前提）。

## scope

scope には、以下の３種類の指定方法がある。

``` javascript
scope: false // directive が利用される場所での scope を利用（デフォルト）
scope: true // directive が利用される場所での scope を継承する、新たな scope を生成
scope: {...} // directive が利用される場所での scope を継承しない、独立した新たな scope を生成
```

`scope: true`が指定されている代表的な directive は`ng-controller`で、ご存知のとおり`ng-controller`を利用すると scope を継承する新たな scope が生成される。これにより、親の scope にあるデータや function を利用したり、オーバーライドしたりできるようになる。

一方で、コンポーネントとして再利用可能な directive を設計するには、directive を利用する場所での scope による影響を受けない分離・独立した scope を生成したい。

``` javascript
scope: {
  name: '@', // interpolate（値、string）
  info: '=', // data bind
  cancel: '&' // expression（function）
}
```
このように、`scope`にオブジェクトを記述することで、この directive が利用されるたびに`name` `info` `cancel`のみ存在する新たな scope が生成される。

なお、directive を利用する側と、テンプレート側とで異なる名前を使いたいときは、以下のように記述することができる。
``` javascript
scope: {
  customerInfo: '=info'
}
```
こうすることで、テンプレート内では`customerInfo`を、directive を利用する要素での属性名には`info`を利用できるようになる。

## ４回目へ向けて

この３回目で、再利用のための directive を記述する方法が理解できた。もう Directive なんてこわくない！

なんだけど、[４回目](/blog/2013/11/29/angularjs-custom-directives/)でも引き続き、`controller` `require`といった directive のプロパティを扱う。
