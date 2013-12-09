---
layout: post
title: "AngularJSのHTMLバインド {{expression}} ではundefinedを気にしない"
date: 2013-12-10 02:09:47 +0900
comments: true
categories: [AngularJS]
---
---
## Forgiving

[AngularJS: Expressions](http://docs.angularjs.org/guide/expression) ページで [Forgiving](http://docs.angularjs.org/guide/expression#property-evaluation_forgiving) として説明されているように、HTML（テンプレート）で記述する AngularJS のバインド部分（{% raw %}`{{ result.title.value }}`{% endraw %} や`ng-if=“result.tags.length”`）では、result、title、tags が、undefined や null でないかや object かどうかということを考慮したコードにしなくていい。

result が通信してサーバから取得するデータであれば、レスポンスが返るまでの間 result は undefined の状態になるけど、だからと言って{% raw %}`{{ ((result || {}).title || {}).c }}`{% endraw %}とか、`result && result.title && result.title.value`のようにコーディングしなくていい。

<!-- more -->

## サンプル

以下のサンプルでは、`ng-hide="result.hidden"`のとこで、result なんて定義してないので undefined だけど、エラーにならずに falsy として扱われている。

<a class="jsbin-embed" href="http://jsbin.com/oTOMaFIJ/11/embed?html,output">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>