---
layout: post
title: "AngularJSでちゃちゃっとアニメーションを試す"
date: 2013-12-08 00:20:54 +0900
comments: true
categories: [AngularJS, service]
---
---
## AngularJS 1.2.4

AngularJS 1.2.4 がリリースされ、$animate 関連の [Bug Fixes](https://github.com/angular/angular.js/blob/master/CHANGELOG.md#1.2.4) が入り ng-include をネストした ng-repeat でアニメーションが効かない問題も解消されたので、アニメーションをちゃちゃっと試す方法を紹介。

## angular-animate.js

HTML に angular と angular-animate の js ファイルを記述する。

``` html
<script src="angular.min.js">
<script src="angular-animate.min.js">
```

## ngAnimate モジュール

依存モジュールとして `ngAnimate` を記述する。

``` javascript
angular.module('app', [ 'ngAnimate' ])
```

## CSS 定義

これだけでもうゴール間近で、あとはどんなアニメーションを適用するのかを考えて定義するだけ。

アニメーションを CSS で定義する方法と JavaScript で記述する方法があり、ここではちゃちゃっと試すのが簡単な CSS を例示する。

<!-- more -->

``` css
.ng-enter,
.ng-leave,
.ng-move {
  -webkit-transition: opacity 0.15s linear;
  transition: opacity 0.15s linear;
}
.ng-enter {
  opacity: 0;
}
.ng-enter.ng-enter-active {
  opacity: 1;
}
.ng-leave {
  opacity: 1;
}
.ng-leave.ng-leave-active {
  opacity: 0;
}
.ng-move {
  opacity: .5;
}
.ng-move.ng-move-active {
  opacity: 1;
}
```

この CSS 定義だけで、`enter` `leave` `move`系の`ngRepeat` `ngView` `ngInclude` `ngSwitch` `ngIf` directives に fade（フェード）のアニメーションが適用される。

## アニメーションを限定的に適用

ちゃちゃっとアニメーションを試してみるのにはさっきの CSS で OK だけど、アニメーションされすぎで気持ち悪いとか、アニメーションのせいでむしろ遅い UI に感じられるとか、テーブルタグなどで不自然なレンダリングになるとか…。

なので、ちゃんとアニメーションを使うときには CSS のセレクタに class name（以下では animated）を追加し、適用箇所を限定する。

``` css
.animated.ng-enter,
.animated.ng-leave,
.animated.ng-move {
  -webkit-transition: opacity 0.15s linear;
  transition: opacity 0.15s linear;
}
.animated.ng-enter {
  opacity: 0;
}
.animated.ng-enter.ng-enter-active {
  opacity: 1;
}
.animated.ng-leave {
  opacity: 1;
}
.animated.ng-leave.ng-leave-active {
  opacity: 0;
}
.animated.ng-move {
  opacity: .5;
}
.animated.ng-move.ng-move-active {
  opacity: 1;
}
```

そして、HTML 側でアニメーションさせたい`ng-if`や`ng-repeat`を指定した要素の class 属性に `animated` を追記する。

``` html
<div ng-if="model.visible" class="animated">...</div>
```

むやみやたらにアニメーションするのでなく、こうしてポイントポイントで上品に適用していくことを心掛けよう。

## サンプル

{% raw %}
<a class="jsbin-embed" href="http://jsbin.com/EpiHEwuK/26/embed?output">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>
{% endraw %}

## class 属性の変化

class 属性が変化する流れを見ておく。

`model.visible = false;`（非表示状態）のときがこのようなコードだとして、

``` html
<div ng-if="model.visible" class="animated">...</div>
```

`model.visible = true;`（表示に切替）になるとまず`ng-enter`が追加（`opacity: 0;`）されて、

``` html
<div ng-if="model.visible" class="animated ng-enter">...</div>
```

その後すぐに`ng-enter-active`が追加（`opacity: 1;`）されることでアニメーションが開始する。CSS で定義している `transition: opacity 0.15s linear;`により 0.15s の速度でフェードしながら表示（fadeIn）され、

``` html
<div ng-if="model.visible" class="animated ng-enter ng-enter-active">...</div>
```

要素の class 属性は元に戻る。

``` html
<div ng-if="model.visible" class="animated">...</div>
```

その逆で表示から非表示になるときには、`ng-enter`の代わりに`ng-leave`と`ng-leave-active`が class 属性に追加される。

## アニメーションに対応する標準 directive

以下の AngularJS 標準 directive には、アニメーションのための処理が実装されているので、表示・非表示が切り替わるタイミングで class 属性に先述したような値（`ng-enter`など）が反映される。

Directive | Supported Animations
--- | ---
ngRepeat | enter, leave, move
ngView | enter, leave
ngInclude | enter, leave
ngSwitch | enter, leave
ngIf | enter, leave
ngClass | add, remove
ngShow / ngHide | add, remove (ng-hide class 値)

もちろんカスタム directive でも $animate service を利用して標準 directive と同じようにアニメーションを実現できるけど、その方法についてはまた別の機会に。

## ngAnimate-animate.css

最後に、[animate.css](https://daneden.me/animate/) を AngularJS 1.2 で利用できるようにするドライバーモジュール [ngAnimate-animate.css](https://github.com/yearofmoo/ngAnimate-animate.css) を紹介。

animate.css とこのモジュールを使えば、class 属性に `dn-fade` と記述するだけでフェードのアニメーションを利用できるようになる。その他いろいろなアニメーションも class 属性に指定するだけで試せる。
