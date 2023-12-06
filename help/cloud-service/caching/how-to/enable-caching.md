---
title: CDN キャッシュを有効にする方法
description: AEMas a Cloud Serviceの CDN で HTTP 応答のキャッシュを有効にする方法を説明します。
version: Cloud Service
feature: Operations, CDN Cache
topic: Administration, Performance
role: Admin, Architect, Developer
level: Beginner
doc-type: Tutorial
last-substantial-update: 2023-11-17T00:00:00Z
jira: KT-14224
thumbnail: KT-14224.jpeg
source-git-commit: 43c021b051806380b3211f2d7357555622217b91
workflow-type: tm+mt
source-wordcount: '897'
ht-degree: 4%

---


# CDN キャッシュを有効にする方法

AEMas a Cloud Serviceの CDN で HTTP 応答のキャッシュを有効にする方法を説明します。 応答のキャッシュは、 `Cache-Control`, `Surrogate-Control`または `Expires` HTTP 応答キャッシュヘッダー。

これらのキャッシュヘッダーは、通常、`mod_headers` を使用して、AEM Dispatcher のホスト設定で行われますが、AEM パブリッシュ自体で実行されるカスタム Java™ コードで設定することもできます。

## デフォルトのキャッシュ動作

カスタム設定が存在しない場合は、デフォルト値が使用されます。 次のスクリーンショットに、AEM Publish とオーサーのデフォルトのキャッシュ動作を示します。 [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) ベース `mynewsite` AEMプロジェクトがデプロイされます。

![デフォルトのキャッシュ動作](../assets/how-to/aem-publish-default-cache-headers.png){width="800" zoomable="yes"}

