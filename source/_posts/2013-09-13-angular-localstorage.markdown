---
layout: post
title: "AngularJS の localStorage モジュール Angular-localStorage"
date: 2013-09-13 23:38
comments: true
categories: AngularJS
---

---

AngularJS アプリケーションで localStorage を使うときに便利なモジュールの紹介。

[agrublev/Angular-localStorage](https://github.com/agrublev/Angular-localStorage)

<!-- more -->

## 機能

* AngularJS の model と localStorage を双方向にバインド
* オブジェクト、配列も変換不要
* localStorage 非対応のブラウザでは $cookies にフォールバック（angular-cookies.min.js を参照し、`ngCookies`を依存モジュールとして記述）

## 使い方

依存モジュールとして`localStorage`を追加。

``` javascript app.js
var yourApp = angular.module('yourApp', [..., 'localStorage']
```

controller に`$store`を記述。

``` javascript controllers.js
yourApp.controller('yourController', function($scope, $store) {
```

`$store`を使う。

``` javascript controllers.js
// $scope.variable にバインド
$store.bind($scope,'varName');

// * defaultValue: デフォルト値
// * storeName: 変数名と異なる localStorage への保存 key を指定
$store.bind($scope, 'varName', {defaultValue: 'randomValue123', storeName: 'customStoreKey'});
```

とにかく便利。
