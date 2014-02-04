---
layout: post
title: "AngularJS で HTTP ヘッダを設定するには"
date: 2014-02-04 16:08:51 +0900
comments: true
categories: [AngularJS, service]
---
---
## HTTP Headers を設定

AngularJS による HTTP リクエストについては、`$httpProvider.defaults.headers`オブジェクトで HTTP ヘッダ情報を設定できる。

``` javascript
$httpProvider.defaults.headers.common.Authorization = '...';
```

以下のように、get だけ、post だけに適用することも。

``` javascript
$httpProvider.defaults.headers.get['Authorization'] = '...';
$httpProvider.defaults.headers.post = { 'Authorization' : '...' };
```

（複数の書き方を例示するために、わざと違った書き方にしてます）

<!-- more -->

## Cache-Control の設定例

IE は XHR (Ajax) リクエストでもキャッシュするようなので、IE をサポートするのであれば初めから Cache-Control などの設定を入れておくのがいいのかも。

``` javascript
$httpProvider.defaults.headers.common['Cache-Control'] = 'no-cache';
```

他にも If-Modified-Since を設定する必要がありそう。あるいは Expires のほうが適切なのかも。ここの設定について詳しい方、こっそり教えてください！