以下を確認します。 [AEM Publish — デフォルトのキャッシュ有効期間](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/caching/publish.html#cdn-cache-life) および [AEM Author — デフォルトのキャッシュ有効期間](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/caching/author.html?#default-cache-life) を参照してください。

つまり、AEMas a Cloud Serviceは、ほとんどのコンテンツタイプ (HTML、JSON、JS、CSS およびアセット ) をAEM Publish に、および一部のコンテンツタイプ (JS、CSS) をAEMオーサーにキャッシュします。

## キャッシュを有効にする

デフォルトのキャッシュ動作を変更するには、2 つの方法でキャッシュヘッダーを更新します。

1. **Dispatcher vhost の設定：** AEM Publish でのみ使用できます。
1. **カスタム Java™コード：** AEM Publish と Author の両方で使用できます。

これらの各オプションを確認してみましょう。

### Dispatcher vhost の設定

キャッシュを有効にする場合は、このオプションを使用することをお勧めしますが、AEM Publish でのみ使用できます。 キャッシュヘッダーを更新するには、 `mod_headers` モジュールと `<LocationMatch>` Apache HTTP Server の vhost ファイル内のディレクティブ。 一般的な構文は次のとおりです。

    &#39;&#39;conf
    &lt;locationmatch url=&quot;&quot; url_regex=&quot;&quot;>
    #この名前の応答ヘッダーが存在する場合は削除します。 同じ名前のヘッダーが複数ある場合は、すべて削除されます。
    ヘッダーの設定解除 Cache-Control
    ヘッダーの Surrogate-Control の設定を解除
    ヘッダーの有効期限が未設定
    
    # Web ブラウザーと CDN に対して、「max-age」値 (XXX) 秒の応答をキャッシュするように指示します。 「stale-while-revalidate」および「stale-if-error」属性は、CDN レイヤーでの古い状態の処理を制御します。
    ヘッダーセット Cache-Control &quot;max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX&quot;
    
    # CDN に対して、「max-age」の値 (XXX) 秒の応答をキャッシュするように指示します。 「stale-while-revalidate」および「stale-if-error」属性は、CDN レイヤーでの古い状態の処理を制御します。
    ヘッダーセット Surrogate-Control &quot;max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX&quot;
    
    #指定された日時まで応答をキャッシュするように Web ブラウザーと CDN に指示します。
    ヘッダーセットの有効期限「Sun, 31 Dec 2023 23」:59:59 GMT&quot;
    &lt;/locationmatch>
    &quot;&#39;

次に、それぞれの目的をまとめます **ヘッダー** および適用可能 **属性** を設定します。

|                     | Web ブラウザー | CDN | 説明 |
|---------------------|:-----------:|:---------:|:-----------:|
| Cache-Control | ✔ | ✔ | このヘッダーは、Web ブラウザーと CDN キャッシュの有効期間を制御します。 |
| サロゲート制御 | ✘ | ✔ | このヘッダーは、CDN キャッシュの有効期間を制御します。 |
| 有効期限 | ✔ | ✔ | このヘッダーは、Web ブラウザーと CDN キャッシュの有効期間を制御します。 |


- **max-age**：この属性は、応答コンテンツの TTL または「有効期間」を秒単位で制御します。
- **stale-while-revalidate**：この属性は、 _古い状態_ 受信したリクエストが指定された期間（秒）内にある場合の CDN レイヤーでの応答コンテンツの処理。 The _古い状態_ は、TTL の期限が切れてから、応答が再検証されるまでの期間です。
- **stale-if-error**：この属性は、 _古い状態_ オリジンサーバーが利用できず、受信したリクエストが指定された期間（秒）内にある場合の、CDN レイヤーでの応答コンテンツの処理。

以下を確認します。 [行き詰まりと再検証](https://developer.fastly.com/learning/concepts/edge-state/cache/stale/) 詳細を参照してください。

#### 例

Web ブラウザーと CDN キャッシュの有効期間を **HTMLコンテンツタイプ** から _10 分_ 古い状態の処理が行われていない場合は、次の手順に従います。

1. AEMプロジェクト内で、目的のホストファイルを次の場所から見つけます。 `dispatcher/src/conf.d/available_vhosts` ディレクトリ。
1. vhost を更新します ( 例： `wknd.vhost`) ファイルの内容を次に示します。

       &#39;&#39;conf
       &lt;locationmatch content=&quot;&quot;>*\.(html)$&quot;>
       #応答ヘッダーが存在する場合は削除する
       ヘッダーの設定解除 Cache-Control
       
       # Web ブラウザーと CDN に対して、max-age 値 (600) 秒の応答をキャッシュするように指示します。
       ヘッダーセット Cache-Control &quot;max-age=600&quot;
       &lt;/locationmatch>
       &quot;&#39;
   の vhost ファイル `dispatcher/src/conf.d/enabled_vhosts` ディレクトリは **symlinks** を `dispatcher/src/conf.d/available_vhosts` ディレクトリに含まれていない場合は、必ず symlinks を作成してください。
1. を使用して、目的のAEMas a Cloud Service環境に vhost の変更をデプロイします。 [Cloud Manager - Web 層設定パイプライン](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) または [RDE コマンド](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration).

ただし、Web ブラウザーと CDN キャッシュの有効期間に異なる値を設定するには、 `Surrogate-Control` ヘッダーを使用します。 同様に、特定の日時にキャッシュを期限切れにするには、 `Expires` ヘッダー。 また、 `stale-while-revalidate` および `stale-if-error` 属性を指定すると、応答コンテンツの古い状態の処理を制御できます。 AEM WKND プロジェクトには、 [参照古い状態治療](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L150-L155) CDN キャッシュの設定

同様に、他のコンテンツタイプ（JSON、JS、CSS、アセット）のキャッシュヘッダーも更新できます。

### カスタム Java™コード

このオプションは、AEM Publish と Author の両方で使用できます。 ただし、AEMオーサーでキャッシュを有効にして、デフォルトのキャッシュ動作を維持することはお勧めしません。

キャッシュヘッダーを更新するには、 `HttpServletResponse` オブジェクトがカスタム Java™コード（Sling サーブレット、Sling サーブレットフィルター）に含まれている。 一般的な構文は次のとおりです。

    &quot;&#39;java
    // Web ブラウザーおよび CDN に対して、「max-age」値 (XXX) 秒の応答をキャッシュするように指示します。 「stale-while-revalidate」および「stale-if-error」属性は、CDN レイヤーでの古い状態の処理を制御します。
    response.setHeader(&quot;Cache-Control&quot;, &quot;max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX&quot;);
    
    // CDN に対して、「max-age」値 (XXX) 秒の応答をキャッシュするように指示します。 「stale-while-revalidate」および「stale-if-error」属性は、CDN レイヤーでの古い状態の処理を制御します。
    response.setHeader(&quot;Surrogate-Control&quot;, &quot;max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX&quot;);
    
    // Web ブラウザーと CDN に対して、指定された日時まで応答をキャッシュするように指示します。
    response.setHeader(&quot;Expires&quot;, &quot;Sun, 31 Dec 2023 23:59:59 GMT&quot;);
    &quot;&#39;
