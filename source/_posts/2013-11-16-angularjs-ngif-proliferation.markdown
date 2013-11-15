---
layout: post
title: "AngularJSで増殖現象に出会ったらng-ifを疑おう"
date: 2013-11-16 02:11
comments: true
categories: [AngularJS, directive]
---
---
## 増殖現象にビビる

UI Bootstrap の [alert](http://angular-ui.github.io/bootstrap/#/alert) と、[angular-app](https://github.com/angular-app/angular-app) あたりを参考にしながらメッセージ表示機能を実装していたら、どんどんメッセージが増殖してくのでビビった。１件メッセージを追加するたびに、メッセージ配列ごと増えるという…。

## 増殖現象デモ

{% raw %}
<script src="//code.angularjs.org/1.2.0/angular.min.js"></script>
<script src="//cdnjs.cloudflare.com/ajax/libs/angular-ui-bootstrap/0.6.0/ui-bootstrap.min.js"></script>
<script src="//cdnjs.cloudflare.com/ajax/libs/angular-ui-bootstrap/0.6.0/ui-bootstrap-tpls.min.js"></script>
<script>
angular.module('Ninja', ['ui.bootstrap'])
  .controller('NotificationsCtrl', function($scope) {
    $scope.i = 0;
    $scope.notifications = [];
    $scope.addMessage = function(message, type) {
      $scope.notifications.push({
        message: message + $scope.i++,
        type: type || 'error'
      });
    };
  });
</script>
<div ng-app="Ninja" ng-controller="NotificationsCtrl" ng-cloak>
  <a href="" ng-click="addMessage('message:')">Add a message: {{i}}</a>　← 何回かクリック！
  <div ng-if="notifications.length">
    <alert type="notification.type" ng-repeat="notification in notifications">
      {{notification.message}}
    </alert>
  </div>
</div>
{% endraw %}

<!-- more -->

## サンプルコード

{% raw %}
``` html
<script src="//code.angularjs.org/1.2.0/angular.min.js"></script>
<script src="//cdnjs.cloudflare.com/ajax/libs/angular-ui-bootstrap/0.6.0/ui-bootstrap.min.js"></script>
<script src="//cdnjs.cloudflare.com/ajax/libs/angular-ui-bootstrap/0.6.0/ui-bootstrap-tpls.min.js"></script>
<script>
angular.module('Ninja', ['ui.bootstrap'])
  .controller('NotificationsCtrl', function($scope) {
    $scope.i = 0;
    $scope.notifications = [];
    $scope.addMessage = function(message, type) {
      $scope.notifications.push({
        message: message + $scope.i++,
        type: type || 'error'
      });
    };
  });
</script>
<div ng-app="Ninja" ng-controller="NotificationsCtrl" ng-cloak>
  <a href="" ng-click="addMessage('message:')">Add a message: {{i}}</a>
  <div ng-if="notifications.length">
    <alert type="notification.type" ng-repeat="notification in notifications">
      {{notification.message}}
    </alert>
  </div>
</div>
```
{% endraw %}


## ng-if には truthy じゃなく、true / false をちゃんと渡そう

はじめは ng-repeat のバグなのかなと思っていたら、その外側の要素ごと増殖していってることに気付いた。つまり ng-if が怪しい。

上記のコードでの ng-if は、notifications 配列が空っぽだったら要素ごと消しとこうってことで付けている。その ng-if に truthy な（別の）値を渡すと増殖現象になってしまうようだ。

そんなわけで、ちゃんと true / false にして渡しましょう。

``` html
NG: <div ng-if="notifications.length">
OK: <div ng-if="!!notifications.length">
```
てことで、増殖現象に出会ったら、この記事のことを思い出してくださーい。