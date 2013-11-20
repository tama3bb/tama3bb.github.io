---
layout: post
title: "jQuery と AngularJS"
date: 2013-10-05 20:32
comments: true
categories: [AngularJS, jQuery]
---
---

## jQuery と AngularJS は併用できるのか？

* jQuery と AngularJS は併用可能
* jQuery メインのサイトで AngularJS を部分的に使用可能

<!-- more -->

## AngularJS と jQuery は併用可能

AngularJS より先に jQuery を読み込ませていればその jQuery が利用される。jQuery を読み込ませていなければ AngularJS が内蔵している jqLite（jQuery の API 互換サブセット）の実装が利用される。

jqLite が実装している jQuery 互換の DOM 操作関連の API は、[AngularJS: element](http://docs.angularjs.org/api/angular.element) で確認できるが、DOM 操作系の主要なメソッドは実装されている。

AngularJS 1.2 では、`bind()`/`unbind()`でなく`on()`/`off()`が利用されるため、jQuery のバージョンは 1.7.1 以降とする必要がある。

AngularJS で jQuery の DOM 操作を実装する場合、controller では要素の追加・削除や表示・非表示などの DOM 操作を実装せずに、AngularJS にビルトイン（標準）の directive を利用するか、自作の directive で DOM 操作を実装しよう。

`ng-repeat`、`ng-show`、`ng-class`など、jQuery で実装していた処理を代替できる directive が多数存在するので、積極的に利用してコード量を減らそう。

## jQuery メインのサイトで AngularJS を部分的に使用可能

jQuery メインに実装してきたサイトで AngularJS を部分的に使うということも可能で、そういう場合には AngularJS の適用範囲（scope）を決めるルート的な directive の`ng-app`を`html`や`body`要素ではなく、必要最小限の範囲を囲う要素に対して記述する。

jQuery で大半を実装しているようなサイトやアプリケーションで、全面的に AngularJS に書き変える決断がすぐにできない場合には、少しずつ部分的に導入して攻めていこう。
