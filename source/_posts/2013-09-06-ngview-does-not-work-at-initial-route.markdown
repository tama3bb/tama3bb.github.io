---
layout: post
title: "初回アクセス時やリロード時だけng-viewの部分が表示されない場合の解決方法"
date: 2013-09-06 18:53
comments: true
categories: AngularJS
---

---

ng-view で表示する部分が、初回アクセス時やリロード時だけ表示されない場合の解決方法について。

この現象は、ng-view が ng-include の中に入っている場合に発生してしまうようで、このページの情報のおかげで解決できた。

[Initial route update doesn't happen if ngView in a template loaded by ngInclude](https://github.com/angular/angular.js/issues/1213)

<!-- more -->

コードはこれだけ。

``` javascript app.js
myApp.run(['$route', function($route)  {
  $route.reload();
}]);
```
