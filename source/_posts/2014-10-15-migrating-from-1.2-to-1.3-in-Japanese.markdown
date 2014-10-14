---
layout: post
title: "AngularJS: Migrating from 1.2 to 1.3 日本語訳"
date: 2014-10-15 08:24:48 +0900
comments: true
categories: AngularJS
---
[AngularJS 1.3.0 がリリース](http://angularjsninja.com/blog/2014/10/14/angularjs-1.3.0-released/)されたので、移行ガイド ([Migrating from 1.2 to 1.3](https://docs.angularjs.org/guide/migration#migrating-from-1-2-to-1-3)) のほうも日本語訳しておきました。

---

## Migrating from 1.2 to 1.3

### $parse

*[prevent invocation of Function's bind, call and apply](https://github.com/angular/angular.js/commit/77ada4c82d6b8fc6d977c26f3cdb48c2f5fbe5a5)*

angular 式の中では function の `.bind` `.call` `.apply` を呼び出せなくなりました。既存 function の振る舞いを予測できない形で変更させないようにするためです。

*[forbid `__proto__` properties in angular expressions](https://github.com/angular/angular.js/commit/6081f20769e64a800ee8075c168412b21f026d99)*

angular 式の中では (deprecated) `__proto__` プロパティは動作しなくなりました。

*[forbid `__{define,lookup}{Getter,Setter}__` properties](https://github.com/angular/angular.js/commit/48fa3aadd546036c7e69f71046f659ab1de244c6)*

angular 式の中では `__{define,lookup}{Getter,Setter}__` を利用できなくなりました。必要な場合は、危険でなくなるようにラップ／バインドして scope オブジェクトを通して利用してください。

*[forbid referencing Object in angular expressions](https://github.com/angular/angular.js/commit/528be29d1662122a34e204dd607e1c0bd9c16bbc)*

angular 式の中では `Object` を利用できなくなりました。`Object.keys` が必要な場合は scope でアクセスできるようにしてください。

<!-- more -->

### Angular.copy

*[preserve prototype chain when copying objects](https://github.com/angular/angular.js/commit/b59b04f98a0b59eead53f6a53391ce1bbcbe9b57)*

コピー元オブジェクトの prototype をコピー先オブジェクトに適用するように `angular.copy` を変更しています。以前はプロトタイプチェーンのプロパティを直接コピーしていました。

コピー先オブジェクトの `hasOwnProperty` プロパティだけを iterate しても、prototype からのプロパティは含まれなくなり、より適切な振る舞いになっていると考えています。

もしアプリケーションがこの振る舞いに依存している場合は、オブジェクト（と継承プロパティ）のすべてのプロパティを `hasOwnProperty` でフィルタしないように iterate してください。

**この変更は IE 8 で動作しない機能を使っていることに注意してください。**もし IE 8 で動作させたい場合は `Object.create` と `Object.getPrototypeOf` の polyfill を使ってください。

### core

*[drop the toBoolean function](https://github.com/angular/angular.js/commit/bdfc9c02d021e08babfbc966a007c71b4946d69d)*

`f` `0` `false` `no` `n` `[]` は falsy として扱われず、JavaScript の falsy 値である `false` `null` `undefined` `NaN` `0` `""` のみ falsy として扱われるようになりました。

### $compile

*[always error if two directives add isolate-scope and new-scope](https://github.com/angular/angular.js/commit/2cde927e58c8d1588569d94a797e43cdfbcedaf9)*

1 つの要素に isolate scope と別の scope をリクエストするとエラーとなるように変更されました。変更前は、isolate でない scope の directive の次に、isolate な scope の directive という順でコンパイラが適用する場合には、2 つの directive が child scope と isolate scope をリクエストすることが可能でした。

順番にかかわらず、コンパイラはエラーとするようになりました。

`$compile:multidir` エラーとなるようであれば、同じ要素で複数の directive が isolate と isolate でない scope をリクエストしていないかを確認し、コードを修正してください。

### NgModel

*[ensure pattern and ngPattern use the same validator](https://github.com/angular/angular.js/commit/1be9bb9d3527e0758350c4f7417a4228d8571440)*

ng-pattern (`ng-pattern="exp"`) あるいは pattern 属性 (`pattern="{{ exp }}"`) で angular 式が使われて文字列として評価される場合、validator は正規表現オブジェクトのリテラル (`/abc/i`) として文字列を解析せず、文字列全体を正規表現としてしまいます。つまり、フラグが正規表現として正しく扱われません。この制限を回避するために、正規表現オブジェクトを angular 式の値に使用してください。

``` javascript
// before
$scope.exp = '/abc/i';

// after
$scope.exp = /abc/i;
```

### Scope

*[change Scope#id to be a simple number](https://github.com/angular/angular.js/commit/8c6a8171f9bdaa5cdabc0cc3f7d3ce10af7b434d)*

Scope#$id は文字列ではなく数値型となりました。この id は主にデバッグ目的で利用されており、他に影響を与えないものと考えています。

### forEach

*[cache array length](https://github.com/angular/angular.js/commit/55991e33af6fece07ea347a059da061b76fc95f5)*

forEach は配列の初期数だけ iterate するようになり、iteration 中に配列に追加されたアイテムは forEach の対象となりません。

この変更により、forEach が Array#forEach の動作により近くなりました。

### jqLite

*[data should store data only on Element and Document nodes](https://github.com/angular/angular.js/commit/a196c8bca82a28c08896d31f1863cf4ecd11401c)*

テキスト／コメントのノードにも jqLite のデータをセットできていましたが、jQuery と同じように要素とドキュメントのノードのみとなりました。

### $resource

*[allow props beginning with $ to be used on resources](https://github.com/angular/angular.js/commit/d3c50c845671f0f8bcc3f7842df9e2fb1d1b1c40)*

`$resource` がプロパティを削除する挙動を期待している場合、手動で行う必要があります。

### angular.toJson

*[only strip properties beginning with $$, not $](https://github.com/angular/angular.js/commit/c054288c9722875e3595e6e6162193e0fb67a251)*

`toJson` がプロパティを削除する挙動を期待していた場合、手動で行う必要があります。

### $compile

*[deprecate `replace` directives](https://github.com/angular/angular.js/commit/eec6394a342fb92fba5270eee11c83f1d895e9fb)*

要素を置き換える directive 定義の `replace` フラグは、Angular の次のメジャーバージョンで廃止されます。この機能は扱いにくい問題（属性をどのようにマージするか、など）があり、この機能が解決できることよりも多くの問題をもたらしています。また、Web Components では DOM にカスタム要素が存在するのが一般的です。

### $parse

*[remove deprecated promise unwrapping](https://github.com/angular/angular.js/commit/fa6e411da26824a5bae55f37ce7dbb859653276d)*


promise をアンラップする機能は 1.2.0-rc.3 で既に削除されています。

### Scope

*[$broadcast and $emit should set event.currentScope to null](https://github.com/angular/angular.js/commit/82f45aee5bd84d1cc53fb2e8f645d2263cdaacbc)*

`$broadcast` と `$emit` は、イベントの伝播 (propagation) を終了した時点でイベントの `currentScope` プロパティを null にリセットするようになりました。`currentScope` プロパティに非同期にアクセスするコードは、`targetScope` を利用するようにしてください。

### jqLite

*[stop patching individual jQuery methods](https://github.com/angular/angular.js/commit/d71dbb1ae50f174680533492ce4c7db3ff74df00)*

jQuery の `detach()` メソッドは `$destroy` イベントをトリガーしなくなりました。要素に付けた Angular データを破棄したい場合は `remove()` を利用してください。

### $http

*[remove deprecated responseInterceptors functionality](https://github.com/angular/angular.js/commit/ad4336f9359a073e272930f8f9bcd36587a8648f)*


これまでは response interceptor を以下のようにも登録できました。

``` javascript
// register the interceptor as a service
$provide.factory('myHttpInterceptor', function($q, dependency1, dependency2) {
  return function(promise) {
    return promise.then(function(response) {
      // do something on success
      return response;
    }, function(response) {
      // do something on error
      if (canRecover(response)) {
        return responseOrNewPromise
      }
      return $q.reject(response);
    });
  }
});

$httpProvider.responseInterceptors.push('myHttpInterceptor');
```

v1.1.4（4ae46814）で導入された API では以下のようになります。

``` javascript
$provide.factory('myHttpInterceptor', function($q) {
  return {
    response: function(response) {
      // do something on success
      return response;
    },
    responseError: function(response) {
      // do something on error
      if (canRecover(response)) {
        return responseOrNewPromise
      }
      return $q.reject(response);
    }
  };
});

$httpProvider.interceptors.push('myHttpInterceptor');
```

この API の詳細は [interceptors](https://docs.angularjs.org/api/ng/service/$http#interceptors) で確認してください。

### injector

*[invoke config blocks for module after all providers](https://github.com/angular/angular.js/commit/c0b4e2db9cbc8bc3164cedc4646145d3ab72536e)*

config ブロックは provider 登録の前に呼び出されていたため動作を制御可能でしたが、常に config よりも前に provider 登録されるようになったために動作を制御できなくなりました。

例：

以前は、以下のようなコードが動作していました。

``` javascript
angular.module('foo', [])
.provider('$rootProvider', function() {
  this.$get = function() { ... }
})
.config(function($rootProvider) {
  $rootProvider.dependentMode = "B";
})
.provider('$dependentProvider', function($rootProvider) {
  if ($rootProvider.dependentMode === "A") {
    this.$get = function() {
      // Special mode!
    }
  } else {
    this.$get = function() {
      // something else
    }
  }
});
```

`$rootProvider` と `$dependentProvider` の間にある config ブロックがアプリケーションの動作を変更できていましたが、これは今では 1 つのモジュール内では実現できなくなりました。

### ngModelOptions

*[move debounce and updateOn logic into NgModelController](https://github.com/angular/angular.js/commit/adfc322b04a58158fb9697e5b99aab9ca63c80bb)*

このコミットは `NgModelController` の API を変更しています。


* `$setViewValue(value)` - このメソッドは `$viewValue` を変更しますが、これまでとは異なり、`$modelValue` の変更をすぐにはコミットしなくなり、関連する `ngModelOptions` directive で指定されたトリガーによってコミットされるようになりました。`ngModelOptions` に `debounce` で遅延させるトリガーが指定されている場合には、変更のコミットはさらに延期されます。
* `$cancelUpdate()` - `$rollbackViewValue()` に名前が変更されましたが、同じ機能のままで、`$viewValue` の値を `$lastCommittedViewValue` に戻し、ペンディング中の debounce されている更新と、input への再 render の処理をキャンセルします。

`$cancelUpdate()` を利用しているコードは、以下の例に従って移行してください。

前： 

``` javascript
$scope.resetWithCancel = function (e) {
  if (e.keyCode == 27) {
    $scope.myForm.myInput1.$cancelUpdate();
    $scope.myValue = '';
  }
};
```

後： 

``` javascript
$scope.resetWithCancel = function (e) {
  if (e.keyCode == 27) {
    $scope.myForm.myInput1.$rollbackViewValue();
    $scope.myValue = '';
  }
}
```

### $interpolate

*[split .parts into .expressions and .separators](https://github.com/angular/angular.js/commit/88c2193c71954b9e7e7e4bdf636a2b168d36300d)*

`$interpolate` に返される function は `.parts` 配列を持たなくなりました。

代わりに、2 つの配列を持つようになります。

* `.expressions` - interpolate されるテキストの expression 配列。
* `.separators` - interpolation 間を区切る文字列の配列で、この配列はマージしやすくするために、**常に** `.expressions` 配列より 1 アイテム長くなっています。

### $animate

*[insert elements at the start of the parent container instead of at the end](https://github.com/angular/angular.js/commit/1cb8584e8490ecdb1b410a8846c4478c6c2c0e53)*

`$animate` は、親コンテナの最後の要素とする after パラメータをデフォルトとしなくなり、after が指定されていない場合には新しい要素を最初の子要素として挿入するようになりました。

既存のコードを更新する場合には、`$animate.enter()` または `$animate.move()` のすべてのインスタンスを

`$animate.enter(element, parent);`

から

`$animate.enter(element, parent, angular.element(parent[0].lastChild));`

に変更してください。

*[make CSS blocking optional for class-based animations](https://github.com/angular/angular.js/commit/1bebe36aa938890d61188762ed618b1b5e193634)*

トランジションを利用する（class-add や class-remove のような）セットアップ CSS class ベースのアニメーションコードは、スタイルがすぐに適用されるように空の transition 値を与えなければなりません。つまり、アニメーションのコードがセットアップ class で定義されているスタイルを適用し、その CSS class で `transition:0s none` の値が存在しない限りは即座に適用されません。この状況はトランジションがベース CSS クラスに存在し、アニメーションが開始されているケースのことです。

前： 

``` css
.animated.my-class-add {
  opacity:0;
  transition:0.5s linear all;
}
.animated.my-class-add.my-class-add-active {
  opacity:1;
}
```

後： 

``` css
.animated.my-class-add {
  transition:0s linear all;
  opacity:0;
}
.animated.my-class-add.my-class-add-active {
  transition:0.5s linear all;
  opacity:1;
}
```

詳細は ngAnimate のドキュメントで確認してください。

### $compile

*[add support for $observer deregistration](https://github.com/angular/angular.js/commit/299b220f5e05e1d4e26bfd58d0b2fd7329ca76b1)*

`attr.$observe` の呼び出しはオブザーバー function ではなく、登録解除の function を返すようになりました。以下の例に従ってコードを移行してください。

前： 

``` javascript
directive('directiveName', function() {
  return {
    link: function(scope, elm, attr) {
      var observer = attr.$observe('someAttr', function(value) {
        console.log(value);
      });
    }
  };
});
```

後： 

``` javascript
directive('directiveName', function() {
  return {
    link: function(scope, elm, attr) {
      var observer = function(value) {
        console.log(value);
      };

      attr.$observe('someAttr', observer);
    }
  };
});
```

### $httpBackend

*[don't error when JSONP callback called with no parameter](https://github.com/angular/angular.js/commit/6680b7b97c0326a80bdccaf0a35031e4af641e0e)*

空のレスポンスに対する JSONP の動作が変更されました。以前は JSONP レスポンスが空の場合にはエラーとみなされていましたが、適切にイベントをリスンするようになりました。

### build

*[remove IE8 target from all test configs](https://github.com/angular/angular.js/commit/eaa1d00b24008f590b95ad099241b4003688cdda)*

IE 8 はサポートされなくなりました。

### input

*[support types date, time, datetime-local, month, week](https://github.com/angular/angular.js/commit/46bd6dc88de252886d75426efc2ce8107a5134e9)*

type が date、time、datetime-local、month、week の input では、モデルとして常に `Date` オブジェクトが必須となりました。
