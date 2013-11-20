---
layout: post
title: "Hex Color Tool で明度だけを変えたヘックスコードを取得する"
date: 2013-09-09 23:34
comments: true
categories: UI
---

---

![Hex Color Tool](http://hexcolortool.com/images/logo.svg)

ベースカラーを少し明るくした色や、少し暗くした色が欲しくても、ヘックスコードがわかんないってときには、[Hex Color Tool](http://hexcolortool.com/) を使おう。ベースカラーを 5% 刻みで明るく、または暗くした値を表示させることができる。

<!-- more -->

白色の #ffffff を 5% ずつ暗くすると、#f2f2f2, #e6e6e6, #d9d9d9, #cccccc となり、[Twitter Bootstrap](http://getbootstrap.com/) の CSS でも頻繁に指定されている薄いグレー色の値となる。

このツールを使って得たヘックスコードをグラデーションなどに利用して、統一感の取れたカラーデザインのページを実現しよう。

え？ LESS や Sass を使えばいいって？ 実はあんまり好きじゃないんですよね…。

てことで、今回は AngularJS と直接関係ない内容。UI デザインに関したこともちょくちょく書いてくということで。
