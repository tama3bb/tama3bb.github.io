---
layout: post
title: "AngularJS で Enter キーを検出するコード"
date: 2014-02-03 18:28:23 +0900
comments: true
categories: [AngularJS, directive]
---
---
テキストボックスなどで、Enter キーによるイベント処理を記述するコード例をいくつか。

## $event オブジェクト

`ng-keydown`などの directive を利用する際には、`$event`でイベントオブジェクトを利用。

``` javascript
$scope.comments = [];
$scope.handleKeydown = function(e) {
  if (e.which !== 13) {
    $scope.comments.push($scope.newComment);
    $scope.newComment = '';
  }
}
```

``` html
<input type="text" ng-model="newComment" ng-keydown="handleKeydown($event)">
<ul>
  <li ng-repeat="comment in comments">{{comment}}</li>
<ul>
```

<!-- more -->

## カスタム directive でのイベント処理

カスタム directive を定義する場合は、jQuery と同じような感じでバインド。

``` javascript
myApp.directive('handleKeydown', function() {
  return {
    require: 'ngModel',
    link: function(scope, element, attrs, modelCtrl) {
      element.bind('keydown', function(e) {
        if (e.which === 13) {
          scope.$apply(function() {
            scope.comments.push(scope.newComment);
            scope.newComment = '';
          });
        }
      });
    }
  };
});
```

``` html
<input type="text" ng-model="newComment" handle-keydown>
```

## form（ng-form）の submit

最後に持ってきたものの、form の submit で実現することを真っ先に検討するのが吉。Enter キーも処理されるし、テキストボックスがたくさんあっても無問題。

``` html
<form ng-submit="handleKeydown()">
  <input type="text" ng-model="newComment">
</form>
```
