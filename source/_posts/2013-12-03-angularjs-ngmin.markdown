---
layout: post
title: "AngularJSのminify対策がngminでラクになる"
date: 2013-12-03 11:15:48 +0900
comments: true
categories: [AngularJS, DI]
---
---
## AngularJS の minify 対策、めんどくさい

AngularJS の JavaScript コードを minify するには、function の引数でインジェクト（DI）する各 services の名前を、文字列として重複させて記述する必要がある。

これって、めっちゃめんどくさい。あほらしすぎる。

## ngmin

そこで [ngmin](https://github.com/btford/ngmin) を使う。

```
npm install -g ngmin
```
```
ngmin somefile.js somefile.annotate.js
```

以下のコードが、

``` javascript somefile.js
angular.module('app', [])

  .controller('TodoCtrl', function($scope, $timeout, Projects) {

    // comments
    $scope.createTask = function(task) {
      task.title = "New Task";
    };
  });
```

ngmin 後は、こうなる。

``` javascript somefile.annotate.js
angular.module('app', []).controller('TodoCtrl', [
  '$scope',
  '$timeout',
  'Projects',
  function ($scope, $timeout, Projects) {
    $scope.createTask = function (task) {
      task.title = 'New Task';
    };
  }
]);
```

<!-- more -->

function の引数に記述している DI する services の名前が、自動的に文字列としても列挙されるので、これで minify 対策がラクになる！

その他、空行・コメント行削除、シングルクオート置換も実施される。

[ngmin](https://github.com/btford/ngmin) は [Grunt](http://gruntjs.com/) タスク（[grunt-ngmin](https://github.com/btford/grunt-ngmin)）としても利用可能で自動化できる！

{% blockquote %}
Life is too short to declare the names of dependencies!
{% endblockquote %}