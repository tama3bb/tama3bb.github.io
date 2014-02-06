---
layout: post
title: "非活性（ng-disabled）にしてもaタグのボタンだとng-click動くので注意"
date: 2014-02-06 15:29:21 +0900
comments: true
categories: [AngularJS, directive]
---
---

## Bootstrap のボタン

[Bootstrap](http://getbootstrap.com) ベースのボタンに`ng-click`を記述する（form を submit しない）コードは、主に以下の 2 通りある。

``` html
<button type="button" class="btn" ng-click="handleClick()">Done</button>
<a href="#" class="btn" ng-click="handleClick()">Done</a>
```

どちらでも見た目は同じになるし、イベント処理は`ng-click`がやってくれるし、`<a>`のほうがコードは短くなるしって感じで、どっちも一緒なんだし`<a>`にしとこってノリでやってると、`disabled` のワナにハマるかも。

## aタグの場合、ng-disabled で非活性にできるのは見た目だけ

ボタンを非活性にするための AngularJS 標準 directive に、[`ng-disabled`](http://docs.angularjs.org/api/ng.directive:ngDisabled)がある。これを使うと、`ng-disabled="true"`となるときにボタンが非活性になってくれる。

``` html
<button type="button" class="btn" ng-click="handleClick()" ng-disabled="aForm.$invalid">Done</button>
<a href="#" class="btn" ng-click="handleClick()" ng-disabled="aForm.$invalid">Done</a>
```

ここで注意が必要なのは、`<a>`のほうは`ng-disabled="true"`で非活性になっているときでも、ボタンクリックで`ng-click`の処理が走るという点。見た目が非活性になっているので、`ng-click`のほうも制御してくれているだろうと期待して（思い込んで）しまう。

そんなわけで、`<a>`タグに`ng-disabled`を使うときは注意しよう。`<button type="button">`に変えるか、どうしても`<a>`タグでいくなら`ng-click="!aForm.$invalid && handleClick()"`とかってしときましょう…。

