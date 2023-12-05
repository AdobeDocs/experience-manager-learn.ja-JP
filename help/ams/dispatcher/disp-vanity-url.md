---
title: AEM Dispatcher のバニティ URL 機能
description: AEMでバニティ URL を処理する方法と、書き換えルールを使用して、コンテンツを配信のエッジに近づける方法を説明します。
version: 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 53baef9c-aa4e-4f18-ab30-ef9f4f5513ee
duration: 286
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '988'
ht-degree: 39%

---

# Dispatcher バニティー URL

[目次](./overview.md)

[&lt;- 前：Dispatcher のフラッシュ](./disp-flushing.md)

## 概要

このドキュメントでは、AEMでバニティー URL を扱う方法や、書き換えルールを使用してコンテンツを配信の端に近づける方法を説明します

## バニティ URL とは

意味のあるフォルダー構造に存在するコンテンツがある場合、必ずしも参照しやすい URL に存在するとは限りません。 バニティー URL はショートカットのようなものです。 実際のコンテンツが存在する場所を参照する短い、または一意の URL です。

例：`/content/we-retail/us/en/about-us.html` をポイントする `/aboutus`

AEMの作成者は、AEMのコンテンツの一部にバニティ URL プロパティを設定して公開するオプションがあります。

この機能を使用するには、バニティを許可するように Dispatcher フィルターを調整する必要があります。 これは、作成者がこれらのバニティーページエントリを設定する必要がある程度の速度で Dispatcher 設定ファイルを調整すると、不合理になります。

このため、Dispatcher モジュールには、コンテンツツリーにバニティとしてリストされるすべての項目を自動許可する機能があります。


## 仕組み

### バニティ URL の作成

作成者がAEMでページを訪問し、ページのプロパティをクリックして、 _バニティ URL_ 」セクションに入力します。 変更を保存し、ページをアクティベートすると、バニティーがページに割り当てられます。

作成者は、 _バニティ URL をリダイレクト_ 追加時のチェックボックス _バニティ URL_ エントリを渡すと、バニティー url が 302 リダイレクトとして動作します。 つまり、ブラウザーは、（を介して）新しい URL に移動するように求められます。 `Location` 応答ヘッダー ) とブラウザーが、新しい URL に対して新しいリクエストをおこないます。

#### タッチ UI：

![サイトエディター画面の AEM 作成 UI 用のドロップダウンダイアログメニュー](assets/disp-vanity-url/aem-page-properties-drop-down.png "aem-page-properties-drop-down")

![aem ページのプロパティダイアログページ](assets/disp-vanity-url/aem-page-properties.png "aem-page-properties")

#### クラシックコンテンツファインダー：

![AEM siteadmin classic ui サイドキックページプロパティ](assets/disp-vanity-url/aem-page-properties-sidekick.png "aem-page-properties-sidekick")

![クラシック UI のページプロパティダイアログボックス](assets/disp-vanity-url/aem-page-properties-classic.png "aem-page-properties-classic")


>[!NOTE]
>
>これは名前空間の問題が発生しやすいことを理解します。 バニティエントリはすべてのページに対してグローバルです。これは、回避策について計画する必要がある欠点の 1 つですが、これについては後で説明します。


## リソースの解決とマッピング

各バニティエントリは、内部リダイレクトの Sling マップエントリです。

マップは、AEMインスタンスの Felix コンソール ( `/system/console/jcrresolver` )

バニティエントリによって作成されたマップエントリのスクリーンショットを次に示します。
![リソース解決ルール内のバニティーエントリのコンソールスクリーンショット](assets/disp-vanity-url/vanity-resource-resolver-entry.png "vanity-resource-resolver-entry")

上記の例では、AEMインスタンスに `/aboutus` それは解決される `/content/we-retail/us/en/about-us.html`

## Dispatcher の自動許可フィルター

安全な状態の Dispatcher は、JCR ツリーのルートであることが理由で、Dispatcher 経由でパス `/` での要求を除外します。

パブリッシャーがコンテンツを許可しているのが、 `/content` その他の安全な道など、 `/system`.

以下は、`/` のベースフォルダーに格納されているバニティ URL です。安全を確保しながらパブリッシャーにどう到達できるでしょうか。

シンプルな Dispatcher には自動フィルターの許可メカニズムがあり、AEMパッケージをインストールしてから、そのパッケージページを指すように Dispatcher を設定する必要があります。

