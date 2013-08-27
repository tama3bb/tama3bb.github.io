---
layout: post
title: "非表示（ng-show/ng-hide）にしてもサーバにアクセスしてる…"
date: 2013-08-27 10:20
comments: true
categories: AngularJS
---

---

## ng-show / ng-hide のポイント

ng-show / ng-hide は、HTML 要素を boolean 条件で表示・非表示を制御できる便利な directive で、ほんと使える。

例えばサムネイル表示設定が非表示になっているため、サムネイル画像を非表示にする実装は ng-show（または ng-hide）でこんなコードを書くだけ。

``` html
<div ng-show="settings.imageVisible" class="img-polaroid">
  <img ng-src="assets/img/anImage.png">
</div>
```

これでページに表示されなくなって Okay ! と思いきや、表示されてないのにしっかり画像ファイルにアクセスが…。これは、ng-show / ng-hide の表示制御が CSS の`display: none;`を要素に適用して非表示にしているだけなので、jQuery の`hide()`とか、Bootstrap の`class="hide"`と同じ。

<!-- more -->

すぐにでも表示されるような仕様なら非表示のうちに画像を取得しておくメリットがあるので、このままでも問題なし。

---

## ng-switch で解決できる

サーバに画像取得のアクセスが行くのをやめたいときは、ng-show / ng-hide ではなく、ng-switch でうまくいく。ng-switch で非表示となるときは、該当の HTML 要素が DOM からいなくなるので。

``` html
<div ng-switch on="settings.imageVisible">
  <div ng-switch-when="true" class="img-polaroid">
    <img ng-src="assets/img/anImage.png">
  </div>
</div>
```

---

## バージョン 1.2 以降は ng-if

ただ、今回の例のように boolean で決まるときには AngularJS 1.2（現時点では RC1）では新しく追加された ng-if を使うことになると思うので、すぐにでも必要なくなる TIPS かもしれない。

ng-if など 1.2 で追加された API のことはまた別の機会で。
