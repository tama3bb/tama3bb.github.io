---
layout: post
title: "AngularJS で HTML をエスケープさせずに出力するには"
date: 2013-09-04 16:49
comments: true
categories: AngularJS
---

---

# AngularJS での出力手段

AngularJS で単に文字列を出力するには、`{% raw %}{{expression}}{% endraw %}`または[`ng-bind`](http://docs.angularjs.org/api/ng.directive:ngBind)を HTML で利用する。

``` html
<span>{% raw %}{{ninja.name}}{% endraw %}</span>
<span ng-bind="ninja.name"></span>
```

<!-- more -->

# {% raw %}{{expression}}{% endraw %} が一瞬表示されてチラつく場合の対処方法

`{% raw %}{{expression}}{% endraw %}`をエントリーポイントの index.html で利用すると、AngularJS が処理するまで`{% raw %}{{expression}}{% endraw %}`がそのままページに表示され、値が切り替わるときにチラついてしまう。

この問題に対しては、`ng-bind`を利用するか、[`ng-cloak`](http://docs.angularjs.org/api/ng.directive:ngCloak)を以下のように利用することで解決できる。

``` html
<span ng-cloak>{% raw %}{{ninja.name}}{% endraw %}</span>
```

なお、[ng-view](http://docs.angularjs.org/api/ngRoute.directive:ngView) や [ng-include](http://docs.angularjs.org/api/ng.directive:ngInclude) で挿入される断片（partial）の HTML では、このチラつく現象は発生しないため、ng-cloak の記述は不要である。


# HTML をエスケープさせずに出力するには

HTML をエスケープさせずに出力するには、[`ng-bind-html`](http://docs.angularjs.org/api/ng.directive:ngBindHtml)を利用する。

``` html
<span ng-bind-html="ninja.htmlContent"></span>
```

この`ng-bind-html`は、別のモジュール（ngSanitize）に分かれているため、angular-sanitize.min.js を index.html で参照し、依存するモジュールとして記述する必要がある。

``` html index.html
<script src="assets/lib/angular-1.1.5/angular-sanitize.min.js"></script>
```
``` javascript app.js
var app = angular.module('app', ['ngSanitize']);
```


# サニタイズせずに出力するには

出力する内容が安全であるとわかっている場合には、[$sce.trustAsHtml](http://docs.angularjs.org/api/ng.$sce#trustAsHtml) を利用してまったくサニタイズせずに出力することができる。

また、バージョン 1.1 までであれば、ngSanitize モジュールを利用することなく`ng-bind-html-unsafe`を利用できる。
