---
title: AEM Assets でのスマート翻訳検索の設定
description: スマート翻訳検索を使用すると、英語以外の検索用語を使用して英語のコンテンツに対応することができます。スマート翻訳検索用に AEM を設定するには、Apache Oak Search Machine Translation OSGi バンドルと、翻訳ルールを含む、関連する無料のオープンソースである Apache Joshua 言語パックをインストールして設定します。
version: 6.4, 6.5
feature: Search
topic: Content Management
role: Developer
level: Beginner
doc-type: Technical Video
exl-id: 7be8c3d5-b944-4421-97b3-bd5766c1b1b5
duration: 653
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '811'
ht-degree: 100%

---

# AEM Assets でのスマート翻訳検索の設定{#set-up-smart-translation-search-with-aem-assets}

スマート翻訳検索を使用すると、英語以外の検索用語を使用して英語のコンテンツに対応することができます。スマート翻訳検索用に AEM を設定するには、Apache Oak Search Machine Translation OSGi バンドルと、翻訳ルールを含む、関連する無料のオープンソースである Apache Joshua 言語パックをインストールして設定します。

>[!VIDEO](https://video.tv.adobe.com/v/21291?quality=12&learn=on)

>[!NOTE]
>
>スマート翻訳検索は、必要な各 AEM インスタンスで設定します。

1. Oak Search Machine Translation OSGi バンドルをダウンロードしてインストールする
   * AEM の Oak バージョンに対応する [Oak Search Machine Translation OSGi バンドル](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22)をダウンロードします。
   * ダウンロードした Oak Search Machine Translation OSGi バンドルを [`/system/console/bundles` ](http://localhost:4502/system/console/bundles) 経由で AEM にインストールします。

2. Apache Joshua 言語パックをダウンロードして更新します
   * 目的の [Apache Joshua 言語パック](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)をダウンロードして解凍します。
   * `joshua.config` ファイルを編集し、以下で始まる 2 行をコメントアウトします。

     ```
     feature-function = LanguageModel ...
     ```

   * 言語パックのモデルフォルダーのサイズを決定して記録します。これは、AEM で必要とされる余分なヒープ領域の量に影響します。
   * 解凍した Apache Joshua 言語パックフォルダー（`joshua.config` の編集済み）を以下の場所に移動します。

     ```
     .../crx-quickstart/opt/<source_language-target_language>
     ```

     次に例を示します。

     ```
      .../crx-quickstart/opt/es-en
     ```

3. ヒープメモリ割り当てを更新して、AEM を再起動します
   * AEM を停止します。
   * AEM に必要な新しいヒープサイズを決定します

      * AEM の pre-language-lack のヒープサイズ + モデルディレクトリのサイズを最も近い 2 GB に切り上げた値
      * 例：プレ言語パックに関して、AEM のインストール実行に 8 GB のヒープが必要で、言語パックのモデルフォルダーが非圧縮で 3.8 GB の場合、新しいヒープサイズは次のようになります。

        オリジナル `8GB` +（`3.75GB` を最も近い `2GB` で切り上げた値 `4GB`）の合計値である `12GB`

   * マシンに、利用可能なメモリ空き容量がこの値の量あることを確認します。
   * AEM 起動スクリプトを更新して、新しいヒープサイズに合わせて調整します

      * 例：`java -Xmx12g -jar cq-author-p4502.jar`

   * ヒープサイズを増やして AEM を再起動します。

   >[!NOTE]
   >
   >特に複数の言語パックが使用されている場合、言語パックに必要なヒープ領域が大きくなる可能性があります。
   >
   >
   >割り当てられたヒープ領域の増加に対応するために、**インスタンスに十分なメモリがある**&#x200B;ことを常に確認します。
   >
   >
   >**言語パックがインストールされていなくても許容可能なパフォーマンスをサポートするために、常にベースヒープを計算する必要があります**。

4. Apache Jackrabbit Oak Machine Translation Full-text Query Terms Provider OSGi 設定を使用して言語パックを登録します。

   * 各言語パックに対して、AEM web コンソールの Configuration Manager を使用して[新しい Apache Jackrabbit Oak Machine Translation Full-text Query Terms Provider OSGi 設定を作成します](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory)。

      * `Joshua Config Path` は joshua.config ファイルへの絶対パスです。AEM プロセスは、言語パックのフォルダー内のすべてのファイルを読み取ることができる状態にしておく必要があります。
      * `Node types` は、フルテキスト検索でこの言語パックを翻訳に関与させる候補ノードタイプです。
      * `Minimum score` は、翻訳された用語が使用されるための信頼性に関する最小スコアです。

         * 例えば、hombre（スペイン語の「男」）は、信頼性スコア `0.9` で英単語「man」に翻訳されますが、信頼性スコア `0.2` で英単語「human」に翻訳される場合もあります。最小スコアを `0.3` に調整すると、翻訳スコア `0.2` が最小スコア `0.3` よりも小さいため、「hombre」から「man」への翻訳は保持されますが、「hombre」から「human」への翻訳は破棄されます。

5. アセットに対してフルテキスト検索を実行します
   * dam:Asset は、この言語パックが再登録されるノードタイプなので、これを検証するには、フルテキスト検索を使用して AEM Assets を検索する必要があります。
   * AEM／Assets に移動し、オムニサーチを開きます。言語パックがインストールされている言語で用語を検索します。
   * 必要に応じて、OSGi 設定の最小スコアを調整し、結果の正確性を確保します。

6. 言語パックを更新します
   * Apache Joshua 言語パックは、Apache Joshua プロジェクトで完全に管理されており、更新や修正は Apache Joshua プロジェクトの裁量によって行われます。
   * 言語パックを更新した場合、AEM で更新をインストールするには、上記の手順 2 ～ 4 に従い、必要に応じてヒープサイズを調整する必要があります。

      * 解凍した言語パックを crx-quickstart/opt フォルダーに移動する場合は、新しいフォルダーにコピーする前に既存の言語パックフォルダーを移動します。

   * AEM を再起動する必要がない場合は、更新された言語パックに関連する Apache Jackrabbit Oak Machine Translation Fulltext Query Terms Provider OSGi 設定を再保存して、AEM が更新されたファイルを処理できるようにします。

## damAssetLucene インデックスの更新 {#updating-damassetlucene-index}

[AEM スマートタグ](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html?lang=ja)が AEM スマート翻訳の影響を受けるようにするには、AEM の `/oak   :index  /damAssetLucene` インデックスを更新して、predictedTags（「スマートタグ」のシステム名）をアセットの集計 Lucene インデックスの一部としてマークする必要があります。

`/oak:index/damAssetLucene/indexRules/dam:Asset/properties/predicatedTags` の下で、設定が次のようになっていることを確認します。

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
* [AEM スマートタグ](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html?lang=ja)
* [クエリとインデックス作成のベストプラクティス](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html?lang=ja)
