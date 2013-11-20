---
layout: post
title: "AngularJS ではモデルをどう宣言すればいいのか"
date: 2013-08-28 07:48
comments: true
categories: [AngularJS, service]
---

---

## AngularJS の ng-model、大事なのは dot があること

AngularJS では、モデルをどこに宣言すればいいんだってのがわかりにくい。

controller に $scope があって、簡単なサンプルコードでは大抵そこに直接`$scope.name`みたいに記述されているので、同じように書いてしまう。

<!-- more -->

うまくいかないコード例は以下。

``` html
<!-- うまくいかないコード -->
<div ng-controller="ParentController">
  <input type="text" name="parent" ng-model="name">
  <div ng-controller="ChildController">
    <input type="text" name="child" ng-model="name">
  </div>
</div>
```
``` javascript
app.controller('ParentController', function($scope) {});
app.controller('ChildController', function($scope) {});

```
このコードでは、parent テキストボックスに入力した値が child テキストボックスにも同期され、一見うまくいくように思ってしまう。ところが、一度でも child テキストボックスに値を入力すると、別のスコープになってその後まったく同期されなくなる。

うまくいくコードは以下。大事なのは dot があること。dot があることによって、参照が同じになって、両方のテキストボックスの値が同期され続ける。

``` html
<!-- うまくいくコード -->
<div ng-controller="ParentController">
  <input type="text" name="parent" ng-model="aModel.name">
  <div ng-controller="ChildController">
    <input type="text" name="child" ng-model="aModel.name">
  </div>
</div>
```


## AngularJS のモデルは factory で宣言

さて、ちゃんとモデルを宣言したい場合はどう書けばいいのか。さきほどの例で言えば、aModel はどこでどう宣言しておくことができるのか。

答えは factory で、以下のようなコード。

``` javascript
app.factory('aModel', function() {
  return {
    name: ''
  };
});
```

これで、どこにだって DI して使えるようになるので、**親子の関係にない controller 間でも同期**させることができるモデルとなる。

``` javascript
app.controller('ExampleController', function($scope, aModel) {
  $scope.aModel = aModel;
}
```

この factory のモデルはシングルトンです、はい。

`app.controller`とか`app.factory`の app って何なのかとか、DI って何なのかとか基本的なところを飛ばしてますが、その辺はまたあらためて。
