---
layout: post
title: "AngularJS Ionic Firebase で作るモバイルチャットアプリ"
date: 2014-02-17 02:15:39 +0900
comments: true
categories: [AngularJS, Ionic, Firebase]
---
この週末、オリンピックを見ながら日本人が出てくるまでヒマだったりしたので、ふとモバイルチャットでも作ってみるかと思い立ってダラダラとやってみました。

数時間しかかけてないので「ザ・未完成」なデキですが、サクッとリアルタイムなモバイルチャットアプリケーションを作れてしまうのは、なんともラクチンで楽しい。

## 利用した技術

* [AngularJS](http://angularjs.org/)
* [Ionic](http://ionicframework.com/)
* [Firebase](https://www.firebase.com/)

Ionic についてはこれまでにブログで、書いてないや…。Firebase については[ずいぶん前に](/blog/2013/09/01/angularfire-realtime-chat-app/)。

<!-- more -->

## モバイルチャットアプリ

リアルタイム処理は Firebase まかせ。見た目は Ionic ベースで少し CSS をいじった程度。認証はとりあえず付けなかったので、サムネイルには AngularJS Ninja のをずっと出しちゃってます。

iPhone や Android から、以下のアドレスを表示して試していただくのがオススメです。

http://ionic-fire.firebaseapp.com/

<iframe src="http://ionic-fire.firebaseapp.com/" height="568" width="320" frameborder="0" style="border: 3px double #aaa;"></iframe>
