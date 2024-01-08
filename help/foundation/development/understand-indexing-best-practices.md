---
title: AEMでのインデックス作成のベストプラクティス
description: AEMでのインデックス作成に関するベストプラクティスについて説明します。
version: 6.4, 6.5, Cloud Service
sub-product: Experience Manager, Experience Manager Sites
feature: Search
doc-type: Article
topic: Development
role: Developer, Architect
level: Beginner
duration: 0
last-substantial-update: 2024-01-04T00:00:00Z
jira: KT-14745
thumbnail: KT-14745.jpeg
source-git-commit: 5fe651bc0dc73397ae9602a28d63b7dc084fcc70
workflow-type: tm+mt
source-wordcount: '1331'
ht-degree: 1%

---


# AEMでのインデックス作成のベストプラクティス

Adobe Experience Manager(AEM) のインデックス作成に関するベストプラクティスについて説明します。 Apache [Jackrabbit Oak](https://jackrabbit.apache.org/oak/docs/query/query.html) AEMでのコンテンツ検索を強化し、次の重要なポイントを示します。

- 標準では、AEMは、検索やクエリ機能をサポートする様々なインデックスを提供します。例えば、 `damAssetLucene`, `cqPageLucene` その他。
- すべてのインデックス定義は、以下の場所にリポジトリに保存されます。 `/oak:index` ノード。
- AEM as a Cloud Serviceは Oak Lucene インデックスのみをサポートします。
- インデックスの設定は、AEMプロジェクトコードベースで管理し、Cloud Manager CI/CD パイプラインを使用してデプロイする必要があります。
- 特定のクエリに対して複数のインデックスが使用可能な場合、 **推定コストが最も低い指標を使用**.
- 特定のクエリに使用できるインデックスがない場合、一致するコンテンツを見つけるためにコンテンツツリーが走査されます。 ただし、 `org.apache.jackrabbit.oak.query.QueryEngineSettingsService` は、10,000 個のノードをトラバースするだけです。
- クエリの結果は次のとおりです **最後にフィルター済み** 現在のユーザーが読み取りアクセス権を持つようにします。 つまり、クエリ結果は、インデックスで指定されたノードの数よりも少なくなる場合があります。
- インデックス定義を変更した後にリポジトリのインデックスを再作成するには時間が必要で、リポジトリのサイズに応じて異なります。

AEMインスタンスのパフォーマンスに影響を与えない、効率的で正しい検索機能を使用するには、インデックス作成のベストプラクティスを理解することが重要です。

## カスタムと標準のインデックス

場合によっては、検索要件に対応するためにカスタムインデックスを作成する必要があります。 ただし、カスタムインデックスを作成する前に、次のガイドラインに従ってください。

- 検索要件を理解し、OOTB インデックスが検索要件をサポートできるかどうかを確認します。 用途 **クエリパフォーマンスツール**（利用可能： ） [ローカル SDK](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) と AEMCS を使用する場合は、デベロッパーコンソールまたは `https://author-pXXXX-eYYYY.adobeaemcloud.com/ui#/aem/libs/granite/operations/content/diagnosistools/queryPerformance.html?appId=aemshell`.

- 最適なクエリを定義するには、 [クエリの最適化](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/query-and-indexing-best-practices.html?#optimizing-queries) フローチャートと [JCR Query Cheat Sheet](https://experienceleague.adobe.com/docs/experience-manager-65/assets/JCR_query_cheatsheet-v1.1.pdf?lang=en) 参照用。

- OOTB インデックスが検索要件をサポートしない場合は、2 つのオプションがあります。 ただし、 [効率的なインデックス作成のヒント](https://experienceleague.adobe.com/docs/experience-manager-65/content/implementing/deploying/practices/best-practices-for-queries-and-indexing.html?#should-i-create-an-index)
   - OOTB インデックスのカスタマイズ：保守とアップグレードが簡単に行えるので、お勧めのオプション。
   - 完全なカスタムインデックス：上記のオプションが機能しない場合のみ。

### OOTB インデックスのカスタマイズ

- OOTB インデックスをカスタマイズする場合は、を使用します。 **\&lt;ootbindexname>-\&lt;productversion>-custom-\&lt;customversion>** 命名規則を使用します。 例： `cqPageLucene-custom-1` または `damAssetLucene-8-custom-1`. これは、OOTB インデックスが更新されるたびに、カスタマイズされたインデックス定義を結合するのに役立ちます。 詳しくは、 [標準提供のインデックスの変更](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html?#changes-to-out-of-the-box-indexes) を参照してください。

- 常に CRX DE Package Manager(/crx/packmgr/) を使用してAEMインスタンスから最新の OOTB インデックス定義をコピーし、名前を変更して XML ファイル内にカスタマイズを追加します。

- インデックス定義をAEMプロジェクト ( ) に格納します。 `ui.apps/src/main/content/jcr_root/_oak_index` Cloud Manager CI/CD パイプラインを使用してデプロイします。 詳しくは、 [カスタム・インデックス定義のデプロイ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html?#deploying-custom-index-definitions) を参照してください。

### 完全なカスタムインデックス

- 完全なカスタムインデックスを作成する場合は、 **\&lt;prefix>.\&lt;customindexname>-\&lt;version>-custom-\&lt;customversion>** 命名規則を使用します。 例えば、`wknd.adventures-1-custom-1` のようになります。これにより、名前の競合を回避できます。 ここで `wknd` はプレフィックスで、 `adventures` は、カスタムインデックス名です。

- AEMCS は Lucene インデックスのみをサポートしているので、AEMCS への今後の移行に備えて、常に Lucene インデックスを使用します。 詳しくは、 [Lucene インデックスとプロパティインデックス](https://experienceleague.adobe.com/docs/experience-manager-65/content/implementing/deploying/practices/best-practices-for-queries-and-indexing.html?#lucene-or-property-indexes) を参照してください。

- カスタムインデックスを `dam:Asset` ノードタイプを指定するが、OOTB をカスタマイズする `damAssetLucene` インデックス。 これは、パフォーマンスと機能の問題の一般的な根本原因です。

- また、例えば複数のノードタイプを追加しないでください。 `cq:Page` および `cq:Tag` インデックス作成ルール (`indexRules`) ノードで使用できます。 代わりに、ノードタイプごとに別々のインデックスを作成します。

- 上記の節で説明したように、インデックス定義をAEMプロジェクト ( ) に格納します。 `ui.apps/src/main/content/jcr_root/_oak_index` Cloud Manager CI/CD パイプラインを使用してデプロイします。 詳しくは、 [カスタム・インデックス定義のデプロイ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/indexing.html?#deploying-custom-index-definitions) を参照してください。

- インデックス定義のガイドラインを次に示します。
   - ノードタイプ (`jcr:primaryType`) は、 `oak:QueryIndexDefinition`
   - インデックスのタイプ (`type`) は、 `lucene`
   - async プロパティ (`async`) は、 `async, rt`
   - 用途 `includedPaths` を回避します。 `excludedPaths` プロパティ。 常に設定 `queryPaths` の値を `includedPaths` の値です。
   - パスの制限を適用するには、 `evaluatePathRestrictions` プロパティを `true`.
   - 用途 `tags` プロパティを使用してインデックスにタグを付け、クエリ時にこのタグ値を指定してインデックスを使用します。 一般的なクエリ構文は次のとおりです。 `<query> option(index tag <tagName>)`.

  ```xml
  /oak:index/wknd.adventures-1-custom-1
      - jcr:primaryType = "oak:QueryIndexDefinition"
      - type = "lucene"
      - compatVersion = 2
      - async = ["async", "rt"]
      - includedPaths = ["/content/wknd"]
      - queryPaths = ["/content/wknd"]
      - evaluatePathRestrictions = true
      - tags = ["customAdvSearch"]
  ...
  ```

### 例

いくつかの例を見て、ベストプラクティスを理解しましょう。

#### タグプロパティの不適切な使用方法

以下の図は、カスタムおよび OOTB のインデックス定義を示し、 `tags` プロパティ、両方のインデックスが同じを使用 `visualSimilaritySearch` の値です。

![タグプロパティの不適切な使用方法](./assets/understand-indexing-best-practices/incorrect-tags-property.png)

##### 分析

これは不適切な用法だ `tags` プロパティを指定します。 Oak クエリエンジンは、推定コストが最も低い原因となる OOTB インデックスを超えるカスタムインデックスを選択します。

正しい方法は、OOTB インデックスをカスタマイズし、 `indexRules` ノード。 詳しくは、 [OOTB インデックスのカスタマイズ](#customize-the-ootb-index) を参照してください。

#### のインデックス `dam:Asset` ノードタイプ

以下の画像は、 `dam:Asset` ノードタイプと `includedPaths` プロパティを特定のパスに設定します。

![dam:Asset ノードタイプのインデックス](./assets/understand-indexing-best-practices/index-for-damAsset-type.png)

##### 分析

Assets に対してオムニサーチを実行すると、誤った結果が返されるので、カスタムインデックスの推定コストが下がります。

カスタムインデックスを `dam:Asset` ノードタイプを指定するが、OOTB をカスタマイズする `damAssetLucene` インデックスと追加のプロパティ `indexRules` ノード。

#### インデックス作成ルールの下の複数のノードタイプ

以下の画像は、 `indexRules` ノード。

![インデックス作成ルールの下の複数のノードタイプ](./assets/understand-indexing-best-practices/multiple-nodetypes-in-index.png)

##### 分析

1 つのインデックスに複数のノードタイプを追加することは推奨されませんが、ノードタイプが密接に関連している場合は、同じインデックスに複数のノードタイプをインデックス化することは問題ありません。例： `cq:Page` および `cq:PageContent`.

有効な解決策は、OOTB をカスタマイズすることです `cqPageLucene` および `damAssetLucene` インデックス、既存の下にプロパティを追加する `indexRules` ノード。

#### 次の値がない `queryPaths` プロパティ

以下の画像は、 `queryPaths` プロパティ。

![queryPaths プロパティの不同](./assets/understand-indexing-best-practices/absense-of-queryPaths-prop.png)

##### 分析

常に設定 `queryPaths` の値を `includedPaths` の値です。 また、パスの制限を適用するには、 `evaluatePathRestrictions` プロパティを `true`.

#### index タグを使用したクエリ

下の画像は、 `tags` プロパティと、クエリ時の使用方法に関する情報です。

![index タグを使用したクエリ](./assets/understand-indexing-best-practices/tags-prop-usage.png)

```
/jcr:root/content/dam//element(*,dam:Asset)[(jcr:content/@contentFragment = 'true' and jcr:contains(., '/content/sitebuilder/test/mysite/live/ja-jp/mypage'))]order by @jcr:created descending option (index tag assetPrefixNodeNameSearch)
```

##### 分析

競合しない設定と修正の方法を示します。 `tags` プロパティの値を指定し、クエリ時に使用します。 一般的なクエリ構文は次のとおりです。 `<query> option(index tag <tagName>)`. 関連トピック [クエリオプションインデックスタグ](https://jackrabbit.apache.org/oak/docs/query/query-engine.html#query-option-index-tag)

#### カスタムインデックス

下の画像は、 `suggestion` ノードを使用して高度な検索機能を実現できます。

![カスタムインデックス](./assets/understand-indexing-best-practices/custom-index-with-suggestion-node.png)

##### 分析

これは、 [詳細検索](https://jackrabbit.apache.org/oak/docs/query/lucene.html#advanced-search-features) 機能。 ただし、インデックス名は **\&lt;prefix>.\&lt;customindexname>-\&lt;version>-custom-\&lt;customversion>** 命名規則を使用します。


## 役立つツール

インデックスの定義、分析、最適化に役立つツールをいくつかご覧ください。

### インデックス作成ツール

The [Oak インデックス定義ジェネレーター](https://oakutils.appspot.com/generate/index) ツールのヘルプ **インデックス定義を生成するには** 入力クエリに基づいて。 カスタムインデックスを作成するのは、まず最初に使用するとよいでしょう。

### インデックス分析ツール

The [インデックス定義アナライザ](https://oakutils.appspot.com/analyze/index) ツールのヘルプ **インデックス定義を分析するには** およびは、インデックス定義を改善するための推奨事項を提供します。

### クエリパフォーマンスツール

ザオットブ _クエリパフォーマンスツール_ 次の場所で利用可能： [ローカル SDK](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) と AEMCS を使用する場合は、デベロッパーコンソールまたは `https://author-pXXXX-eYYYY.adobeaemcloud.com/ui#/aem/libs/granite/operations/content/diagnosistools/queryPerformance.html?appId=aemshell` ヘルプ **クエリのパフォーマンスを分析するには** および [JCR Query Cheat Sheet](https://experienceleague.adobe.com/docs/experience-manager-65/assets/JCR_query_cheatsheet-v1.1.pdf?lang=en) を使用して、最適なクエリを定義します。

### ツールとヒントのトラブルシューティング

以下に示すほとんどは、AEM 6.X およびローカルのトラブルシューティングの目的で適用されます。

- インデックスマネージャは次の場所で使用可能： `http://host:port/libs/granite/operations/content/diagnosistools/indexManager.html` インデックス情報を取得するために使用します。

- Oak クエリの詳細なログとインデックス作成関連の Java™パッケージ ( `org.apache.jackrabbit.oak.plugins.index`, `org.apache.jackrabbit.oak.query`、および `com.day.cq.search` 経由 `http://host:port/system/console/slinglog` 」を参照してください。

- JMX MBean of _IndexStats_ 利用可能なタイプ `http://host:port/system/console/jmx` 非同期インデックス作成に関連するステータス、進行状況、統計などのインデックス情報を取得する場合。 また、 _FailingIndexStats_&#x200B;ここに結果がない場合は、インデックスが破損していないことを意味します。 AsyncIndexerService は、30 分間（設定可能）更新に失敗したインデックスを破損したものとしてマークし、インデックス作成を停止します。 クエリが期待した結果を返さない場合は、開発者がインデックス再作成を続行する前に、この点を確認すると役に立ちます。インデックス再作成は計算上のコストと時間がかかるためです。

- JMX MBean of _LuceneIndex_ 利用可能なタイプ `http://host:port/system/console/jmx` Lucene Index の統計（サイズ、インデックス定義あたりのドキュメント数など）の場合。

- JMX MBean of _QueryStat_ 利用可能なタイプ `http://host:port/system/console/jmx` Oak クエリの統計（クエリ、実行時間などの詳細を含む、低速で一般的なクエリを含む）。

## その他のリソース

詳しくは、次のドキュメントを参照してください。

- [Oak クエリとインデックス作成](https://experienceleague.adobe.com/docs/experience-manager-65/content/implementing/deploying/deploying/queries-and-indexing.html)
- [クエリとインデックス作成のベストプラクティス](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/operations/query-and-indexing-best-practices.html)
- [クエリとインデックス作成のベストプラクティス](https://experienceleague.adobe.com/docs/experience-manager-65/content/implementing/deploying/practices/best-practices-for-queries-and-indexing.html)
