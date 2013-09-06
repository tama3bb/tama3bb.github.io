---
layout: post
title: "AngularFire でリアルタイムアプリケーション"
date: 2013-09-01 00:29
comments: true
categories: [AngularJS, AngularFire, Firebase]
---

---

![AngularFire](http://angularfire.com/img/afire-logo.png)

Firebase の AngularJS 向け API、[AngularFire](http://angularfire.com) のページが公開されたばかりで、ちょうどいい機会なのでリアルタイムチャットアプリケーションの実装がどれほど簡単にできるのかを見ておく。

<!-- more -->


## AngularFire とは

[AngularFire](http://angularfire.com) とは、Firebase による AngularJS のアプリケーションを高速に実装するためのサービスで、バックエンドのコードを書く必要も、サーバをセットアップする必要も無く、ただフロントエンドの実装に集中できる。

AngularFire の API を利用することで、AngularJS のモデルが自動的に同期（保存・更新）される。AngularJS の強みの 1 つである 2-way データバインドを、サーバ側にまで拡張する 3-way データバインドと言える。


## Firebase とは

[Firebase](https://www.firebase.com) とは、サーバ管理不要で、高速・スケーラブル・リアルタイムなバックエンドを提供するサービス。一定の転送量・接続数・容量まで無料で、クレジッドカード無しで始めることができる。


## 概要

Firebase の URL とモデルを関連付けすることで、アプリケーションを利用しているすべてのクライアント（ブラウザ）を同期させる。

まず、`firebase.js`と`angularfire.js`を参照。

``` html
<script src="https://cdn.firebase.com/v0/firebase.js"></script>
<script src="https://cdn.firebase.com/libs/angularfire/0.3.0/angularfire.js"></script>
```

AngularJS の app モジュールが依存するモジュールとして、`firebase`を記述。

``` javascript
var myapp = angular.module('myapp', ['firebase']);
```


## 同期

Firebase とモデルを同期化させるコード。すべてのローカルでの変更が自動的に Firebase に送信され、すべてのリモートでの変更が即座にローカルのモデルに反映される。

`angularFire`を controller が依存するサービスとして記述。

``` javascript
myapp.controller('MyCtrl', ['$scope', 'angularFire',
  function MyCtrl($scope, angularFire) {
    ...
  }
]);
```

Firebase の参照を、`$scope`のモデルにバインド。

``` javascript
var ref = new Firebase('https://<my-firebase>.firebaseio.com/items');
angularFire(ref, $scope, 'items');
```

マークアップで普通にモデルを利用。

``` html
<ul ng-controller="MyCtrl">
  <li ng-repeat="item in items">{{item.name}}: {{item.desc}}</li>
</ul>
```

Firebase からのデータは非同期にロードされ、サーバからのデータロードの通知には promise が利用できる。ローカルで実施したモデルへの変更は、AngularFire が自動的にリモートのデータとマージする。

モデルのデータ変更についても普通に実装。

``` javascript
// モデルに直接新しいアイテムを追加
$scope.items.push({name: "Firebase", desc: "is awesome!"});
// $scope に function を定義し、directive からのモデル操作も可
$scope.removeItem = function() {
  $scope.items.splice($scope.toRemove, 1);
  $scope.toRemove = null;
};
```
なお、同期のタイミングを制御する API も用意されている。詳しくは、AngularFire の Documentation ページで [Explicit Data Binding](http://angularfire.com/documentation.html#explicit) を参照。


## まとめ

AngularFire の[トップページ](http://angularfire.com)にチャットアプリケーションを実装した 30 行のコードが掲載されている。また、そこでチャットアプリケーションのデモを確認できる。

たったこれだけの簡単なコードで、AngularJS のモデルをサーバに同期させ、すべてのクライアントに即座に同期させるリアルタイムアプリケーションを実装することができる。

AngularJS の勉強会をするなら、ハンズオンの題材としてちょうどよさそう。
