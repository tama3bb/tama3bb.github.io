---
layout: post
title: "AngularJS 基礎の基礎"
date: 2013-09-02 16:39
comments: true
categories: AngularJS
---

---

# AngularJS 基礎

しばらく AngularJS の基礎的なことを中心に書いていく。

AngularJS の基礎として、まず [AngularJS のページ](http://angularjs.org/#the-basics) で一番初めにあるコードから、AngularJS に関する部分を簡単に。

<!-- more -->

---
<div ng-app>
  <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.0.7/angular.min.js"></script>
  <label>Name:</label>
  <input type="text" ng-model="yourName" placeholder="Enter a name here">
  <hr>
  <h1>Hello {% raw %}{{yourName}}{% endraw %}!</h1>
</div>
---

``` html index.html
<!doctype html>
<html ng-app>
  <head>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.0.7/angular.min.js"></script>
  </head>
  <body>
    <div>
      <label>Name:</label>
      <input type="text" ng-model="yourName" placeholder="Enter a name here">
      <hr>
      <h1>Hello {% raw %}{{yourName}}{% endraw %}!</h1>
    </div>
  </body>
</html>
```

* [`ng-app`](http://docs.angularjs.org/api/ng.directive:ngApp)
  * AngularJS が動作する範囲を指定。
  * ページ全体とする場合は`<html ng-app>`とし、`<head>`を対象外とするなら`<body ng-app>`とする。
  * より限定的に、`<div ng-app>`でも構わない。

* [`ng-model`](http://docs.angularjs.org/api/ng.directive:ngModel)
  * フォームとモデルをリンクし、どちらかでの変更を他方に反映する。
  * この例の場合はテキストボックスへの入力値が即座に yourName プロパティに反映される。

* `{% raw %}{{yourName}}{% endraw %}`
  * yourName プロパティの値を HTML に表示するコード。
  * yourName プロパティの変更が即座に反映される。

この例は、[jsFiddle](http://jsfiddle.net/api/post/library/pure/) でコードを編集して試すことができる。

