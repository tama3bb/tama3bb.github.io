---
layout: post
title: "AngularJS Directive なんてこわくない（その４）"
date: 2013-11-29 15:33:32 +0900
comments: true
categories: [AngularJS, directive]
---
---
## controller, require

[その１](/blog/2013/11/20/angularjs-custom-directives/)、[その２](/blog/2013/11/22/angularjs-custom-directives/)、[その３](/blog/2013/11/27/angularjs-custom-directives/)に引き続き、今回もカスタム directive について。

今回のサンプルコードは、UI Bootstrap の [Tabs](https://github.com/angular-ui/bootstrap/tree/master/src/tabs) からの一部抜粋で、`controller` `require`オプションについて見ていく。

``` javascript tabs.js
angular.module('ui.bootstrap.tabs', [])
.controller('TabsetController', ['$scope', function TabsetCtrl($scope) {
  var ctrl = this,
      tabs = ctrl.tabs = $scope.tabs = [];

  ctrl.select = function(tab) {
    angular.forEach(tabs, function(tab) {
      tab.active = false;
    });
    tab.active = true;
  };
  // ...
}])

.directive('tabset', function() {
  return {
    restrict: 'EA',
    transclude: true,
    replace: true,
    require: '^tabset',
    scope: {},
    controller: 'TabsetController',
    templateUrl: 'template/tabs/tabset.html',
    compile: function(elm, attrs, transclude) {
      return function(scope, element, attrs, tabsetCtrl) {
        // ...
      };
    }
  };
})

.directive('tab', ['$parse', function($parse) {
  return {
    require: '^tabset',
    restrict: 'EA',
    replace: true,
    templateUrl: 'template/tabs/tab.html',
    transclude: true,
    scope: {
      heading: '@',
      onSelect: '&select',
      onDeselect: '&deselect'
    },
    controller: function() {},
    compile: function(elm, attrs, transclude) {
      return function postLink(scope, elm, attrs, tabsetCtrl) {
        // ...
        scope.$watch('active', function(active) {
          setActive(scope.$parent, active);
          if (active) {
            tabsetCtrl.select(scope);
            scope.onSelect();
          } else {
            scope.onDeselect();
          }
        });
        // ...
        scope.select = function() {
          if (!scope.disabled ) {
            scope.active = true;
          }
        };
        tabsetCtrl.addTab(scope);
        scope.$on('$destroy', function() {
          tabsetCtrl.removeTab(scope);
        });
        // ...
      };
    }
  };
}])
```

<!-- more -->

## controller

directive にも`ng-controller`で利用するときに定義するのと同じような`controller`を記述でき、`$scope`や`$http`などをインジェクト（DI）することもできる。directive の場合でも、`controller`では DOM 操作するコードは記述しないようにし、`compile`または `link`のほうに記述する。

なお、directive の`controller`には、モジュールで定義した`controller`の名前を記述することができる。

``` javascript
.directive('tabset', function() {
  return {
    ...,
    controller: 'TabsetController',
    ...
  };
})
```

注意すべき点としては、再利用されるコンポーネントとして directive を作成する場合、`controller`に付ける名前が重複されにくい名前にしておくこと。

`controller`に名前を付けずに、直接 function を記述することもできる。

``` javascript
.directive('tabset', function() {
  return {
    ...,
    controller: ['$scope', function($scope) {
      // ...
    }],
    ...
  };
})
```

`$scope`だけでなく`$http`や`$timeout`などをインジェクト（DI）できる。また、directive の`controller`では`$element` `$attrs` `$transclude`の service をインジェクトできるようになっている。

`link`でも`controller`でも、どちらでも同じような処理を記述することができそうに思う。違う点は、DI を利用できるか否かと、処理のタイミング（`controller`が先で、`link`が後）。使い分けのヒントとしては、子要素など別の directive から呼び出すのであれば`controller`として API を公開する感じで実装し、そうでなければ`link`で実装するという感じで。

## require

ネストされた directive から親の directive の`controller`で定義された API を呼び出すには`require`が必要となる。

上のコード例では、`require: '^tabset'`の記述があり、これによって`tabset` directive の`controller`である`tabsetCtrl`を参照して API を利用できるようになる。

``` javascript
compile: function(elm, attrs, transclude) {
  return function postLink(scope, elm, attrs, tabsetCtrl) {
    tabsetCtrl.select(scope);
    tabsetCtrl.addTab(scope);
    tabsetCtrl.removeTab(scope);
  };
}
```

なお、`^`を付けない場合、親階層ではなく directive を指定した要素の`controller`を探すこととなる。このケースでよく使うのは`require: 'ngModel'`で、directive と同じ要素に`ng-model="..."`の記述があることを前提として実装できることになる。

同じ要素に`ng-model`属性の記述が無い場合、こんなエラーになる。

```
Error: [$compile:ctreq] Controller 'ngModel', required by directive 'input', can't be found!
```

このエラーを発生させる必要が無いなら、`require: '?ngModel'`のように`?`を付けて記述する。

## これでもうカスタム directive を書ける

この４回目で`controller`と`require`を使って、複数の directive でコンポーネントを構成することについての理解も進んだので、どんどん directive を活用していこう！
