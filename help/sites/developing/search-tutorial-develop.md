---
title: シンプルな検索実装ガイド
description: 簡易検索の実装は、2017 Summit lab AEM Search Demystified の資料です。このページには、この実習の資料が含まれています。 ラボのガイド付きツアーについては、このページの「プレゼンテーション」セクションで Lab ワークブックを参照してください。
version: 6.4, 6.5
feature: Search
topic: Development
role: Developer
level: Intermediate, Experienced
exl-id: aa268c5f-d29e-4868-a58b-444379cb83be
last-substantial-update: 2022-08-10T00:00:00Z
thumbnail: 32090.jpg
source-git-commit: 1ecd3c761ea7c79036b263ff8528a6cd01af0e76
workflow-type: tm+mt
source-wordcount: '897'
ht-degree: 12%

---

# シンプルな検索実装ガイド{#simple-search-implementation-guide}

簡易検索の実装は、 **Adobe SummitラボAEM Search Demystified**. このページには、この実習の資料が含まれています。 ラボのガイド付きツアーについては、このページの「プレゼンテーション」セクションで Lab ワークブックを参照してください。

![検索アーキテクチャの概要](assets/l4080/simple-search-application.png)

## プレゼンテーション資料 {#bookmarks}

* [Lab ワークブック](assets/l4080/l4080-lab-workbook.pdf)
* [プレゼンテーション](assets/l4080/l4080-presentation.pdf)

## ブックマーク {#bookmarks-1}

### ツール {#tools}

