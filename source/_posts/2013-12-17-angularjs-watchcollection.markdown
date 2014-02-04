---
layout: post
title: "$watchと$watchの中間的な位置付けの$watchCollection"
date: 2013-12-17 00:01:26 +0900
comments: true
categories: [AngularJS, scope]
---
---
## $watchCollection

[AngularJSのデータバインドを支える$watch](http://angularjsninja.com/blog/2013/12/13/angularjs-watch/) で見たように、$watch ではオブジェクトの参照を監視するか、またはオブジェクトの中身まですべて監視（deep watch）するかを切り替えることができる。

その 2 種類の $watch の中間に位置付けられる [$watchCollection](http://docs.angularjs.org/api/ng.$rootScope.Scope#methods_$watchcollection) というのもあり、1 階層分だけを監視（shallow watch）してくれる。

## 配列の場合

配列の場合に $watch、$watchCollection、および $watch (deep watch) がそれぞれどのように異なるのかを見ていく。

<!-- more -->

### $watch

``` javascript
$scope.results = [ {...}, {...}, ... ];
$scope.$watch('results', function() {...});
```

$watch の場合、参照が変更されたときだけリスナーが動作する。

``` javascript
results[0].title = '';  // 動かない
```

``` javascript
results.push({...});  // 動かない
```

``` javascript
results = [...];  // 動く
```

### $watchCollection

``` javascript
$scope.results = [ {...}, {...}, ... ];
$scope.$watchCollection('results', function() {...});
```

$watchCollection の場合、監視している配列に追加、削除などをした場合にも動作する。

``` javascript
results[0].title = '';  // 動かない
```

``` javascript
results.push({...});  // 動く
```

``` javascript
results = [...];  // 動く
```

### $watch（deep watch）

``` javascript
$scope.results = [ {...}, {...}, ... ];
$scope.$watch('results', function() {...}, true);
```

$watch (deep watch) の場合、なにかしらあれば動作する。

``` javascript
results[0].title = '';  // 動く
```

``` javascript
results.push({...});  // 動く
```

``` javascript
results = [...];  // 動く
```

## 普通のオブジェクトの場合

``` javascript
$scope.user = {
  name: 'unknown',
  images: [...]
};
$scope.$watchCollection('user', function() {...});
```

普通のオブジェクトを対象とした $watchCollection の動作は、監視しているオブジェクトのプロパティ値の変更や、プロパティの追加・削除でも動作する。

``` javascript
user.images.push(...);  // 動かない
```

``` javascript
user.name = 'known';  // 動く
```

``` javascript
user.newProperty = 'new prop!';  // 動く
```

``` javascript
user = {...};  // 動く
```
