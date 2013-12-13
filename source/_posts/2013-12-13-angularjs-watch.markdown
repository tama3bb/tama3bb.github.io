---
layout: post
title: "AngularJS のデータバインドを支える $watch"
date: 2013-12-13 18:43:39 +0900
comments: true
categories: [AngularJS, scope]
---
---
## $watch

AngularJS の強力なデータバインドを支える仕組みのうち、まず [$watch](http://docs.angularjs.org/api/ng.$rootScope.Scope#methods_$watch) について取り上げる。

$watch を使えば、監視（Observe）したいオブジェクトやプロパティが変化したときに実行する処理（リスナー）を容易に記述できる。

$watch を利用する場所は scope のある directive や controller で、ng-model や ng-bind のようなデータバインドする directive を独自に実装する場合や、モデルの変更に応じて処理をバインドする場合などに使用できる。

## $digest サイクル

$watch による変更検知処理は、ポーリング的（一定間隔で頻繁）に実施されるのではなく、以下のイベントが生じたときに $digest サイクル（または $digest ループ）と呼ばれる処理が実行され、その中で実行される。

イベント | 概要
--- | ---
ナビゲーション | ブラウザの location 変更時
ネットワーク | $http, $resource レスポンス受信時
DOM イベント | ng-click, ng-mouseover などの実行時
タイマー | $timeout によるタイマー処理の実行時

<!-- more -->

## 構文

`$watch(watchExpression, listener, objectEquality)`

### watchExpression

監視したいオブジェクトや値（を返す function）を第 1 引数に指定する。

``` javascript
$scope.$watch(function() {
  return $location.path();
}, function() {
  // $location の path が変わった時
});
```

scope にあるオブジェクトや値であれば、文字列で指定できる。

``` javascript
$scope.name = 'unknown';
$scope.$watch('name', function() {
  // scope の name が変わった時
});
```

### listener

watchExpression で監視しているオブジェクトや値が変化したときに実行するリスナー function を第 2 引数に指定する。

変更後の値だけでなく、変更前の値を参照することもできる。

``` javascript
$scope.name = 'unknown';
$scope.$watch('name', function(newVal, oldVal) {
  // newVal: 変更後の値: 'Hanzo'
  // oldVal: 変更前の値: 'unknown'
});
$scope.name = 'Hanzo';
```

なお、このリスナー function についても、scope に定義があれば文字列で指定できる。

### objectEquality

ここまでのコード例では、すべて $watch の対象となる watchExpression が文字列であったため、変更が常に検知される。

watchExpression がオブジェクトの場合には注意が必要で、この第 3 引数を省略（または false を指定）している場合は reference（同じオブジェクトを参照しているか）で比較されることとなり、オブジェクトのプロパティ値が変わろうと配列の中身が変わろうと、変化したとは扱われない。

このオブジェクト id での比較のほうが高速に処理されるが、どうしてもオブジェクトをプロパティごとに比較したい場合には、第 3 引数 objectEquality に true を指定する。性能の点では不利になるが、オブジェクトや配列の中身が変更されたかを検知できるようになる。

なお、性能だけでなく、新旧比較のためにオブジェクトや配列全体のコピー（angular.copy）を保持することになり、メモリ消費の点でも不利になる。

``` javascript
$scope.user = {
  name: 'unknown',
  gender: 'male'
  …
};
$scope.$watch('user', function(newVal, oldVal) {
  // newVal: user オブジェクト
  // オブジェクトの参照が変わった時、または オブジェクトのいずれかのプロパティが変わった時
  if (newVal) {  // オブジェクトの場合は undefined チェックを
  }
}, true);  // 性能、メモリ消費の点からできるだけ true を指定しない方法を検討すべき
```


単にオブジェクトが持つ特定のプロパティを監視したいだけであれば、以下のように記述しよう。

``` javascript
$scope.$watch('user.name', function(newVal, oldVal) {
  // newVal: user オブジェクト
  // オブジェクトの name プロパティが変わった時
});
```
