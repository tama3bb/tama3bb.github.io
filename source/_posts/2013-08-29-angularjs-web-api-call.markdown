---
layout: post
title: "AngularJS で Web API を呼び出すコードをまとめる"
date: 2013-08-29 08:42
comments: true
categories: AngularJS
---

---

## AngularJS で Web API を呼び出すコードをまとめる

AngularJS で Web API を呼び出す $http などを利用するコードを、各 controller に散らして実装してしまわずに、どこかに集約しておきたいぞって場合の話。

<!-- more -->

``` javascript services.js
app.factory('NinjaService', function($http) {
  var search = function(params) {
    return $http.get('/search', {params: params}).then(function(response) {
      return response.data;
    });
  };
  return {
    search: search
  };
});
```

``` javascript controllers.js
app.controller('NinjaController', function($scope, NinjaService) {
  $scope.search = function() {
    NinjaService.search(params).then(function(data) {
      $scope.results = data.results;
    });
  };
});
```

こんな感じで、XxxService とかいう名前でサービスを factory 使って宣言しておけば、同じコードを複数の controller で利用したいときに便利だし、Web API 呼び出しのコードがまとまることで見通しやすくなる。

このコードに出てくる params については、search メソッドのクエリパラメータとして検索条件を渡してるイメージなだけで、詳細は省略。