[https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components](https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components)

Dispatcher のファームファイルに設定セクションがあります。

```
/vanity_urls { 
    /url    "/libs/granite/dispatcher/content/vanityUrls.html" 
    /file   "/tmp/vanity_urls" 
    /delay  300 
}
```

この設定は、300 秒ごとにフロントするAEMインスタンスからこの URL を取得し、許可する項目のリストを取得するよう Dispatcher に指示します。

応答のキャッシュを `/file` この例の引数 `/tmp/vanity_urls`

そのため、URI でAEMインスタンスにアクセスすると、取得内容が表示されます。
![/libs/granite/dispatcher/content/vanityUrls.htmlからレンダリングされたコンテンツのスクリーンショット](assets/disp-vanity-url/vanity-url-component.png "vanity-url-component")

文字通り、非常にシンプルなリストです

## バニティルールとしてのルールの書き換え

上述のように、AEM に組み込まれているデフォルトのメカニズムの代わりに、書き換えルールを使用することが挙げられるのはなぜでしょうか。

簡単に、名前空間の問題、パフォーマンス、処理を改善できる上位レベルのロジックについて説明します。

バニティエントリの例を見てみましょう `/aboutus` その内容に応じて `/content/we-retail/us/en/about-us.html` Apache の使用 `mod_rewrite` モジュールを使用して、これを実行します。

```
RewriteRule ^/aboutus /content/we-retail/us/en/about-us.html [PT,L,NC]
```

このルールはバニティを探します `/aboutus` PT フラグ (Pass Through) を使用してレンダラーからフルパスを取得します。

また、他のすべてのルール L フラグ (Last) の処理を停止します。つまり、JCR Resolving などの大量のルールリストをトラバースする必要はありません。

リクエストをプロキシする必要がなく、AEMパブリッシャーがこのメソッドのこれらの 2 つの要素に応答するのを待つことで、パフォーマンスが大幅に向上します。

次に、ここでのケーキのアイシングは NC フラグ（大文字と小文字を区別しない）です。これは、顧客が `/AboutUs` の代わりに `/aboutus` まだ機能します。

これを実行するために書き換えルールを作成するには、Dispatcher に設定ファイル（例： `/etc/httpd/conf.d/rewrites/examplevanity_rewrite.rules`）を作成し、これらのバニティ URL を適用する必要があるドメインを処理する `.vhost` ファイルに含めます。

以下に、内部に含まれるのコードスニペットの例を示します。 `/etc/httpd/conf.d/enabled_vhosts/we-retail.vhost`

```
<VirtualHost *:80> 
 ServerName weretail.com 
 ServerAlias www.weretail.com 
        ........ SNIP ........ 
 <IfModule mod_rewrite.c> 
  ReWriteEngine on 
  LogLevel warn rewrite:info 
  Include /etc/httpd/conf.d/rewrites/examplevanity_rewrite.rules 
 </IfModule> 
        ........ SNIP ........ 
</VirtualHost>
```

## メソッドと場所

バニティエントリの制御に AEM を使用すると、次のメリットがあります

- 作成者はその場で作成できる
- コンテンツと共に存在し、コンテンツと共にパッケージ化できる

バニティエントリの制御に `mod_rewrite` を使用すると、次のメリットがあります

- コンテンツの解決を迅速に行う
- エンドユーザーコンテンツリクエストのエッジに近い
- 他の条件でのコンテンツのマッピング方法を制御するための拡張性とオプションの増加
- 大文字と小文字を区別しない場合がある

両方の方法を使用しますが、どちらをいつ使用するかについてのアドバイスと条件は以下のとおりです。

- バニティが一時的で、予定されているトラフィックレベルが低い場合は、AEMの組み込み機能を使用します。
- バニティが頻繁に変更されず、頻繁に使用されるステープルエンドポイントの場合は、 `mod_rewrite` ルールを使用します。
- バニティ名前空間（例：`/aboutus`）を同じ AEM インスタンス上の多数のブランドで再利用する必要がある場合は、書き換えルールを使用します。

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>メモ：</b>

AEMバニティ機能を使用し、名前空間を避けたい場合は、命名規則を作成できます。 `/brand1/aboutus`, `brand2/aboutus`, `brand3/aboutus` のようにネストされたバニティ URL の使用 
</div>

[次へ -> 一般的なログ](./common-logs.md)
