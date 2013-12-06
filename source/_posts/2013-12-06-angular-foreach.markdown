---
layout: post
title: "ObjectでもArrayでも回せるangular.forEach"
date: 2013-12-06 13:55:45 +0900
comments: true
categories: [AngularJS, global APIs]
---
---
## angular.forEach

AngularJS 標準の [Global API](http://docs.angularjs.org/api/ng#function) から、[angular.forEach](http://docs.angularjs.org/api/angular.forEach) の紹介。

angular.forEach は、Object でも Array でも回してくれる。

## angular.forEach(Object, Function)

``` javascript
var user = { name: 'ninja', gender: 'unknown', weapons: [ ..., ... ] };
angular.forEach(user, function(value, key) {
  // ...
});
```

オブジェクトを回す場合の Iterator function の引数は value, key の順。

## angular.forEach(Array, Function)

``` javascript
var records = [ { ... }, { ... } ];
angular.forEach(records, function(record, i) {
  // ...
});
```

配列を回す場合の Iterator function は第１引数が配列の中身で、第２引数が配列インデックスとなる。

<!-- more -->

## context の指定

第３引数に Iteration function での context (this) を指定できる。

以下、公式サイトの [angular.forEach](http://docs.angularjs.org/api/angular.forEach) ページに掲載されているコード。

``` javascript
var values = {name: 'misko', gender: 'male'};
var log = [];
angular.forEach(values, function(value, key) {
  this.push(key + ': ' + value);
}, log);
```

この例だと、第３引数（context）に`log`を渡していて、この`log`が iterator の中での context `this`となる。

