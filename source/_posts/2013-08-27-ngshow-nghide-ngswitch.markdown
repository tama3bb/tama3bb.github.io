---
layout: post
title: "非表示（ng-show/ng-hide）にしてもサーバにアクセスしてる…"
date: 2013-08-27 10:33
comments: true
categories: AngularJS
---

---

## ng-show / ng-hide のポイント

ng-show / ng-hide は、HTML 要素を boolean 条件で表示・非表示を制御できる便利な directive で、ほんと使える。

例えば、サムネイルを非表示にしたいときは、ng-show（または ng-hide）に boolean 値を渡すこんなコードを書くだけ。`settings.imageVisible=false`ということ。

``` html
<div ng-show="settings.imageVisible" class="img-polaroid">
  <img ng-src="assets/img/anImage.png">
</div>
```

これでページに表示されなくなって Okay !

なんだけど、表示されてなくても画像ファイルにはアクセスしてるようす…。これは、ng-show / ng-hide の表示制御は、CSS の`display: none;`を要素に適用しているだけだから。jQuery の`hide()`とか、Bootstrap の`class="hide"`と同じ。

<!-- more -->

ホバーですぐに表示したいとかいう場合は、非表示のうちにバックグラウンドで画像を取得しておくメリットがあるので、そんなときにはこれでいい。だけど非表示にしたとこはムダに動いてほしくないってときはどうするか。

---

## ng-switch で解決（1.2 未満）

非表示にしてるところに動作してほしくないときは、ng-show / ng-hide ではなく、ng-switch でうまくいく。ng-switch で非表示となるときは、該当の HTML 要素が DOM からいなくなるので。

``` html
<div ng-switch on="settings.imageVisible">
  <div ng-switch-when="true" class="img-polaroid">
    <img ng-src="assets/img/anImage.png">
  </div>
</div>
```

---

## 1.2 以降は ng-if

AngularJS のバージョン 1.2（現時点では RC1）がリリースされ、今回の例のように boolean で決まるときには、新しく追加された ng-if を使うことになってるんだと思う（未検証）ので、すぐにでも必要なくなる TIPS かもしれない。

ng-if など、1.2 で追加された API のことはまた別の機会に。
