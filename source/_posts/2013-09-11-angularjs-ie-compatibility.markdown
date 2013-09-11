---
layout: post
title: "AngularJS を古い IE に対応させるには"
date: 2013-09-11 23:58
comments: true
categories: AngularJS
---

---

AngularJS を IE 8 以前の Internet Explorer に対応させるには、[AngularJS: Internet Explorer Compatibility](http://docs.angularjs.org/guide/ie) のページに記載されている手順を実施する必要がある。

<!-- more -->

まず json2.js または json3.js。

``` html index.html
<!doctype html>
<html xmlns:ng="http://angularjs.org">
  <head>
    <!--[if lte IE 8]>
      <script src="/path/to/json2.js"></script>
    <![endif]-->
  </head>
  <body>
    ...
  </body>
</html>
```

次に "ng-app" という値の id 属性を、ng-app 属性を記述する要素に追加。

``` html index.html
<!doctype html>
<html xmlns:ng="http://angularjs.org" id="ng-app" ng-app="optionalModuleName">
  ...
</html>
```

最後に、カスタムタグを使わないようにする。

または、<ng-view> や 独自のカスタムタグ（例えば <alert> や <tabset> など）を使うのであれば、そのためのコード（document.createElement）を追加。

``` html index.html
<!doctype html>
<html xmlns:ng="http://angularjs.org" id="ng-app" ng-app="optionalModuleName">
  <head>
    <!--[if lte IE 8]>
      <script>
        document.createElement('ng-include');
        document.createElement('ng-pluralize');
        document.createElement('ng-view');
 
        // Optionally these for CSS
        document.createElement('ng:include');
        document.createElement('ng:pluralize');
        document.createElement('ng:view');
      </script>
    <![endif]-->
  </head>
  <body>
    ...
  </body>
</html>
```
## それだけで済まない場合も

最後に、さっきのページでは扱われていないものの、$http の delete メソッドを使っているとエラーが発生して動かない。

``` javascript
$http.delete(…) // エラー
$http['delete'](...) // これで OK
```
