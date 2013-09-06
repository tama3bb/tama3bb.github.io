---
layout: post
title: "AngularJS アプリケーションを国際化するには"
date: 2013-09-05 19:49
comments: true
categories: [AngularJS, angular-translate]
---

---

![angular-translate](http://pascalprecht.github.io/angular-translate/docs/en/img/logo/angular-translate-alternative/angular-translate_alternative_medium2.png)

AngularJS アプリケーションを国際化するには、[angular-translate](http://pascalprecht.github.io/angular-translate/) を使うのがとてもいい感じなので紹介。

簡単な特徴

* 言語ごとにリソースファイルを分けられる
* 表示する言語のファイルだけを非同期に読み込める
* 選択言語を LocalStorage または Cookie に保存してくれる

<!-- more -->

## angular-translate のインストール

Bower でモジュールをインストールし、JS ファイルを適当なとこに置いて HTML から参照させて、AngularJS の依存モジュールとして記述。
```
$ bower install angular-translate
$ bower install angular-translate-storage-cookie
$ bower install angular-translate-storage-local
$ bower install angular-translate-loader-static-files
$ bower install angular-translate-handler-log
```
``` html index.html
<html>
  <head>
    <meta charset="utf-8">
    <title>i18n app</title>
 
    <script src="path/to/angular.min.js"></script>
    <script src="path/to/angular-translate.min.js"></script>
    <script src="path/to/angular-translate-storage-cookie.min.js"></script>
    <script src="path/to/angular-translate-storage-local.min.js"></script>
    <script src="path/to/angular-translate-loader-static-files.min.js"></script>
    <script src="path/to/angular-translate-handler-log.min.js"></script>
    <script src="app.js"></script>
  </head>
 
  <body ng-app="myApp">
 
  </body>
</html>
```
``` javascript app.js
var app = angular.module('myApp', ['pascalprecht.translate']);
```

## $translateProvider の設定

$translateProvider を config で設定。

``` javascript
app.config(['$translateProvider', function($translateProvider) {
  $translateProvider.useStaticFilesLoader({
    prefix: 'assets/i18n/locale-',
    suffix: '.json'
  });
  $translateProvider.preferredLanguage('ja');
  $translateProvider.fallbackLanguage('en');
  $translateProvider.useMissingTranslationHandlerLog();
  $translateProvider.useLocalStorage();
}]);
```

* useStaticFilesLoader でリソースファイルのファイルパスを指定
* preferredLanguage でデフォルトの言語キーを指定
* fallbackLanguage で選択言語にリソースが見つからない場合の言語を指定
* useMissingTranslationHandlerLog でキーに対応するリソースが見つからない場合に console 出力
* useLocalStorage で選択言語の保存先として localStorage を指定（非対応ブラウザでは Cookie に保存される）

```
assets/i18n/
├── locale-en.json
└── locale-ja.json
```
リソースファイルは、en や ja などの言語キーの前（prefix）と後（suffix）を指定。

## リソースの記述方法

JSON オブジェクトとして記述。ネストもできる。
``` javascript assets/i18n/locale-en.json
{
  "HEADLINE": "What an awesome module!",
  "PARAGRAPH": "Srsly!",
  "NAMESPACE": {
    "PARAGRAPH": "And it comes with awesome features!"
  }
}
```

## HTML で利用するには

HTML で利用する場合には、translate フィルタ、または translate ディレクティヴで。

``` html filters
<h2>{% raw %}{{ 'HEADLINE' | translate }}{% endraw %}</h2>
<p>{% raw %}{{ 'PARAGRAPH' | translate }}{% endraw %}</p>
```
``` html directives
<h2 translate>HEADLINE</h2>
<p translate="PARAGRAPH"></p>
```

## controller で利用するには

controller で利用する場合には、$translate サービスで。

``` javascript controllers.js
app.controller('Ctrl', ['$scope', '$translate', function ($scope, $translate) {
  $scope.headline = $translate('HEADLINE');
  $scope.paragraph = $translate('PARAGRAPH');
  $scope.namespaced_paragraph = $translate('NAMESPACE.PARAGRAPH');
}]);
```

## 変数を使った置換

メッセージの一部を置き換えられる。

```
{
  "TRANSLATION_ID": "{% raw %}{{username}}{% endraw %} is logged in."
}
```

``` javascript controllers.js
angular.module('myApp').controller('Ctrl', ['$scope', function ($scope) {
 
  $scope.translationData = {
    username: 'PascalPrecht'
  };
}]);
```

以下のように渡す。
``` javascript controllers.js
$translate('TRANSLATION_ID', $scope.translationData);
```
``` html filters
{% raw %}{{ 'TRANSLATION_ID' | translate:translationData }}{% endraw %}
```
``` html directive
<ANY translate="TRANSLATION_ID" translate-values="{{translationData}}"></ANY>
```

## 言語の切り替え

言語を切り替える場合は、$translate の uses で。

``` javascript controllers.js
angular.module('myApp').controller('Ctrl', ['$translate', '$scope', function ($translate, $scope) {
 
  $scope.changeLanguage = function (langKey) {
    $translate.uses(langKey);
  };
 
}]);
```
こんな感じかな。

``` html
<button ng-click="changeLanguage('ja')" translate="BUTTON_LANG_JA"></button>
<button ng-click="changeLanguage('en')" translate="BUTTON_LANG_EN"></button>
```
``` javascript assets/i18n/locale-ja.json
{
  "BUTTON_LANG_JA": "日本語",
  "BUTTON_LANG_EN": "英語"
}
```

たいへん便利な国際化モジュール、angular-translate の紹介でした。