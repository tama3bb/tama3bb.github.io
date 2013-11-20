---
layout: post
title: "AngularJSのng-classを使いこなそう"
date: 2013-11-12 16:16
comments: true
categories: [AngularJS, directive]
---
---
## ng-class とは
[ng-class](http://docs.angularjs.org/api/ng.directive:ngClass) は、HTML 要素に CSS class 属性値を動的にセットできる directive で、ほんとよく使う。

すでに同じ class 属性値が要素にセットされてるときは、重複しないようになっているあたりもいい感じ。

ng-class の使い方は、スペース区切りの class 文字列`'active disabled'`を保持する変数や、class 属性値文字列の配列`['active', 'disabled']`を保持する変数を指定する。

けれど一番良く使うのは、class 属性値と boolean 値をマッピングした object で、これを ng-class に指定する使い方について紹介。

<!-- more -->

## ng-class の利用例

``` javascript link
scope.isActive = function (matchIdx) {
  return scope.active == matchIdx;
};
scope.selectActive = function (matchIdx) {
  return scope.active = matchIdx;
};
```

{% raw %}
``` html template
<li ng-repeat="match in matches" ng-class="{active: isActive($index)}" ng-mouseenter="selectActive($index)">
```
{% endraw %}

UI Bootstrap の [Typeahead](http://angular-ui.github.io/bootstrap/#/typeahead) から抜き出したコードで、マウスホバーされた行のインデックスを active で保持し、ng-class では`isActive`で boolean を返す isActive を指定していて、ホバー行では`class="ng-scope active"`となり、その他の行では`class="ng-scope"`となるような指定になっている。

ちなみに、`$index`は ng-repeat で利用できるインデックス（0..length-1）で、`ng-scope`は scope ができる要素に AngularJS が自動的に付けてる class。

## ng-class を controller で実装してみる
たくさんの class 属性値を制御したい場合、ng-class の指定がすごく長くなって微妙な気分になってくるので、そんなときは controller のほうに移すのもいいかもしれない（CSS の class が JavaScript 側に行ってしまうのもまた微妙だけれど）。

ソート列のアイコンを変える UI を実現するサンプルコードはこんな感じに。

{% raw %}
``` html
<link href="//netdna.bootstrapcdn.com/font-awesome/3.2.1/css/font-awesome.css" rel="stylesheet">
<script src="//code.angularjs.org/1.2.0/angular.min.js"></script>
<script>
angular.module('Ninja', [])
.controller('SortCtrl', function($scope) {
  $scope.sortField = undefined;
  $scope.ascending = true;
  $scope.sort = function(fieldName) {
    if ($scope.sortField == fieldName) {
      $scope.ascending = !$scope.ascending;
    } else {
      $scope.sortField = fieldName;
      $scope.ascending = true;
    }
  };
  var isSortedBy = function(fieldName) {
    return $scope.sortField === fieldName;
  };
  var isSortedAscending = function(fieldName) {
    return isSortedBy(fieldName) && $scope.ascending;
  };
  var isSortedDescending = function(fieldName) {
    return isSortedBy(fieldName) && !$scope.ascending;
  };
  $scope.iconSort = function(fieldName) {
    return {
      'icon-sort': !isSortedBy(fieldName),
      'icon-sort-up': isSortedAscending(fieldName),
      'icon-sort-down': isSortedDescending(fieldName)
    };
  };
});
</script>
<table ng-app="Ninja" class="demo">
  <thead ng-controller="SortCtrl">
    <tr>
      <th ng-click="sort('name')">
        Name <i ng-class="iconSort('name')"></i>
      </th>
      <th ng-click="sort('modified')">
        Date Modified <i ng-class="iconSort('modified')"></i>
      </th>
      <th ng-click="sort('size')">
        Size <i ng-class="iconSort('size')"></i>
      </th>
      <th ng-click="sort('kind')">
        Kind <i ng-class="iconSort('kind')"></i>
      </th>
    </tr>
  </thead>
</table>
```
{% endraw %}

<link href="//netdna.bootstrapcdn.com/font-awesome/3.2.1/css/font-awesome.css" rel="stylesheet">
<script src="//code.angularjs.org/1.2.0/angular.min.js"></script>
<script>
angular.module('Ninja', [])
.controller('SortCtrl', function($scope) {
  $scope.sortField = undefined;
  $scope.ascending = true;
  $scope.sort = function(fieldName) {
    if ($scope.sortField == fieldName) {
      $scope.ascending = !$scope.ascending;
    } else {
      $scope.sortField = fieldName;
      $scope.ascending = true;
    }
  };
  var isSortedBy = function(fieldName) {
    return $scope.sortField === fieldName;
  };
  var isSortedAscending = function(fieldName) {
    return isSortedBy(fieldName) && $scope.ascending;
  };
  var isSortedDescending = function(fieldName) {
    return isSortedBy(fieldName) && !$scope.ascending;
  };
  $scope.iconSort = function(fieldName) {
    return {
      'icon-sort': !isSortedBy(fieldName),
      'icon-sort-up': isSortedAscending(fieldName),
      'icon-sort-down': isSortedDescending(fieldName)
    };
  };
});
</script>
<table ng-app="Ninja" class="demo">
  <thead ng-controller="SortCtrl">
    <tr>
      <th ng-click="sort('name')">
        Name <i ng-class="iconSort('name')"></i>
      </th>
      <th ng-click="sort('modified')">
        Date Modified <i ng-class="iconSort('modified')"></i>
      </th>
      <th ng-click="sort('size')">
        Size <i ng-class="iconSort('size')"></i>
      </th>
      <th ng-click="sort('kind')">
        Kind <i ng-class="iconSort('kind')"></i>
      </th>
    </tr>
  </thead>
</table>

見どころは 25 行目の`$scope.iconSort`function で return している object で、この`iconSort`を ng-class で使ってソートのアイコン表示を切り替えている。