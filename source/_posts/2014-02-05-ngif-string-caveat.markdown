---
layout: post
title: "AngularJSのng-ifで文字列を使うなら !! を付けよう"
date: 2014-02-05 17:44:16 +0900
comments: true
categories: [AngularJS, directive]
---
---
## '0' が表示されない…

`ng-if` あるいは、`ng-show`や`ng-hide`を使うときに、string 値の有無で表示判定を記述することもある。

{% raw %}
``` html
<p ng-if="model.value">{{model.label}}</p>
```
{% endraw %}

そんなとき、`model.value`の値が`'0'`だったらどうなるか。

なんと、表示されない…。

<!-- more -->

## '0' だけじゃない

この現象、`'0'`だけでなく、`'f'` `'no'` `'n'`といった文字列（大文字・小文字問わず）の場合であっても表示されない。

angular.js のコードはこうなってる。

``` javascript
function toBoolean(value) {
  if (typeof value === 'function') {
    value = true;
  } else if (value && value.length !== 0) {
    var v = lowercase("" + value);
    value = !(v == 'f' || v == '0' || v == 'false' || v == 'no' || v == 'n' || v == '[]');
  } else {
    value = false;
  }
  return value;
}
```

## boolean にしちゃっておこう

ということで、文字列の有無で判定させたいだけってときは`!!`を付けときましょう。boolean とか object の場合でも`!!`付ける方針にしとくのがラクでいいかな。

{% raw %}
``` html
<p ng-if="!!model.value">{{model.label}}</p>
```
{% endraw %}
