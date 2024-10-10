---
title: 簡易検索の実装ガイド
description: 簡易検索の実装は、2017 Summit lab AEM Search Demystified の資料です。このページには、このラボの資料が含まれています。 ラボのガイド付きツアーについては、このページの「プレゼンテーション」の節でラボワークブックを参照してください。
version: 6.4, 6.5
feature: Search
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Tutorial
exl-id: aa268c5f-d29e-4868-a58b-444379cb83be
last-substantial-update: 2022-08-10T00:00:00Z
thumbnail: 32090.jpg
duration: 138
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '627'
ht-degree: 100%

---

# 簡易検索の実装ガイド{#simple-search-implementation-guide}

簡易検索の実装は、**Adobe Summit lab AEM Search Demystified** の資料です。このページには、このラボの資料が含まれています。 ラボのガイド付きツアーについては、このページの「プレゼンテーション」の節でラボワークブックを参照してください。

![検索アーキテクチャの概要](assets/l4080/simple-search-application.png)

## プレゼンテーション資料 {#bookmarks}

* [ラボワークブック](assets/l4080/l4080-lab-workbook.pdf)
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

*以下の章のリンクは、[初期パッケージ](#initialpackages)が`http://localhost:4502`* で AEM オーサーにインストールされていることを前提としています

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

* [第 1 章ソリューション](assets/l4080/l4080-chapter1.zip)
* [第 2 章ソリューション](assets/l4080/l4080-chapter2.zip)
* [第 3 章ソリューション](assets/l4080/l4080-chapter3.zip)
* [第 4 章ソリューション](assets/l4080/l4080-chapter4.zip)
* [第 5 章設定](assets/l4080/l4080-chapter5-setup.zip)
* [第 5 章ソリューション](assets/l4080/l4080-chapter5-solution.zip)
* [第 6 章ソリューション](assets/l4080/l4080-chapter6.zip)
* [第 9 章ソリューション](assets/l4080/l4080-chapter9.zip)

## 参照資料 {#reference-materials}

* [GitHub リポジトリ](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/master/simple-search-guide)
* [Sling Model](https://sling.apache.org/documentation/bundles/models.html)
* [Sling Model Exporter](https://sling.apache.org/documentation/bundles/models.html#exporter-framework-since-130)
* [QueryBuilder API](https://experienceleague.adobe.com/docs/?lang=ja)
* [AEM Chrome プラグイン](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode)（[ドキュメントページ](https://adobe-consulting-services.github.io/acs-aem-tools/aem-chrome-plugin/)）

## 修正とフォローアップ {#corrections-and-follow-up}

ラボでのディスカッションからの修正と説明および出席者からのフォローアップの質問への回答。

1. **インデックスの再作成を停止するにはどうすればよいですか？**

   インデックスの再作成は、[AEM Web コンソール／JMX](http://localhost:4502/system/console/jmx) から使用可能な IndexStats MBean を介して停止できます。

   * [http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats](http://localhost:4502/system/console/jmx/org.apache.jackrabbit.oak%3Aname%3Dasync%2Ctype%3DIndexStats)
      * `abortAndPause()` を実行して、インデックスの再作成を中止します。これにより、`resume()` が呼び出されるまでインデックスがロックされて、さらにインデックスを再作成できなくなります。
      * `resume()` を実行すると、インデックス作成プロセスが再開されます。
   * ドキュメント：[https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean](https://jackrabbit.apache.org/oak/docs/query/indexing.html#async-index-mbean)

2. **Oak インデックスは複数のテナントをどのようにサポートできますか？**

   Oak はコンテンツツリー全体へのインデックス配置をサポートしており、これらのインデックスはそのサブツリー内でのみインデックス化できます。例えば、**`/content/site-a/oak:index/cqPageLucene`** を作成して、**`/content/site-a`の下にあるコンテンツのみをインデックス化できます。**

   同等の方法で、**`/oak:index`** の下にあるインデックスで **`includePaths`** および **`queryPaths`** プロパティを使用できます。次に例を示します。

   * `/oak:index/siteAcqPageLucene@includePaths=/content/site-a`
   * `/oak:index/siteAcqPageLucene@queryPaths=/content/site-a`

   この方法に関する考慮事項は次のとおりです。

   * クエリでは、インデックスのクエリパス範囲と等しいか、またはその下位であるパス制限を指定する必要があります。
   * 範囲の広いインデックス（`/oak:index/cqPageLucene` など）もデータのインデックスを作成するので、取り込みコストとディスク使用コストが重複します。
   * 重複した設定管理が必要になる場合があります（例：同じクエリセットを満たす必要がある場合は、複数のテナントインデックスに同じ indexRules を追加する）
   * この方法は、カスタムサイト検索の AEM パブリッシュ層で最適に機能します。AEM オーサーの場合と同様に、様々なテナントのコンテンツツリーの上位でクエリが実行されるのが一般的です（オムニサーチ経由など）。インデックスの定義が異なると、パス制限のみに基づく動作が異なる場合があります。

3. **使用可能なすべてのアナライザーのリストはどこにありますか？**

   Oak は、AEM で使用するための lucene-provides アナライザー設定要素のセットを公開します。

   * [Apache Oak アナライザードキュメント](https://jackrabbit.apache.org/oak/docs/query/lucene.html#analyzers)
      * [Tokenizers](https://cwiki.apache.org/confluence/display/solr/Tokenizers)
      * [フィルター](https://cwiki.apache.org/confluence/display/solr/Filter+Descriptions)
      * [CharFilters](https://cwiki.apache.org/confluence/display/solr/CharFilterFactories)

4. **同じクエリでページとアセットを検索するにはどうすればよいですか？**

   指定したクエリと同じクエリで複数のノードタイプをクエリする機能が AEM 6.3 で新しく導入されました。次の QueryBuilder クエリを例に考えてみます。各「サブクエリ」は独自のインデックスに解決できるので、この例では、`cq:Page` サブクエリは `/oak:index/cqPageLucene` に解決され、`dam:Asset` サブクエリは `/oak:index/damAssetLucene` に解決されます。

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   # add all page restrictions to this group
   group.2_group.type=dam:Asset
   # add all asset restrictions to this group
   ```

   結果として、次のクエリとクエリプランが得られます。

   ```plain
   QUERY:(//element(*, cq:Page) | //element(*, dam:Asset))
   
   PLAN: [cq:Page] as [a] /* lucene:cqPageLucene(/oak:index/cqPageLucene) *:* */ union [dam:Asset] as [a] /* lucene:damAssetLucene(/oak:index/damAssetLucene) *:* */
   ```

   [QueryBuilder デバッガー](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Ddam%3AAsset%0D%0A%23+add+all+asset+restrictions+to+this+group)と [AEM Chrome プラグイン](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=ja-JP)を使用して、クエリと結果を調べます。

5. **同じクエリで複数のパスを検索するにはどうすればよいですか？**

   指定したクエリと同じクエリで複数のパスにわたってクエリする機能が AEM 6.3 で新しく導入されました。次の QueryBuilder クエリを例に考えてみます。各「サブクエリ」が独自のインデックスに解決される可能性があります。

   ```plain
   group.p.or=true
   group.1_group.type=cq:Page
   group.1_group.path=/content/docs/en/6-2
   # add all page restrictions to this group
   group.2_group.type=cq:Page
   group.2_group.path=/content/docs/en/6-3
   # add all asset restrictions to this group
   ```

   結果として、次のクエリとクエリプランが得られます。

   ```plain
   QUERY: (/jcr:root/content/docs/en/_x0036_-2//element(*, cq:Page) | /jcr:root/content/docs/en/_x0036_-3//element(*, cq:Page))
   
   PLAN: [cq:Page] as [a] /* traverse "/content/docs/en/6-2//*" where isdescendantnode([a], [/content/docs/en/6-2]) */ union [cq:Page] as [a] /* traverse "/content/docs/en/6-3//*" where isdescendantnode([a], [/content/docs/en/6-3]) */
   ```

   [QueryBuilder デバッガー](http://localhost:4502/libs/cq/search/content/querydebug.html?_charset_=UTF-8&amp;query=group.p.or%3Dtrue%0D%0Agroup.1_group.type%3Dcq%3APage%0D%0Agroup.1_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-2%0D%0A%23+add+all+page+restrictions+to+this+group%0D%0Agroup.2_group.type%3Dcq%3APage%0D%0Agroup.2_group.path%3D%2Fcontent%2Fdocs%2Fen%2F6-3%0D%0A%23+add+all+asset+restrictions+to+this+group)と [AEM Chrome プラグイン](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=ja-JP)を使用して、クエリと結果を調べます。
