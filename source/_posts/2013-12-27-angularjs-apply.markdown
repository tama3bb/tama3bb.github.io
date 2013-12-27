---
layout: post
title: "AngularJS のデータバインドを支える $apply"
date: 2013-12-27 16:19:50 +0900
comments: true
categories: [AngularJS, scope]
---
---
## データバインドが効かない？！

AngualrJS を使っていて楽しいのは超ラクチンなデータバインド。なのに、そのデータバインドで以下のような困ったことに遭遇しているとしたら、それは [$apply](http://docs.angularjs.org/api/ng.$rootScope.Scope#methods_$apply) を学ぶときが来ているということ。

* データバインドが効かないぞ？！
* データの反映が次のイベントまで遅れてる気がする？？

こういうときは $apply の出番だ。$apply を使う必要があるケースというのは、ざくっと言うと AngularJS が知りえないところでイベントが起こっているとき。$apply で AngularJS に変化が起きていることを伝え、後のことは任せることができる。

<!-- more -->

## $apply が必要ないケース

整合性を維持するための dirty checking（$watch）処理は、$digest ループ（サイクル）でまとめて実行される。この $digest ループが始まるきっかけは、『[AngularJS のデータバインドを支える $watch](/blog/2013/12/13/angularjs-watch/)』 のページにも掲載した以下に示す各種イベント。

イベント | 概要
--- | ---
ナビゲーション | ブラウザの location 変更時
ネットワーク | $http, $resource レスポンス受信時
DOM イベント | ng-click, ng-mouseover などの実行時
タイマー | $timeout によるタイマー処理の実行時

こういった処理によってデータや UI に変更があった場合のことは、$apply を自分で記述することなく AngularJS におまかせできる。こうしたイベントでは、内部的に $apply が使われている。

## $apply が必要なケース

じゃあ、どういうときには $apply を自分で記述する必要があるのか。それは、AngularJS 組み込みの services（$http や $timeout など）や directives（ng-click など）を使わない（使えない）とき、ということ。

AngularJS と無関係なところ（jQuery など）で XHR 通信して受け取ったデータをモデルに反映した場合や、あるいは datepicker などのプラグインからモデルに値を反映した場合は、そのモデルと UI が ng-model や ng-bind などでバインドされていたとしても、それだけでは即時には反映されない。反映されるのを、次の $digest サイクルが起こるまでただ待つことになる。

この $digest サイクルを起こす役割が $apply である。

## $interval と setInterval を比較して $apply を理解する

まず、AngularJS 標準 API の $interval を利用している例。これであれば 1 秒毎に日時が更新され続ける。

``` javascript
var update = function() {
  $scope.now = new Date();
};
update();
$interval(update, 1000);
```

次に、setInterval を利用したコード。これだと 1 秒ごとには反映されない。なにかしら $digest ループが起きたタイミングで反映される。

``` javascript
var update = function() {
  $scope.now = new Date();
};
update();
setInterval(update, 1000);
```

setInterval に $apply を付ければ、1 秒ごとに反映されるようになる。

``` javascript
var update = function() {
  $scope.now = new Date();
};
update();
setInterval(function() {
  $scope.$apply(update);
}, 1000);
```
