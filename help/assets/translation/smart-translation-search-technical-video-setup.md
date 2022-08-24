---
title: AEM Assetsでのスマート翻訳検索の設定
description: スマート翻訳検索を使用すると、英語以外の検索用語を使用して英語のコンテンツに解決できます。 スマート翻訳検索用にAEMを設定するには、Apache Oak Search Machine Translation OSGi バンドルと、翻訳ルールを含む関連するフリーソースでオープンソースの Apache Joshua 言語パックをインストールし、設定する必要があります。
version: 6.4, 6.5
feature: Search
topic: Content Management
role: Developer
level: Beginner
exl-id: 7be8c3d5-b944-4421-97b3-bd5766c1b1b5
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '867'
ht-degree: 1%

---

# AEM Assetsでのスマート翻訳検索の設定{#set-up-smart-translation-search-with-aem-assets}

スマート翻訳検索を使用すると、英語以外の検索用語を使用して英語のコンテンツに解決できます。 スマート翻訳検索用にAEMを設定するには、Apache Oak Search Machine Translation OSGi バンドルと、翻訳ルールを含む関連するフリーソースでオープンソースの Apache Joshua 言語パックをインストールし、設定する必要があります。

>[!VIDEO](https://video.tv.adobe.com/v/21291/?quality=9&learn=on)

>[!NOTE]
>
>スマート翻訳検索は、必要な各AEMインスタンスに対して設定する必要があります。

1. Oak Search Machine Translation OSGi バンドルをダウンロードしてインストールします。
   * [Oak Search Machine Translation OSGi バンドルのダウンロード](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) AEM Oak バージョンに対応する
   * ダウンロードした Oak Search Machine Translation OSGi バンドルををを介してAEMにインストールします。 [ `/system/console/bundles`](http://localhost:4502/system/console/bundles).

2. Apache Joshua 言語パックをダウンロードして更新します。
   * 目的のをダウンロードして展開します。 [Apache Joshua 言語パック](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs).
   * を編集します。 `joshua.config` ファイルを開き、次の行で始まる 2 行をコメントアウトします。

      ```
      feature-function = LanguageModel ...
      ```

   * 言語パックのモデルフォルダーのサイズを決定して記録します。これは、AEMで必要とされる余分なヒープスペースの量に影響します。
   * 解凍した Apache Joshua 言語パックフォルダー ( `joshua.config` 編集 )

      ```
      .../crx-quickstart/opt/<source_language-target_language>
      ```

      次に例を示します。

      ```
       .../crx-quickstart/opt/es-en
      ```

3. ヒープメモリ割り当てを更新してAEMを再起動
   * AEM を停止します。
   * AEMの新しく必要なヒープサイズの決定

      * AEM pre-lack heap size +モデルディレクトリのサイズを最も近い 2GB に丸めた値
      * 例：言語パック前の場合、AEMのインストールを実行するには 8 GB のヒープが必要で、言語パックのモデルフォルダーが 3.8 GB の非圧縮の場合、新しいヒープサイズは次のようになります。

         オリジナル `8GB` + ( `3.75GB` 最も近い `2GB`は、 `4GB`) の合計 `12GB`
   * この容量の余分なメモリが装置にあることを確認します。
   * AEM起動スクリプトを更新して、新しいヒープサイズに合わせて調整します

      * 例： `java -Xmx12g -jar cq-author-p4502.jar`
   * ヒープサイズを増やしてAEMを再起動します。

   >[!NOTE]
   >
   >言語パックに必要なヒープスペースは、特に複数の言語パックを使用する場合に大きくなる可能性があります。
   >
   >
   >常に確認する **インスタンスに十分なメモリがある** 割り当てられたヒープ領域の増加に対応するため。
   >
   >
   >この **言語パックを使用せずに、許容可能なパフォーマンスをサポートするために、常にベースヒープを計算する必要があります** インストール済み

4. Apache Jackrabbit Oak Machine Translation Full-text Query Terms Provider OSGi 設定を使用して言語パックを登録します。

   * 各言語パックに対して、 [新しい Apache Jackrabbit Oak Machine Translation Full-text Query Terms Provider OSGi 設定を作成します。](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory) AEM Web コンソールの Configuration Manager を使用する。

      * `Joshua Config Path` は joshua.config ファイルへの絶対パスです。 AEMプロセスは、言語パックのフォルダー内のすべてのファイルを読み取れる必要があります。
      * `Node types` は、フルテキスト検索でこの言語パックを翻訳に関与させる候補ノードタイプです。
      * `Minimum score` は、使用する翻訳された用語の最小信頼性スコアです。

         * 例えば、hombre（スペイン語で「man」）は、信頼性スコアが `0.9` また、信頼性スコアを持つ英語の単語「人間」に翻訳します。 `0.2`. 最小スコアの調整 `0.3`は、「hombre」から「man」への翻訳を保持しますが、「hombre」から「human」への翻訳を、 `0.2` が `0.3`.

5. アセットに対するフルテキスト検索の実行
   * dam:Asset は、この言語パックが再び登録されるノードタイプなので、これを検証するには、フルテキスト検索を使用してAEM Assetsを検索する必要があります。
   * AEM / Assets に移動し、オムニサーチを開きます。 言語パックがインストールされた言語の語句を検索します。
   * 必要に応じて、OSGi 設定の最小スコアを調整し、結果の正確性を確保します。

6. 言語パックを更新しています
   * Apache Joshua 言語パックは Apache Joshua プロジェクトによって完全に管理され、更新や修正は Apache Joshua プロジェクトの裁量によって行われます。
   * 言語パックを更新した場合、AEMで更新をインストールするには、上記の手順 2～4 に従い、必要に応じてヒープサイズを上下に調整する必要があります。

      * 解凍した言語パックを crx-quickstart/opt フォルダーに移動する場合は、新しいフォルダーにコピーする前に既存の言語パックフォルダーを移動します。
   * AEMを再起動する必要がない場合は、更新された言語パックに関連する関連する Apache Jackrabbit Oak Machine Translation Fulltext Query Terms Provider OSGi 設定を再保存し、AEMが更新されたファイルを処理できるようにする必要があります。


## damAssetLucene インデックスを更新中 {#updating-damassetlucene-index}

次のために [AEM Smart Tags](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html) AEM Smart Translation の影響を受ける、AEM `/oak   :index  /damAssetLucene` index を更新して、predictedTags（「スマートタグ」のシステム名）をアセットの集計 Lucene インデックスの一部にするようにマークする必要があります。

の下 `/oak:index/damAssetLucene/indexRules/dam:Asset/properties/predicatedTags`で、設定が次のようになっていることを確認します。

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

* [Apache Oak Search Machine Translation OSGi バンドル](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22)
* [Apache Joshua 言語パック](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
* [AEM Smart Tags](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
* [クエリとインデックスに関するベストプラクティス](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)
