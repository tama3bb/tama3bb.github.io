---
layout: post
title: "AngularJS のプロジェクト構成をどうするか"
date: 2013-08-30 20:0	5
comments: true
categories: [AngularJS, Yeoman]
---

---

# AngularJS のプロジェクト構成ってどうするのがいいんだ？

AngularJS でアプリケーションを作るときに悩むのがプロジェクト構成。なんでもかんでも一つの JavaScript ファイルで実装してしまうことも可能だけれど、コード量が増えてくるとすぐにつらくなる。

そこでどうしようかなと考えるときに参照するであろう一つが [AngularJS チーム](https://github.com/angular?tab=members) による AngularJS Web アプリケーションのスケルトンプロジェクト [angular-seed](https://github.com/angular/angular-seed)。

<!-- more -->


# Angular Seed

この angular-seed のプロジェクト構成はこんな感じ。

```
app
├─ css
│  └─ app.css
├─ img
├─ js
│  ├─ app.js
│  ├─ controllers.js
│  ├─ directives.js
│  ├─ filters.js
│  └─ services.js
├─ lib
│  └─ angular
│     └─ angular.js
├─ partials
│  ├─ partial1.html
│  └─ partial2.html
├─ index-async.html
└─ index.html
```

簡単に言うと、AngularJS の機能である controller、directive、filter、service と機能別にファイルを分けるということ。

前回の[モデル定義の記事](/blog/2013/08/28/how-to-declare-models/)、前々回の[サービス定義の記事](/blog/2013/08/29/angularjs-web-api-call/)で利用した factory は service の仲間なので services.js に書くことになる。

``` html index.html
  <!-- In production use:
  <script src="//ajax.googleapis.com/ajax/libs/angularjs/1.0.7/angular.min.js"></script>
  -->
  <script src="lib/angular/angular.js"></script>
  <script src="js/app.js"></script>
  <script src="js/services.js"></script>
  <script src="js/controllers.js"></script>
  <script src="js/filters.js"></script>
  <script src="js/directives.js"></script>
</body>
```

まあ、これでもいいんだけれど、大規模になってくるともっとファイルを分けたくなってくる。

特に partials の下にある HTML テンプレートは分割しているのに、それに対応するコントローラはすべて controllers.js に記述していくことに抵抗感が出てくる。

また、services.js にモデルを記述するのではなく、モデルごとにファイルを分けてモデルの名前をファイル名に付けたくなる。

そこで、あるべき姿、ベストプラクティスはどんな構成なんだとネットをさまよう。


# Building Huuuuuge Apps with AngularJS

大規模な AngularJS についての記事では [Building Huuuuuge Apps with AngularJS](http://briantford.com/blog/huuuuuge-angular-apps.html) が参考になる。この記事は [Brian Ford](https://twitter.com/briantford) さんによるもの。

ここで推奨されている構成はこんな感じ。

```
root-app-folder
├─ index.html
├─ scripts
│  ├─ controllers
│  │  └─ main.js
│  │  └─ ...
│  ├─ directives
│  │  └─ myDirective.js
│  │  └─ ...
│  ├─ filters
│  │  └─ myFilter.js
│  │  └─ ...
│  ├─ services
│  │  └─ myService.js
│  │  └─ ...
│  ├─ vendor
│  │  ├─ angular.js
│  │  ├─ angular.min.js
│  │  ├─ es5-shim.min.js
│  │  └─ json3.min.js
│  └─ app.js
├─ styles
│  └─ ...
└─ views
   ├─ main.html
   └─ ...
```

で、さらにやるなら、controllers や services にサブディレクトリを、例えば、services/models みたいに作ると。

angular-seed と大きく異なるのは各機能をフォルダにしているところで、controller や service の一つひとつを別のファイルにしやすい。

なぜ lib -> vendor、css -> styles、js -> scripts、partials -> views に変更しているのかは不明。流儀があるのかな。


# Yeoman

また、この Brian Ford さんが関わっている [Yeoman](http://yeoman.io) という Web アプリケーションのワークフローを改善するためのツール群があって、[yeoman/generator-angular](https://github.com/yeoman/generator-angular) という AngularJS 用の Yeoman ジェネレータがある。

Yeoman がベストプラクティスと考えるプロジェクト構成で出力されるので、これを利用し、まず従ってみてから、自分の頭で考えるというのもいいと思う。

この Yeoman を利用したプロジェクト構成のことや、Yeoman のページに出てくる Grunt、Bower などのツール類の話はまたあらためて。
