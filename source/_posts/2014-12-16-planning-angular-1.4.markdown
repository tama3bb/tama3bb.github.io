---
layout: post
title: "Angular 1.4.0 は 2015 年の春にリリース"
date: 2014-12-16 12:06:25 +0900
comments: true
categories: AngularJS
---

AngularJS 1.4.0 のリリースは来春 2015 年 3 月頃になるようですね。

公式ブログの記事『[Planning Angular 1.4](http://angularjs.blogspot.jp/2014/12/planning-angular-14.html)』を日本語訳しておきました。

---

## Planning Angular 1.4

AngularJS 1.4 についてのミーティングを先週実施し、その概要をまとめています。ミーティングの映像は YouTube でご覧いただけます。

### Release Schedule

最初のリリースとなる 1.4.0 を、2015 年 3 月 5 日の [ng-conf](http://www.ng-conf.org/) にあわせて、2015 年の春を予定しています。1.3.x は継続的にリリースしていきます。

### Work Planning

GitHub でコミュニティの関心が高い issues と PRs から、1.4 への提案機能のリスト [spreadsheet](https://docs.google.com/spreadsheets/d/1ihM8ORf8v38qAbnzJM4TiCqdKeCRz_1VO9JPjSpPWKU/edit?usp=sharing) を Lucas が作成しました。大きな機能や、重大な変更 (breaking changes)、新しい API について記載しています。ミーティングの大半の時間は、これらの機能について Angular 1.x で対応すべきか、どのリリースにするか、誰が担当するかについて話し合いました。

### 1.4 Targets

1.4 で予定している機能について [tracking spreadsheet](https://docs.google.com/spreadsheets/d/1F4JmM25GeaVWLv7oaJrmfFZoE8ioepqNZQKd6xx_Jc4/edit?usp=sharing) を作成しています。

1.4 の主要なテーマは以下のとおりです。

* **Router** - *Brian* - Angular 1 と 2 で対応する新しい router - [Progress](https://docs.google.com/document/d/1-DBXTHaeec6XH5qx2tKVrgrjiILy76_lSrjgJv95RJ4/edit#)
* **I18N** - *Chirayu* - ファーストクラスの国際化対応 - [Design Doc](https://docs.google.com/document/d/1mwyOFsAD-bPoXTk3Hthq0CAcGXCUw-BtTJMR4nGTY-0/edit?usp=sharing)
* **Forms** - *Martin* - 簡単に使えて保守性の高い parsing / formatting / validation - [Design Doc](https://drive.google.com/open?id=1-MhomULgCFOXVsqAGrI7l-cXYMYmJM4BMzgy_anC93o&authuser=0)
* **HTTP** - *Pawel* - `$http` サービスの改善 (serialization, JSON parsing, testing mock DSL)
* **Parser** - *Lucas* - `$parse` サービスのパフォーマンス改善
* **Documentation** - *Caitlin* - Material Design を使ってドキュメントのデザインを再設計

加えて、以下の重要または重大な変更 (breaking changes) を含めることを計画しています。

* **`$injector`** - *Brian* - module を再定義しているとエラーを投げることで、バグを早期に発見 ([#1779](https://github.com/angular/angular.js/issues/1779))
* **`$compile`** - *Igor* - より簡単にコンポーネントタイプの directive を定義するために、新しく module.component ヘルパーを提供 ([#10007](https://github.com/angular/angular.js/issues/10007))
* **`$compile`** - *Caitlin* - オプショナルでない isolated scope のマッピングに対応する属性を記述漏れしているとエラーを投げる ([#9216](https://github.com/angular/angular.js/pull/9216))
* **Project layout / Modularity** - *Pete* - angular.js をさらに小さなオプショナルのモジュール／ファイルに分割し、ノンオプショナルなコアのファイルサイズを小さくする（モバイルのユースケースで有益）

<!-- more -->

## Github Milestones and Labels

master ブランチでの 1.4.x の開発を開始します。進行中の開発用に新しい labels と milestones があります。

*Milestones:*

- 1.4.x - 1.4 で計画されている issues と pull requests 用

*Labels:*

- branch: 1.2.x (replaces stable: yes)
- branch: 1.3.x (replaces stable: no)
- branch: 1.4.x (replaces 1.4 - for triaging 1.4.x issues and PRs)
- Primary Focus: (new for items that we are focusing on for 1.4 - i.e. the stuff in the tracking spreadsheet)

## Other Versions and Backporting

- master ブランチ (1.4.x) はフォーカスの大部分を受け取る
- 1.3.x ブランチはマスターからバックポートしたバージョン特有の修正を受け取る
- 1.2.x ブランチはセキュリティの問題と重要なリグレッションの修正のみ受け取る

## Video

設計と開発について透明かつオープンであるための継続的な活動として、このミーティングの録画を公開しています。[https://www.youtube.com/watch?v=Uae9_8aFo-o](https://www.youtube.com/watch?v=Uae9_8aFo-o)

<iframe width="560" height="315" src="//www.youtube.com/embed/Uae9_8aFo-o?rel=0" frameborder="0" allowfullscreen></iframe>

## Just the Beginning

1.4 の計画はまだ始まったばかりです。上述した概要に加えて、GitHub で 1.4 への追加提案を歓迎しています。1.4.0 をリリース後は、1.4.0 で入らなかった non-breaking な変更のために 1.4.x のリリースを継続していきます。
