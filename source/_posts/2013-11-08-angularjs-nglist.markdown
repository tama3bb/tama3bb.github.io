---
layout: post
title: "AngularJSのng-listが便利なようで便利でなく、でもやっぱり便利"
date: 2013-11-08 1:16
comments: true
categories: [AngularJS, directive]
---
---
## ngList とは
[ngList](http://docs.angularjs.org/api/ng.directive:ngList) は、テキストボックスに入力された区切り文字列と、文字列配列のモデルとを相互に変換してくれる directive です。

区切り文字（delimiter）のデフォルトはカンマですが、別の文字列や、正規表現も使えます。

うん、なんだか便利な感じ！

<!-- more -->

## さっそく使ってみたけど、便利じゃない…

* 配列に文字列を追加・削除してもテキストボックスの表示変わらんやん…
* 正規表現はおろか、何を指定してもカンマとして動いてくれちゃうやん…

なんだろう、あきらかに不具合だよ、これは。

てことで、いつものように Stack Overflow に頼る。

[javascript - ng-list input not updating when adding items to array - Stack Overflow](http://stackoverflow.com/questions/15590140/ng-list-input-not-updating-when-adding-items-to-array)

{% blockquote %}
Formatters are only invoked if the value is strictly not equal to the previous value, but since it is the same array instance in your first example, that statement evaluates to false, and hence the text field isn't updated.
{% endblockquote %}

配列の中身が変わっても知らんしって実装になってるから更新されへんねんでってことね。

## でもやっぱり便利だから使えるようにしたい

AngularJS 本体のコードを修正して Pull Req...。いやいや、敷居が高い。

とりあえずの対応としては、配列に文字列を追加・削除するたびに配列を新しくしちゃえば動く。

## デモとサンプルコード

---
<div ng-app>
  <script src="http://codeorigin.jquery.com/jquery-2.0.3.min.js"></script>
  <script src="http://code.angularjs.org/1.2.0-rc.3/angular.min.js"></script>  
  <script>
    function TagCtrl($scope) {
      $scope.tags = [];
      $scope.select = function(tag) {
        if ($scope.contains(tag, $scope.tags)) {
          var tags = [];
          angular.forEach($scope.tags, function(t) {
            if (tag != t) {
              tags.push(t);
            }
          });
          $scope.tags = tags;
        } else {
          $scope.tags = angular.copy($scope.tags);
          $scope.tags.push(tag);
        }
      };
      $scope.contains = function(value, array) {
        return 0 <= $.inArray(value, array);
      };
    }
  </script>
  <div ng-controller="TagCtrl">
    <input type="text" ng-model="tags" ng-list><br>
    <a href="" ng-click="select('Red')">Red</a>    
    <a href="" ng-click="select('Orange')">Orange</a>
    <a href="" ng-click="select('Yellow')">Yellow</a>
    <a href="" ng-click="select('Green')">Green</a>
    <a href="" ng-click="select('Blue')">Blue</a>
    <a href="" ng-click="select('Purple')">Purple</a>
    <a href="" ng-click="select('Gray')">Gray</a>
  </div>
</div>
---

``` html
<div ng-app>
  <script src="http://codeorigin.jquery.com/jquery-2.0.3.min.js"></script>
  <script src="http://code.angularjs.org/1.2.0-rc.3/angular.min.js"></script>  
  <script>
    function TagCtrl($scope) {
      $scope.tags = [];
      $scope.select = function(tag) {
        if ($scope.contains(tag, $scope.tags)) {
          var tags = [];
          angular.forEach($scope.tags, function(t) {
            if (tag != t) {
              tags.push(t);
            }
          });
          $scope.tags = tags;
        } else {
          $scope.tags = angular.copy($scope.tags);
          $scope.tags.push(tag);
        }
      };
      $scope.contains = function(value, array) {
        return 0 <= $.inArray(value, array);
      };
    }
  </script>
  <div ng-controller="TagCtrl">
    <input type="text" ng-model="tags" ng-list><br>
    <a href="" ng-click="select('Red')">Red</a>    
    <a href="" ng-click="select('Orange')">Orange</a>
    <a href="" ng-click="select('Yellow')">Yellow</a>
    <a href="" ng-click="select('Green')">Green</a>
    <a href="" ng-click="select('Blue')">Blue</a>
    <a href="" ng-click="select('Purple')">Purple</a>
    <a href="" ng-click="select('Gray')">Gray</a>
  </div>
</div>
```
## 区切り文字の変更は？

まあ、とりあえずカンマ区切りでヨシということで。ここの [value.join(', ')](https://github.com/angular/angular.js/blob/v1.2.0-rc.3/src/ng/directive/input.js#L1381) がおかしいんだろうと思いつつも、必要になるまで置いとこう。
