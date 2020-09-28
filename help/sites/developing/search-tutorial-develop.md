---
title: シンプル検索導入ガイド
description: 簡易検索の実装は、2017 Summit lab AEM Search Demystified の資料です。このページには、このラボの資料が含まれています。 実習のガイド付きツアーについては、このページの「プレゼンテーション」セクションでLabワークブックを表示してください。
topics: development, search
audience: developer
doc-type: tutorial
activity: develop
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '902'
ht-degree: 11%

---


# シンプル検索導入ガイド{#simple-search-implementation-guide}

The Simple search implementation are the materials from the **Adobe Summit lab AEM Search Demystified**. このページには、このラボの資料が含まれています。 実習のガイド付きツアーについては、このページの「プレゼンテーション」セクションでLabワークブックを表示してください。

![検索アーキテクチャの概要](assets/l4080/simple-search-application.png)

## プレゼンテーション資料 {#bookmarks}

* [Labワークブック](assets/l4080/l4080-lab-workbook.pdf)
* [プレゼンテーション](assets/l4080/l4080-presentation.pdf)

## ブックマーク {#bookmarks-1}

### ツール {#tools}

* [インデックスマネージャー](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_oakindexmanager)
* [クエリの説明を実行](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_queryperformance)
* [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/oak%3Aindex/cqPageLucene) //oak:index/cqPageLucene
* [CRXパッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)
* [QueryBuilderデバッガー](http://localhost:4502/libs/cq/search/content/querydebug.html?)
* [Oak Index Definition Generator](https://oakutils.appspot.com/generate/index)

### 章 {#chapters}

*以下のチャプターリンクは、[初期パッケージが次の場所にAEM作成者にインストールされていることを前提としています](#initialpackages)。`http://localhost:4502`*

* [第 1 章](http://localhost:4502/editor.html/content/summit/l4080/chapter-1.html)
* [第 2 章](http://localhost:4502/editor.html/content/summit/l4080/chapter-2.html)
* [第 3 章](http://localhost:4502/editor.html/content/summit/l4080/chapter-3.html)
* [第 4 章](http://localhost:4502/editor.html/content/summit/l4080/chapter-4.html)
* [第 5 章](http://localhost:4502/editor.html/content/summit/l4080/chapter-5.html)
* [第 6 章](http://localhost:4502/editor.html/content/summit/l4080/chapter-6.html)
* [第 7 章](http://localhost:4502/editor.html/content/summit/l4080/chapter-7.html)
* [第 8 章](http://localhost:4502/editor.html/content/summit/l4080/chapter-8.html)
* [第 9 章](http://localhost:4502/editor.html/content/summit/l4080/chapter-9.html)

## パッケージ {#packages}

### 初期パッケージ {#initial-packages}

* [タグ](assets/l4080/summit-tags.zip)
* [シンプル検索アプリケーションパッケージ](assets/l4080/simple.ui.apps-0.0.1-snapshot.zip)

### チャプターパッケージ {#chapter-packages}

* [第1章解決](assets/l4080/l4080-chapter1.zip)
* [第2章解決](assets/l4080/l4080-chapter2.zip)
* [第3章解決](assets/l4080/l4080-chapter3.zip)
* [第4章解決](assets/l4080/l4080-chapter4.zip)
* [第5章の設定](assets/l4080/l4080-chapter5-setup.zip)
* [第5章解決](assets/l4080/l4080-chapter5-solution.zip)
* [第6章解決](assets/l4080/l4080-chapter6.zip)
* [第9章解決](assets/l4080/l4080-chapter9.zip)

## 参照されるマテリアル {#reference-materials}

* [Githubリポジトリ](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/master/simple-search-guide)
* [Sling Model](https://sling.apache.org/documentation/bundles/models.html)
* [Slingモデルエクスポータ](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
* [QueryBuilder API](https://docs.adobe.com/docs/en/aem/6-2/develop/search/querybuilder-api.html)
* [AEM Chromeプラグイン](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode) ([ドキュメントページ](https://adobe-consulting-services.github.io/acs-aem-tools/aem-chrome-plugin/))

## 修正とフォローアップ {#corrections-and-follow-up}

ラボでのディスカッションの修正と説明、および参加者によるフォローアップ質問への回答。

1. **再インデックスを停止する方法**

   AEM Webコンソール/JMXを使用して、IndexStats MBeanで再インデックスを停止で [きます。](http://localhost:4502/system/console/jmx)

   * [http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats](http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats)
      * 「実行」 `abortAndPause()` をクリックして、再インデックス化を中止します。 これにより、が呼び出されるまでインデックスがさらに再インデックス付けされ `resume()` るようにロックされます。
      * を実行す `resume()` ると、インデックス作成プロセスが再開されます。
   * ドキュメント： [https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean](https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean)

2. **oakインデックスが複数のテナントをサポートする方法**

   Oakは、コンテンツツリーから外へのインデックスの配置をサポートし、これらのインデックスはそのサブツリー内のインデックスのみになります。 例えば、コンテンツのインデックスを作成する場合 **`/content/site-a/oak:index/cqPageLucene`** は、を使用してのみコンテンツをインデックス付け **`/content/site-a`します。**

   同様の方法では、のインデックスに対して **`includePaths`** と **`queryPaths`** プロパティを使用 **`/oak:index`**&#x200B;します。 次に例を示します。

   * `/oak:index/siteAcqPageLucene@includePaths=/content/site-a`
   * `/oak:index/siteAcqPageLucene@queryPaths=/content/site-a`

   このアプローチの考慮事項は次のとおりです。

   * クエリは、インデックスのクエリパススコープと等しいか、またはの子孫であるパス制限を指定する必要があります。
   * 範囲の広いインデックス(例 `/oak:index/cqPageLucene`)もデータのインデックスを作成し、重複した取り込みとディスク使用コストを生み出します。
   * 設定管理の重複が必要な場合がある( 同じクエリセットを満たす必要がある場合、複数のテナントインデックスに同じindexRulesを追加する)
   * この方法は、カスタムサイト検索のAEM Publish層に適しています。AEM Authorと同様、クエリは、様々なテナント（OmniSearch経由など）のコンテンツツリーを高い位置で実行するのが一般的です。インデックス定義が異なると、パス制限に基づいて動作が異なります。


3. **利用可能なすべてのアナライザーのリストはどこですか？**

   Oakは、AEMで使用するLucene提供のアナライザ構成要素のセットを公開します。

   * [Apache Oak Analyzersドキュメント](http://jackrabbit.apache.org/oak/docs/query/lucene.html#analyzers)
      * [トークン化](https://cwiki.apache.org/confluence/display/solr/Tokenizers)
      * [フィルター](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions)
      * [CharFilters](https://cwiki.apache.org/confluence/display/solr/CharFilterFactories)

4. **同じクエリ内でページとアセットを検索する方法**

   AEM 6.3で新たに追加された機能は、同じクエリで複数のノードタイプをクエリできる機能です。 次のQueryBuilderクエリです。 各「サブクエリ」は、独自のインデックスに解決されることに注意してください。この例では、 `cq:Page` サブクエリはに解決され `/oak:index/cqPageLucene` 、サブクエリはに解決され `dam:Asset``/oak:index/damAssetLucene`ます。

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   # add all page restrictions to this group
   group.2_group.type=dam:Asset
   # add all asset restrictions to this group
   ```

   結果として、次のクエリとクエリ計画が作成されます。

   ```plain
   QUERY:(//element(*, cq:Page) | //element(*, dam:Asset))
   
   PLAN: [cq:Page] as [a] /* lucene:cqPageLucene(/oak:index/cqPageLucene) *:* */ union [dam:Asset] as [a] /* lucene:damAssetLucene(/oak:index/damAssetLucene) *:* */
   ```

   QueryBuilder Debugger [および](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Ddam%3AAsset%0D%0A%23+add+all+asset+restrictions+to+this+group) AEM Chromeプラグインを使用して、クエリと結果を調べます [](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=ja-JP)。

5. **同じクエリ内の複数のパスを検索する方法**

   AEM 6.3で新たに追加された機能は、同じクエリで複数のパスをクエリできる機能です。 次のQueryBuilderクエリです。 各「サブクエリ」は、独自のインデックスに解決される可能性があることに注意してください。

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   group.1_group.path=/content/docs/en/6-2
   # add all page restrictions to this group
   group.2_group.type=cq:Page
   group.2_group.path=/content/docs/en/6-3
   # add all asset restrictions to this group
   ```

   結果として、以下のクエリ・クエリ計画が得られる

   ```plain
   QUERY: (/jcr:root/content/docs/en/_x0036_-2//element(*, cq:Page) | /jcr:root/content/docs/en/_x0036_-3//element(*, cq:Page))
   
   PLAN: [cq:Page] as [a] /* traverse "/content/docs/en/6-2//*" where isdescendantnode([a], [/content/docs/en/6-2]) */ union [cq:Page] as [a] /* traverse "/content/docs/en/6-3//*" where isdescendantnode([a], [/content/docs/en/6-3]) */
   ```

   QueryBuilder Debugger [および](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0Agroup.1_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-2%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Dcq%3APage%0D%0Agroup.2_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-3%0D%0A%23+add+all+asset+restrictions+to+this+group) AEM Chromeプラグインを使用して、クエリと結果を調べます [](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=ja-JP)。
