---
layout: post
title: "Yeoman generator for AngularJS"
date: 2013-08-31 00:25
comments: true
categories: [AngularJS, Yeoman]
---

---

![Yeoman](https://github-camo.global.ssl.fastly.net/10c0f69d03b7ac184d77e8aaba4358a2d4791823/687474703a2f2f79656f6d616e2e696f2f6d656469612f79656f6d616e2d6d617374686561642e706e67)

## AngularJS 用 Yeoman ジェネレータ

Yeoman チームがベストプラクティスだと言っているプロジェクト構成を、AngularJS 用の Yeoman ジェネレータで実際に生成して確認する。

<!-- more -->


# yo (Yeoman) と generator-angular をインストール

まず yo (Yeoman) をインストール。

```
npm install -g yo
```

次に、AngularJS 用の Yeoman ジェネレータ generator-angular をインストール。

```
npm install -g generator-angular
```


# AngularJS プロジェクトを生成

新しいディレクトリを作って、そのディレクトリに移動。

```
mkdir my-new-project && cd $_
```

`yo angular` を実行（アプリケーション名を付けれる）。

```
yo angular [app-name]
```

で、Twitter Bootstrap を含めるか、SCSS を使うか、angular-resource.js, angular-cookies.js, angular-sanitiaze.js のモジュールを含めるかといった質問に答える。

すると、うお、そんなに？ってぐらいにターミナルが流れに流れて my-new-project ディレクトリに山ほどのサブディレクトリとファイルができている。


# 生成したプロジェクトの構成を確認

とりあえずプロジェクト構成のことだけに絞って、ここでは app ディレクトリの下を確認。

```
app/
├── 404.html
├── bower_components
│   ├── angular
│   ├── angular-cookies
│   ├── angular-mocks
│   ├── angular-resource
│   ├── angular-sanitize
│   ├── angular-scenario
│   ├── bootstrap-sass
│   ├── es5-shim
│   ├── jquery
│   └── json3
├── favicon.ico
├── index.html
├── robots.txt
├── scripts
│   ├── app.js
│   └── controllers
│       └── main.js
├── styles
│   ├── bootstrap.css
│   └── main.css
└── views
    └── main.html
```

目新しいのは bower_components の存在。これは、パッケージマネジメントツール [Bower](http://bower.io) でインストールするときにできるディレクトリの名前。Yeoman は Bower を使っている。

肝心の scripts ディレクトリの下がスッカスカなので、先に進む。


# サブジェネレータでいろいろ生成

```
yo angular:controller myController
yo angular:directive myDirective
yo angular:filter myFilter
yo angular:service myService
```

これで、scripts ディレクトリはこうなる。

```
app/scripts/
├── app.js
├── controllers
│   ├── main.js
│   └── myController.js
├── directives
│   └── myDirective.js
├── filters
│   └── myFilter.js
└── services
    └── myService.js
```


# プロジェクト構成のまとめ

ということで、やっぱり controller、directive、filter、service に分けて、あとはサブディレクトリを作るなり、細かく単機能ごとにファイルを分けていくということで。

ちなみに、どれだけファイルをバラして作ったとしても、[Grunt](http://gruntjs.com) でビルドすれば minify された 1 つの JS ファイルになるので心配ご無用！
