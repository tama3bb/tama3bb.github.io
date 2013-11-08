---
layout: post
title: "AngularJSでカスタムfilterを書いてみよう"
date: 2013-11-09 00:01
comments: true
categories: [AngularJS, filter]
---
---
## filter とは
filter は、表示用に値を加工してくれる機能。HTML テンプレートだけでなく、controller や service でも利用できる。また、独自の filter を簡単に定義することもできる。

構文から利用例、そしてカスタム filter のサンプルコードを紹介！

<!-- more -->

## filter の構文
こんなふうに、パイプ記号を使う構文で記述。
```
{% raw %}{{ expression | filter }}{% endraw %}
```
チェーン（chaining）することもできるし、
```
{% raw %}{{ expression | filter1 | filter2 }}{% endraw %}
```
引数を取ることもできる。
```
{% raw %}{{ expression | filter:arg1:arg2 }}{% endraw %}
```

## filter の利用例
たとえば数値をカンマ区切りで表示したければ、AngularJS 標準の [number](http://docs.angularjs.org/api/ng.filter:number) filter を使うだけでラクチン。

```
{% raw %}{{ 123456789 | number }}{% endraw %}
```
123,456,789

標準の filter については、AngularJS [公式サイト](http://docs.angularjs.org/api/ng#filter)のほうで。[filter](http://docs.angularjs.org/api/ng.filter:filter) filter はかなり使えるので要チェック！

## カスタム filter を実装してみる
例として、全角英数字が混じってて見苦しいデータがあったとして、せめて表示の段階ででもスッキリと半角英数字に揃えて表示したいなーということを実現する filter のサンプルコードを。

{% raw %}
``` javascript
<script src="http://code.angularjs.org/1.2.0-rc.3/angular.min.js"></script>
<script>
angular.module('Ninja', [])
.filter('oneByte', function() {
  return function(input) {
    return input.replace(/[Ａ-Ｚａ-ｚ０-９]/g, function(s) {
      return String.fromCharCode(s.charCodeAt(0) - 0xFEE0);
    });
  };
});
</script>
<div ng-app="Ninja">
  {{ val | oneByte }}<br>
  <input type="text" ng-model="val" ng-init="val='Ａｎｇｕｌａｒｊｓ Ninja'">  
</div>
```
{% endraw %}

<script src="http://code.angularjs.org/1.2.0-rc.3/angular.min.js"></script>
<script>
angular.module('Ninja', [])
.filter('oneByte', function() {
  return function(input) {
    return input.replace(/[Ａ-Ｚａ-ｚ０-９]/g, function(s) {
      return String.fromCharCode(s.charCodeAt(0) - 0xFEE0);
    });
  };
});
</script>
<div ng-app="Ninja">
  {% raw %}{{ val | oneByte }}{% endraw %}<br>
  <input type="text" ng-model="val" ng-init="val='Ａｎｇｕｌａｒｊｓ Ninja'">
</div>

いやー、filter 楽しい。でも filter はパフォーマンス的にアレなので、使いすぎにご注意を。