---
layout: post
title: "AngularJSでHTMLを分割するのにお手軽なng-include"
date: 2013-11-19 09:17
comments: true
categories: [AngularJS, directive]
---
---
## AngularJS で HTML を分割するのにお手軽な ng-include

``` html
<div ng-include="'partials/sidebar.html'"></div>
```
``` html
<ng-include src="'partials/sidebar.html'"></ng-include>
```

ポイントとしては、属性値に文字列を渡す必要があって、ダブルクオートの内側にシングルクオートを記述すること。

変数を渡して、可変にもできる。

``` javascript
$scope.sidebarUrl = 'partials/sidebar.html';
```
``` html
<div ng-include="sidebarUrl"></div>
```

`ng-include`で表示する HTML 部分でも、もちろん普通に AngularJS の管理下にあり、データバインドも効く。分割したフラグメント専用に controller の scope を作るなら、`ng-controller`も指定できる。

``` html
<div ng-include="'partials/sidebar.html'" ng-controller="SidebarCtrl"></div>
```
<!-- more -->

## 分割した HTML は $templateCache でキャッシュしてくれる

分割した HTML は、通信して取得した時点で AngularJS が $templateCache で（メモリに）保持するため、分割されている部分を表示するたびに取得する通信が発生するわけではない。

かつ、必要となった時点で取得しにいくレイジーローディングなので、ユーザがまったく表示しない部分であれば、１度も取得しにいかないので効率的でもある。

一方で、Grunt などでビルドする時に、この HTML フラグメントを一つのファイルにしてしまい、一括してロードさせることでネットワークでのロスを下げるという方向で工夫もできる。

スクリプトとして HTML テンプレートを記述する場合は、script 要素に`type`と`id`を指定する。

``` html
<scipt type="text/ng-template" id="templateId.html">
  This is the content of the template
</script>
```

初期処理でテンプレートを`$templateCache`に`put`するようにしておけば、その分だけ操作性の向上も期待できる。

``` javascript
var myApp = angular.module('Ninja', []);
myApp.run(function($templateCache) {
  $templateCache.put('templateId.html', 'This is the content of the template');
});
```

お手軽に断片化できる ng-include の紹介でしたが、HTML を分割する手段としては他にも ng-view の routing で templateUrl を指定することや、custom directive を作成することもできるので、次回以降でその辺も触れる（たぶん）。
