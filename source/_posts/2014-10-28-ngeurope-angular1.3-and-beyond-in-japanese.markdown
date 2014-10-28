---
layout: post
title: "（日本語訳）ng-europe, Angular 1.3, and beyond"
date: 2014-10-28 16:12:47 +0900
comments: true
categories: [AngularJS, ng-europe]
---

AngularJS 公式ブログの記事 [ng-europe, Angular 1.3, and beyond](http://angularjs.blogspot.jp/2014/10/ng-europe-angular-13-and-beyond.html) を日本語訳しておきました。

---

先週、パリで開催した [ng-europe](http://ngeurope.org) において、Angular の過去、現在、そして未来について発表しました。このブログポストでは、重要なポイントとお知らせについてまとめています。

## Angular 1.3

リリースしたばかりの新しい AngularJS 1.3 では、AngularJS 1.2 には無かった多くの機能と改善が含まれています。

* パフォーマンス：DOM 操作や digest など多くの処理が 3 - 4 倍高速化し、アプリケーションが軽快に動作します。
* フォーム：シンプルな API となり、フォームバリデーションのコードを短く記述できます。
* ARIA サポート：[ngAria](https://docs.angularjs.org/api/ngAria) により、スクリーンリーダーなどの支援ソフトウェアをサポートする適切な ARIA 属性を追加・削除できます。
* Material Design：ユーザインターフェイスデザインとインタラクションのための [Google's specification](http://www.google.com/design/spec/material-design/introduction.html) をほぼ完全に機能するよう実装しています。詳しくは [material.angularjs.org](https://material.angularjs.org) で。

Web アプリケーションを実装しているなら、このバージョンを使ってください。Angular 1.x で構築された Google の 1,600 を超えるアプリケーションで、長期間にわたってこのバージョンを最上位としてサポートしていくことをコミットしています。

Angular 1.x で予定していた新機能と breaking changes については、一部の例外を除いて 2.0 に先送りし、新しい設計をベースに取り組んでいきます。PRs（プルリクエスト）のレビューと issues への返答を続けてはいますが、1.x については、機能よりも安定性、セキュリティ、性能をより重視していきます。

Angular 1.2 をお使いであれば、コードを 1.3 に移行する方法について [migration instructions](https://docs.angularjs.org/guide/migration)（[日本語](http://angularjsninja.com/blog/2014/10/15/migrating-from-1.2-to-1.3-in-Japanese/)）を確認してください。

<!-- more -->

## From 1.3 to 2.0: Angular's Next Step

1 月に開催した [ng-conf](http://ng-conf.org) の [keynote](https://www.youtube.com/watch?v=r1A1VR0ibIQ) で、Angular 2 の計画について発表しました。それ以降の数か月にわたり、Angular にとっての次の進化となるステップについてブレインストーミングを繰り返してきました。3 月には[デザインドキュメントで考えを発表](http://angularjs.blogspot.jp/2014/03/angular-20.html)し、フィードバックから Angular がどう使われ、どう使いたいと考えられているかを確認し、様々なアプローチ、プロトタイプ、ベンチマーク、デザインを繰り返し、ベストを求めてきました。

先週の [ng-europe](http://ngeurope.org) の場で、このリサーチとプロトタイプによる Angular 2.0 のビジョンについて発表し、Angular が本物の Angular（DI、HTML ベースのテンプレート、ディレクティブ、テスタビリティ）となることを見ていただけたと思います。一方で、Angular を近年の Web プラットフォームのシフト（web components や module system など）に適用させ、Angular を著しく高速化して使いやすくするためには、1.x からの段階的なステップでは実現できず、それに伴うデザイン変更が生じることも見ていただきました。

具体的な変更点：

* Angular 1.x の controllers と templates を包含する統一されたコンポーネントモデルにより、概念 (concepts) と定型 (boilerplate) を減らして再利用性を高める。
* scope の概念を見直してシンプルでわかりやすくし、コンポーネント間の責任分担を改善。
* モジュール化されたモバイルファーストなデザインで、エンタープライズレベルのデスクトップアプリケーションのニーズまでスケール。 - 世界人口の 50 % を超える人々が、デスクトップではなくモバイルでインターネットに接続しており、モバイル向けのアプリケーションを開発しやすくしたいと考えています。一方で、エンタープライズ分野ではデスクトップ Web アプリケーションの重要性も残り続けます。
* Web Components サポート。1.x での Web プラットフォームについての前提はもはや有効ではなく、対応させるために Angular を変更していきます。
* ES6 (with easy transpilation to ES5) で構築。つまり、現在のブラウザで未来の JavaScript のコードを書き始めることができます。あるいは、ES5 で Angular 2 アプリケーションを実装することもできます。
* [AtScript を導入](https://docs.google.com/presentation/d/1hr2IM-8G-0RzpB-WY8pLHvxqNggKPzUO0KvEv1IKPws/edit#slide=id.p)。TypeScript シンタックスと ES6 を拡張し、実行時の型とアノテーションを追加することで、大きなチームが大規模なアプリケーションを構築し、ドキュメント化することを支援します。ES6 と同じように、アプリケーションの構築に AtScript は必須ではありません。
* Angular は jqLite や DOM ラッパーに依存しない。DOM は 2009 年以来大幅に改善しており、AngularJS がラッパーに依存する必要はなく、ラッパーを無くすことでパフォーマンスも向上します。必要であればディレクティブに jQuery や他の DOM ライブラリを使うこともできます。

これらのアイデアの多くは、Angular 開発者との[ディスカッション](https://drive.google.com/#folders/0B7Ovm8bUYiUDR29iSkEyMk5pVUk)によるもので、ディスカッションに加わってくださった開発者の方々にあらためて感謝します！ 安定と性能を重視した [AngularJS 1.3 をリリース](http://angularjs.blogspot.jp/2014/10/angularjs-130-superluminal-nudge.html)（[日本語](http://angularjsninja.com/blog/2014/10/14/angularjs-1.3.0-released/)）した今、Angular 2.0 の構築に向けて進んでいきます。

## What does this mean for me?

Angular コミュニティこそが、Angular を素晴らしいものにしています。2.0 の計画を早期に共有することで、コンセプトから実際のコードにしていくためのディスカッションに、多くの方々が参加していただけるようにしています。開発者の方々からの協力を必要としており、考えをお伺いできることを楽しみにしています。GitHub で issues を発行していただくか、Twitter ([Brad](https://twitter.com/bradlygreen), [Igor](https://twitter.com/IgorMinar), [Brian](https://twitter.com/briantford), [Jeff](https://twitter.com/jeffbcross)) や [Google+](https://plus.google.com/+AngularJS/posts) でご連絡ください。ミートアップにお越しいただいたり、[ミーティングノート](https://drive.google.com/#folders/0BxgtL8yFJbacMEZDc2NtWS1VZ1k)のフォロー・コメントもお願いします。

## When can I use Angular 2.0?

現在の実験的な状態にある Angular 2.0 であれば、[GitHub](http://github.com/angular/angular) と[ミーティングノート](https://drive.google.com/#folders/0BxgtL8yFJbacMEZDc2NtWS1VZ1k)でフォローしていただけます。2.0 のコードで何かを構築するには早すぎ、プロジェクトはまだ本当に初期の段階です。まだリリース日を発表できるような状態ではありませんが、初期のバージョンを少しでも早くリリースできるよう進めています。

## How can I learn more about Angular 2.0?

まだ非常に初期の段階ですが、まず ng-europe ([when they are available](https://twitter.com/ngEurope/status/525966523496955904)) のビデオを見ていただくことから始められるのが一番です。特に、2 日間それぞれの Keynote と、Angular 2 Core のセッションです。

時間を掛けて、より深い内容を見ていただけるなら、すべての[デザインドキュメントとリサーチ](https://drive.google.com/#folders/0B7Ovm8bUYiUDR29iSkEyMk5pVUk)に目を通してみてください。

コードは GitHub の [angular/angular](https://github.com/angular/angular) リポジトリにありますが、まだ初期の段階であり、数か月のうちに続々と増えていくことになります。

最後に、パートナーによって公開されているリソースについても確認してください。ES6 と Angular 2.0 についての [TypeScript のブログ記事](http://blogs.msdn.com/b/typescript/archive/2014/10/22/typescript-and-the-road-to-2-0.aspx)や、[Traceur](https://github.com/google/traceur-compiler)、[EcmaScript 6](http://wiki.ecmascript.org/doku.php?id=harmony:specification_drafts)、[Web Components](http://webcomponents.org/) です。

## What about Migrating from 1.3 to 2.0?

Angular 2 のゴールは、既存 API との後方互換性に縛られることなく、Web アプリケーションを構築するための最高のツールセットにすることです。Angular 2 の最初のバージョンに合わせて、Angular 1 ベースのアプリケーションからの移行パスについての作業を始めます。

Angular で Web アプリケーションを実装するために、たくさんの時間を投資して学習してくださっていることを知っています。核となる概念のほとんどを維持していきますので、Angular 2 においても同じ知識を活かして短期間で熟練していただけます。

## What's next?

ng-europe は Angular コミュニティにとって素晴らしいイベントでした。プレゼンテーションやデモは素晴らしく、またそれ以上に、通路などで交わされたすべてのインフォーマルな会話が大切なものでした。そして今、発表したアイデアをリアルなものとしていくために、[ご参加ください](https://github.com/angular/angular/)。2.0 が開発中の間は、Angular 1.3 で素晴らしいアプリケーションを構築してください。
