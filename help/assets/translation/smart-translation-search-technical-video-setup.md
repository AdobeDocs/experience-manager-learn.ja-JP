---
title: AEM Assetsでのスマート翻訳検索の設定
description: スマート翻訳検索では、英語以外の検索用語を使用して英語のコンテンツに解決できます。 スマート翻訳検索用のAEMを設定するには、Apache Oak Search Machine Translation OSGiバンドルと、該当するフリーおよびオープンソースのApache Joshua言語パック（翻訳ルールを含む）をインストールし、設定する必要があります。
version: 6.3, 6.4, 6.5
feature: Search
topic: Content Management
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '870'
ht-degree: 1%

---


# AEM Assets{#set-up-smart-translation-search-with-aem-assets}を使用したスマート翻訳検索の設定

スマート翻訳検索では、英語以外の検索用語を使用して英語のコンテンツに解決できます。 スマート翻訳検索用のAEMを設定するには、Apache Oak Search Machine Translation OSGiバンドルと、該当するフリーおよびオープンソースのApache Joshua言語パック（翻訳ルールを含む）をインストールし、設定する必要があります。

>[!VIDEO](https://video.tv.adobe.com/v/21291/?quality=9&learn=on)

>[!NOTE]
>
>スマート翻訳検索は、必要な各AEMインスタンスで設定する必要があります。

1. Oak Search Machine Translation OSGiバンドルのダウンロードとインストール
   * [AEM Oakバージョンに対応するOak Search Machine Translation OSGi](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) バンドルをダウンロードします。
   * ダウンロードしたOak Search Machine Translation OSGiバンドルを[ `/system/console/bundles`](http://localhost:4502/system/console/bundles)経由でAEMにインストールします。

2. Apache Joshua言語パックのダウンロードと更新
   * 目的の[Apache Joshua言語パック](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)をダウンロードして解凍します。
   * `joshua.config`ファイルを編集し、次で始まる2行をコメントアウトします。

      ```
      feature-function = LanguageModel ...
      ```

   * 言語パックのモデルフォルダーのサイズを決定し、記録します。これは、AEMに必要な追加のヒープ領域の量に影響します。
   * 解凍されたApache Joshua言語パックフォルダー（`joshua.config`編集を含む）を

      ```
      .../crx-quickstart/opt/<source_language-target_language>
      ```

      次に例を示します。

      ```
       .../crx-quickstart/opt/es-en
      ```

3. ヒープメモリの割り当てが更新されたAEMを再起動
   * AEM を停止します。
   * AEMに必要な新しいヒープサイズの確認

      * AEM以前の言語不足のヒープサイズと、モデルディレクトリのサイズを、最も近い2 GBに切り上げ
      * 次に例を示します。言語パックを使用している場合、AEMのインストールを実行するのに8 GBのヒープが必要で、言語パックのモデルフォルダーが3.8 GBの非圧縮の場合、新しいヒープサイズは次のようになります。

         元の`8GB` + （ `3.75GB`は最も近い`2GB`に切り上げられ、`4GB`になります）。合計は`12GB`です
   * この空きメモリ量がマシンにあることを確認します。
   * AEM開始アップスクリプトを更新して、新しいヒープサイズに合わせて調整します

      * Ex.`java -Xmx12g -jar cq-author-p4502.jar`
   * ヒープサイズを増やしてAEMを再起動します。

   >[!NOTE]
   >
   >言語パックに必要なヒープスペースが大きくなる場合があります。特に、複数の言語パックが使用されている場合に大きくなります。
   >
   >
   >割り当てられたヒープ領域の増加に対応するために、**インスタンスに十分なメモリ**&#x200B;があることを必ず確認してください。
   >
   >
   >**ベースヒープは、言語パック**&#x200B;がインストールされていなくても、許容可能なパフォーマンスをサポートするために必ず計算する必要があります。

4. Apache Jackrabbit Oak Machine Translation Full-textクエリ用語プロバイダーOSGi設定を使用して言語パックを登録します。

   * 各言語パックに対して、AEM Web ConsoleのConfiguration Managerを使用して、[新しいApache Jackrabbit Oak Machine Translation Full-textクエリ用語プロバイダーOSGi設定](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory)を作成します。

      * `Joshua Config Path` は、joshua.configファイルの絶対パスです。AEMプロセスは、言語パックのフォルダー内のすべてのファイルを読み取れる必要があります。
      * `Node types` は、全文検索を行うことで、この言語パックを翻訳の対象とする候補ノードタイプです。
      * `Minimum score` は、使用される翻訳済みの用語の信頼性スコアの最小値です。

         * 例えば、hombre（「man」のスペイン語）は、信頼性スコア`0.9`の英語の単語「man」に翻訳し、信頼性スコア`0.2`の英語の単語「human」に翻訳できます。 最小スコアを`0.3`に調整すると、「ホンブレ」は「man」翻訳に保たれますが、この「a1/>」の翻訳スコアが`0.3`の最小スコアより小さいので、「ホンブレ」を「人間」翻訳に破棄します。`0.2`

5. アセットに対するフルテキスト検索の実行
   * dam:Assetは、この言語パックが再び登録されるノードタイプなので、この検証を行うには、フルテキスト検索を使用してAEM Assetsを検索する必要があります。
   * AEM/アセットに移動し、Omnisearchを開きます。 言語パックがインストールされた言語で用語を検索します。
   * 必要に応じて、OSGi設定の最小スコアを調整し、結果の正確性を確認します。

6. 言語パックの更新
   * Apache Joshua言語パックはApache Joshuaプロジェクトによって完全に管理され、その更新や修正はApache Joshuaプロジェクトの裁量によって行われます。
   * 言語パックが更新された場合、AEMでアップデートをインストールするには、上記の手順2 ～ 4に従い、必要に応じてヒープサイズを上下に調整する必要があります。

      * 解凍した言語パックをcrx-quickstart/optフォルダーに移動する場合は、既存の言語パックフォルダーを移動してから、新しいフォルダーにコピーすることに注意してください。
   * AEMを再起動する必要がない場合は、更新されたクエリパックに関連する、関連するApache Jackrabbit Oak Translation Fulltext言語用語プロバイダOSGi設定を再保存し、AEMが更新されたファイルを処理する必要があります。


## damAssetLuceneインデックス{#updating-damassetlucene-index}を更新しています

[AEMスマートタグ](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)がAEMスマート翻訳の影響を受けるためには、AEM `/oak   :index  /damAssetLucene`インデックスを更新して、predictedTags（「スマートタグ」のシステム名）をアセットの集計Luceneインデックスの一部とする必要があります。

`/oak:index/damAssetLucene/indexRules/dam:Asset/properties/predicatedTags`の下で、設定が次のようになっていることを確認します。

```xml
 <damAssetLucene jcr:primaryType="oak:QueryIndexDefinition">
        <indexRules jcr:primaryType="nt:unstructured">
            <dam:Asset jcr:primaryType="nt:unstructured">
                <properties jcr:primaryType="nt:unstructured">
                    ...
                    <predictedTags
                        jcr:primaryType="nt:unstructured"
                        isRegexp="{Boolean}true"
                        name="jcr:content/metadata/predictedTags/*/name"
                        useInSpellheck="{Boolean}true"
                        useInSuggest="{Boolean}true"
                        analyzed="{Boolean}true"
                        nodeScopeIndex="{Boolean}true"/>
```

## その他のリソース{#additional-resources}

* [Apache Oak Search Machine Translation OSGiバンドル](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22)
* [Apache Joshua言語パック](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
* [AEMスマートタグ](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
* [クエリとインデックス作成のベストプラクティス](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)