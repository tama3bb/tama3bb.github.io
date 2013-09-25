---
layout: post
title: "Twitter Bootstrap のモーダルをドラッグで動かしたいときは"
date: 2013-09-25 21:57
comments: true
categories: Bootstrap
---
---

みんな大好き [Twitter Bootstrap](http://getbootstrap.com/2.3.2/) の話。CSS フレームワークの枠を超え、フロントエンド開発のキングというか、デファクトですね。

数年前のプロジェクトで [Blueprint](http://www.blueprintcss.org) をチョイスした自分がバカみたいに、その後の Bootstrap の大流行。[Bootstrap 3](http://getbootstrap.com) も引き続き人気を博するだろうか。

そんな Bootstrap なんだけど、不満があるとすれば、モーダル（[Modals](http://getbootstrap.com/2.3.2/javascript.html#modals)）を動かせないこと！ モーダルダイアログがなんでドラッグで動かないんだ！

<!-- more -->

## 困ったときはググるでしょ

てことで、`bootstrap modal drag`とかでググればアッサリ答えにありつける。いつもながら Stack Overflow はすごいなあ、助かるなあと感謝しながらおもむろにページを開く。

[Twitter Bootstrap Modal Form: How to drag and drop? - Stack Overflow](http://stackoverflow.com/questions/12591597/twitter-bootstrap-modal-form-how-to-drag-and-drop)

答えとしては、[jQuery UI](http://jqueryui.com/) の [Draggable](http://jqueryui.com/draggable/) を使えということ。

``` javascript
$("#myModal").draggable({
    handle: ".modal-header"
});
```

そんだけー。

jQuery UI 全体のコードはいらないだろうから、そんなときは Draggable だけ選択してダウンロードしましょう。

## UI Bootstrap と AngularStrap

ちょっと脱線して、AngularJS のことも。

AngularJS と Bootstrap を併用するときは [UI Bootstrap](http://angular-ui.github.io/bootstrap/) か、[AngularStrap](http://mgcrea.github.io/angular-strap/) を使うと、Bootstrap のコンポーネントが AngularJS の directive として定義されているのでとてもラク。自分で記述するコードがすごく少なくなって快適。

ただ、この UI Bootstrap と AngularStrap は、それぞれいいとこも微妙なとこもあるので両方を使いたいと欲張っちゃうこともある。すると今回のメインである Modal でエラーが…。

Modal に限れば UI Bootstrap のほうが優れてるので、AngularStrap の $modal directive コードを削除しちゃいましょう。
