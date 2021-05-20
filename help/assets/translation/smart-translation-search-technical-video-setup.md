---
title: AEM Assetsでのスマート翻訳検索の設定
description: スマート翻訳検索を使用すると、英語以外の検索用語を使用して英語のコンテンツに解決できます。 スマート翻訳検索用にAEMを設定するには、Apache Oak Search Machine Translation OSGiバンドルと、翻訳ルールを含む関連するフリーソースのオープンソースApache Joshua言語パックをインストールし、設定する必要があります。
version: 6.3, 6.4, 6.5
feature: 検索
topic: コンテンツ管理
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '870'
ht-degree: 1%

---


# AEM Assets{#set-up-smart-translation-search-with-aem-assets}でのスマート翻訳検索の設定

スマート翻訳検索を使用すると、英語以外の検索用語を使用して英語のコンテンツに解決できます。 スマート翻訳検索用にAEMを設定するには、Apache Oak Search Machine Translation OSGiバンドルと、翻訳ルールを含む関連するフリーソースのオープンソースApache Joshua言語パックをインストールし、設定する必要があります。

>[!VIDEO](https://video.tv.adobe.com/v/21291/?quality=9&learn=on)

>[!NOTE]
>
>スマート翻訳検索は、必要な各AEMインスタンスに対して設定する必要があります。

1. Oak Search Machine Translation OSGiバンドルをダウンロードしてインストールします。
   * [AEM Oakバージョンに対応するOak Search Machine Translation OSGiバ](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) ンドルをダウンロードします。
   * ダウンロードしたOak Search Machine Translation OSGiバンドルを[ `/system/console/bundles`](http://localhost:4502/system/console/bundles)を介してAEMにインストールします。

2. Apache Joshua言語パックをダウンロードして更新します。
   * 目的の[Apache Joshua言語パック](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)をダウンロードして解凍します。
   * `joshua.config`ファイルを編集し、次の行で始まる2行をコメントアウトします。

      ```
      feature-function = LanguageModel ...
      ```

   * 言語パックのモデルフォルダーのサイズを決定して記録します。これは、AEMで必要とされる追加のヒープスペースの量に影響します。
   * 解凍したApache Joshua言語パックフォルダー（`joshua.config`編集内容を含む）をに移動します。

      ```
      .../crx-quickstart/opt/<source_language-target_language>
      ```

      以下に例を示します。

      ```
       .../crx-quickstart/opt/es-en
      ```

3. ヒープメモリ割り当てを更新してAEMを再起動する
   * AEM を停止します。
   * AEMの新しく必要なヒープサイズの決定

      * AEM pre-language-lack heap size +モデルディレクトリのサイズを最も近い2 GBに丸めた値
      * 例：言語パック前の場合、AEMのインストールを実行するには8 GBのヒープが必要で、言語パックのモデルフォルダーが3.8 GB未圧縮の場合、新しいヒープサイズは次のようになります。

         元の`8GB` + ( `3.75GB`は最も近い`2GB`(`4GB`)に切り上げられ、合計は`12GB`になります。
   * この容量のメモリがマシンにあることを確認します。
   * AEM起動スクリプトを更新して、新しいヒープサイズに合わせて調整する

      * 例：`java -Xmx12g -jar cq-author-p4502.jar`
   * ヒープサイズを増やしてAEMを再起動します。

   >[!NOTE]
   >
   >特に複数の言語パックを使用する場合、言語パックに必要なヒープスペースが大きくなる可能性があります。
   >
   >
   >割り当てられたヒープ領域の増加に対応する十分なメモリ&#x200B;**をインスタンスに必ず**&#x200B;用意してください。
   >
   >
   >**ベースヒープは、言語パック**&#x200B;をインストールせずに許容可能なパフォーマンスをサポートするために、常に計算する必要があります。

4. Apache Jackrabbit Oak Machine Translation Full-text Query Terms Provider OSGi設定を使用して言語パックを登録します。

   * 各言語パックに対して、AEM WebコンソールのConfiguration Managerを使用して、新しいApache Jackrabbit Oak Machine Translation Full-text Query Terms Provider OSGi設定](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory)を作成します。[

      * `Joshua Config Path` はjoshua.configファイルへの絶対パスです。AEMプロセスは、言語パックのフォルダー内のすべてのファイルを読み取れる必要があります。
      * `Node types` は、フルテキスト検索でこの言語パックを翻訳に関与させる候補ノードタイプです。
      * `Minimum score` は、使用される翻訳済みの用語の最小信頼性スコアです。

         * 例えば、hombre（「man」のスペイン語）は、信頼性スコア`0.9`の英語の単語「man」に翻訳し、信頼性スコア`0.2`の英語の単語「human」に翻訳することができます。 最小スコアを`0.3`にチューニングすると、「ホンブル」から「男性」への翻訳は維持されますが、この「ホンブル」から「人間」への翻訳は破棄されます。これは、`0.2`のこの翻訳スコアが`0.3`の最小スコアよりも小さいためです。

5. アセットに対する全文検索の実行
   * dam:Assetは、この言語パックが再び登録されるノードタイプなので、フルテキスト検索を使用してAEM Assetsを検索し、これを検証する必要があります。
   * AEM /アセットに移動し、オムニサーチを開きます。 言語パックがインストールされた言語の用語を検索します。
   * 必要に応じて、OSGi設定の最小スコアを調整し、結果の正確性を確認します。

6. 言語パックの更新
   * Apache Joshua言語パックは、Apache Joshuaプロジェクトによって完全に管理され、その更新や修正はApache Joshuaプロジェクトの裁量となります。
   * 言語パックを更新した場合、AEMで更新をインストールするには、上記の手順2～4に従い、必要に応じてヒープサイズを上下に調整する必要があります。

      * 解凍した言語パックをcrx-quickstart/optフォルダーに移動する場合は、既存の言語パックフォルダーを移動してから、新しい言語パックフォルダーにコピーします。
   * AEMを再起動する必要がない場合は、AEMが更新されたファイルを処理するために、更新された言語パックに関連する関連するApache Jackrabbit Oak Machine Translation Fulltext Query Terms Provider OSGi設定を再保存する必要があります。


## damAssetLuceneインデックス{#updating-damassetlucene-index}を更新しています

[AEM Smart Tags](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)がAEM Smart Translationの影響を受けるには、AEM `/oak   :index  /damAssetLucene`インデックスを更新して、predictedTags（「Smart Tags」のシステム名）をAssetの集計Luceneインデックスの一部にする必要があります。

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
* [クエリとインデックスに関するベストプラクティス](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)