* [インデックスマネージャー](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_oakindexmanager)
* [クエリの説明を実行](http://localhost:4502/libs/granite/operations/content/diagnosis/tool.html/granite_queryperformance)
* [CRXDE Lite](http://localhost:4502/crx/de/index.jsp#/oak%3Aindex/cqPageLucene) > /oak:index/cqPageLucene
* [CRX パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)
* [QueryBuilder デバッガー](http://localhost:4502/libs/cq/search/content/querydebug.html?)
* [Oak Index Definition Generator](https://oakutils.appspot.com/generate/index)

### 章 {#chapters}

*以下のチャプターリンクは、 [初期パッケージ](#initialpackages) は、AEM オーサーインストール先の`http://localhost:4502`*

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
* [簡易検索アプリケーションパッケージ](assets/l4080/simple.ui.apps-0.0.1-snapshot.zip)

### チャプターパッケージ {#chapter-packages}

* [第 1 章解決策](assets/l4080/l4080-chapter1.zip)
* [第 2 章解決策](assets/l4080/l4080-chapter2.zip)
* [第 3 章解決策](assets/l4080/l4080-chapter3.zip)
* [第 4 章解決策](assets/l4080/l4080-chapter4.zip)
* [第 5 章の設定](assets/l4080/l4080-chapter5-setup.zip)
* [第 5 章の解決策](assets/l4080/l4080-chapter5-solution.zip)
* [第 6 章解決策](assets/l4080/l4080-chapter6.zip)
* [第 9 章の解決策](assets/l4080/l4080-chapter9.zip)

## 参照されるマテリアル {#reference-materials}

* [GitHub リポジトリ](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/master/simple-search-guide)
* [Sling Model](https://sling.apache.org/documentation/bundles/models.html)
* [Sling Model Exporter](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
* [QueryBuilder API](https://experienceleague.adobe.com/docs/?lang=ja)
* [AEM Chrome プラグイン](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode) ([ドキュメントページ](https://adobe-consulting-services.github.io/acs-aem-tools/aem-chrome-plugin/))

## 修正とフォローアップ {#corrections-and-follow-up}

研究室のディスカッションの修正と説明、参加者からのフォローアップ質問への回答。

1. **インデックスの再作成を停止する方法は？**

   インデックスの再作成は、で使用できる IndexStats MBean を通じて停止できます。 [AEM Web コンソール/JMX](http://localhost:4502/system/console/jmx)

   * [http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats](http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats)
      * 実行 `abortAndPause()` ：インデックスの再作成を中止します。 これにより、インデックスがロックされ、インデックスがさらに再作成されるまで `resume()` が呼び出されます。
      * 実行中 `resume()` インデックス作成プロセスを再開します。
   * ドキュメント： [https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean](https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean)

2. **Oak インデックスは複数のテナントをどのようにサポートしますか？**

   Oak は、コンテンツツリーからインデックスを配置することをサポートし、これらのインデックスはそのサブツリー内でのみインデックスを作成します。 例： **`/content/site-a/oak:index/cqPageLucene`** 次の場所にのみコンテンツをインデックス化するように作成できます。 **`/content/site-a`.**

   同等の方法は、 **`includePaths`** および **`queryPaths`** 下のインデックスのプロパティ **`/oak:index`**. 次に例を示します。

   * `/oak:index/siteAcqPageLucene@includePaths=/content/site-a`
   * `/oak:index/siteAcqPageLucene@queryPaths=/content/site-a`

   このアプローチの考慮事項は次のとおりです。

   * クエリは、インデックスのクエリパススコープと等しいパス制限を指定するか、その下位のパス制限を指定する必要があります。
   * 範囲を広げたインデックス ( 例： `/oak:index/cqPageLucene`) もデータのインデックスを作成するので、取り込みコストとディスク使用コストが重複します。
   * 重複した設定管理が必要な場合があります ( 例： 同じクエリセットを満たす必要がある場合は、複数のテナントインデックスに同じ indexRules を追加します )。
   * この方法は、カスタムサイト検索の AEM パブリッシュ層に最適です。AEM オーサーと同様に、クエリは様々なテナントのコンテンツツリーの上位で実行されるのが一般的です（OmniSearch など）。インデックス定義が異なると、パス制限に基づいてのみ異なる動作が生じます。


3. **利用可能なすべてのアナライザーのリストはどこにありますか？**

   Oak は、AEMで使用する Lucene 提供のアナライザ設定要素のセットを公開します。

   * [Apache Oak Analyzers ドキュメント](https://jackrabbit.apache.org/oak/docs/query/lucene.html#analyzers)
      * [Tokenizers](https://cwiki.apache.org/confluence/display/solr/Tokenizers)
      * [フィルター](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions)
      * [CharFilters](https://cwiki.apache.org/confluence/display/solr/CharFilterFactories)

4. **同じクエリでページとアセットを検索する方法は？**

   AEM 6.3 の新機能は、同じ指定されたクエリで複数のノードタイプに対してクエリを実行できる機能です。 次の QueryBuilder クエリ。 各「サブクエリ」は、独自のインデックスに解決できるので、この例では `cq:Page` サブクエリの解決先 `/oak:index/cqPageLucene` そして `dam:Asset` サブクエリの解決先 `/oak:index/damAssetLucene`.

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   # add all page restrictions to this group
   group.2_group.type=dam:Asset
   # add all asset restrictions to this group
   ```

   結果は、次のクエリおよびクエリプランになります。

   ```plain
   QUERY:(//element(*, cq:Page) | //element(*, dam:Asset))
   
   PLAN: [cq:Page] as [a] /* lucene:cqPageLucene(/oak:index/cqPageLucene) *:* */ union [dam:Asset] as [a] /* lucene:damAssetLucene(/oak:index/damAssetLucene) *:* */
   ```

   を使用してクエリと結果を調べる [QueryBuilder デバッガー](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Ddam%3AAsset%0D%0A%23+add+all+asset+restrictions+to+this+group) および [AEM Chrome プラグイン](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=ja-JP).

5. **同じクエリで複数のパスを検索する方法は？**

   AEM 6.3 の新機能は、同じ指定されたクエリで複数のパスに対してクエリを実行する機能です。 次の QueryBuilder クエリ。 各「サブクエリ」は、独自のインデックスに解決される場合があることに注意してください。

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   group.1_group.path=/content/docs/en/6-2
   # add all page restrictions to this group
   group.2_group.type=cq:Page
   group.2_group.path=/content/docs/en/6-3
   # add all asset restrictions to this group
   ```

   結果は、次のクエリおよびクエリプランになります。

   ```plain
   QUERY: (/jcr:root/content/docs/en/_x0036_-2//element(*, cq:Page) | /jcr:root/content/docs/en/_x0036_-3//element(*, cq:Page))
   
   PLAN: [cq:Page] as [a] /* traverse "/content/docs/en/6-2//*" where isdescendantnode([a], [/content/docs/en/6-2]) */ union [cq:Page] as [a] /* traverse "/content/docs/en/6-3//*" where isdescendantnode([a], [/content/docs/en/6-3]) */
   ```

   を使用してクエリと結果を調べる [QueryBuilder デバッガー](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0Agroup.1_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-2%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Dcq%3APage%0D%0Agroup.2_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-3%0D%0A%23+add+all+asset+restrictions+to+this+group) および [AEM Chrome プラグイン](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=en-US).
