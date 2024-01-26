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
exl-id: 544c3230-6eb6-4f06-a63c-f56d65c0ff4b
duration: 200
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '637'
ht-degree: 5%

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

```
<LocationMatch "$URL$ || $URL_REGEX$">
    # Removes the response header of this name, if it exists. If there are multiple headers of the same name, all will be removed.
    Header unset Cache-Control
    Header unset Surrogate-Control
    Header unset Expires

    # Instructs the web browser and CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
    Header set Cache-Control "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX"
    
    # Instructs the CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
    Header set Surrogate-Control "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX"
    
    # Instructs the web browser and CDN to cache the response until the specified date and time.
    Header set Expires "Sun, 31 Dec 2023 23:59:59 GMT"
</LocationMatch>
```

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

   ```
   <LocationMatch "^/content/.*\.(html)$">
       # Removes the response header if present
       Header unset Cache-Control
   
       # Instructs the web browser and CDN to cache the response for max-age value (600) seconds.
       Header set Cache-Control "max-age=600"
   </LocationMatch>
   ```

   の vhost ファイル `dispatcher/src/conf.d/enabled_vhosts` ディレクトリは **symlinks** を `dispatcher/src/conf.d/available_vhosts` ディレクトリに含まれていない場合は、必ず symlinks を作成してください。
1. を使用して、目的のAEMas a Cloud Service環境に vhost の変更をデプロイします。 [Cloud Manager - Web 層設定パイプライン](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html?#web-tier-config-pipelines) または [RDE コマンド](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/rde/how-to-use.html?lang=en#deploy-apache-or-dispatcher-configuration).

ただし、Web ブラウザーと CDN キャッシュの有効期間に異なる値を設定するには、 `Surrogate-Control` ヘッダーを使用します。 同様に、特定の日時にキャッシュを期限切れにするには、 `Expires` ヘッダー。 また、 `stale-while-revalidate` および `stale-if-error` 属性を指定すると、応答コンテンツの古い状態の処理を制御できます。 AEM WKND プロジェクトには、 [参照古い状態治療](https://github.com/adobe/aem-guides-wknd/blob/main/dispatcher/src/conf.d/available_vhosts/wknd.vhost#L150-L155) CDN キャッシュの設定

同様に、他のコンテンツタイプ（JSON、JS、CSS、アセット）のキャッシュヘッダーも更新できます。

### カスタム Java™コード

このオプションは、AEM Publish と Author の両方で使用できます。 ただし、AEMオーサーでキャッシュを有効にして、デフォルトのキャッシュ動作を維持することはお勧めしません。

キャッシュヘッダーを更新するには、 `HttpServletResponse` オブジェクトがカスタム Java™コード（Sling サーブレット、Sling サーブレットフィルター）に含まれている。 一般的な構文は次のとおりです。

```java
// Instructs the web browser and CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
response.setHeader("Cache-Control", "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX");

// Instructs the CDN to cache the response for 'max-age' value (XXX) seconds. The 'stale-while-revalidate' and 'stale-if-error' attributes controls the stale state treatment at CDN layer.
response.setHeader("Surrogate-Control", "max-age=XXX,stale-while-revalidate=XXX,stale-if-error=XXX");

// Instructs the web browser and CDN to cache the response until the specified date and time.
response.setHeader("Expires", "Sun, 31 Dec 2023 23:59:59 GMT");
```
