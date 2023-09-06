---
title: AEMオーサーサービスのキャッシュ
description: AEMas a Cloud Serviceオーサーサービスのキャッシュの概要です。
version: Cloud Service
feature: Developer Tools
topic: Performance
role: Architect, Developer
level: Intermediate
doc-type: Article
last-substantial-update: 2023-08-28T00:00:00Z
jira: KT-13858
thumbnail: KT-13858.jpeg
source-git-commit: 6cbd8f3c49d44e75337715c35c198008da8ae7b9
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 3%

---


# AEM オーサー

AEMオーサーは、非常に動的で、提供するコンテンツの権限に影響を受けやすい性質を持つので、キャッシュに制限があります。 一般に、AEMオーサーのキャッシュをカスタマイズすることはお勧めしません。代わりに、Adobeが提供するキャッシュ設定を利用して、パフォーマンスを高めることをお勧めします。

![AEMオーサーキャッシュの概要図](./assets/author/author-all.png){align="center"}

AEMオーサー上のキャッシュをカスタマイズすることはお勧めしませんが、AEMオーサーにはAdobeが管理する CDN があり、AEM Dispatcher がないことを理解すると役立ちます。 AEM Dispatcher を持たないので、AEMオーサー環境ではすべてのAEM Dispatcher 設定が無視されることに注意してください。

## CDN

AEMオーサーサービスは CDN を使用しますが、その目的は製品リソースの配信を強化することです。広範囲に設定しないで、そのまま機能させます。

![AEMパブリッシュキャッシュの概要図](./assets/author/author-cdn.png){align="center"}

AEMオーサー CDN は、エンドユーザー（通常はマーケターやコンテンツのオーサー）とAEMオーサーの間に配置されます。 AEMオーサリングエクスペリエンスを強化する静的アセットなど、不変ファイル（オーサリングコンテンツが作成されていない静的アセットなど）をキャッシュします。

AEMオーサーの CDN は、 [永続クエリのカスタマイズ可能な TTL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?author-instances)、および [カスタムクライアントライブラリの長い TTL](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries).

### デフォルトのキャッシュ有効期間

次のお客様向けリソースは、AEMオーサー CDN によってキャッシュされ、デフォルトのキャッシュ有効期間は次のとおりです。

| コンテンツタイプ | デフォルトの CDN キャッシュの有効期間 |
|:------------ |:---------- |
| [永続クエリ (JSON)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/graphql-api/persisted-queries.html?author-instances) | 1 分 |
| [クライアントライブラリ (JS/CSS)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#client-side-libraries) | 30 日 |
| [その他すべて](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/caching.html#other-content) | キャッシュされていません |


## AEM Dispatcher

AEMオーサーサービスはAEM Dispatcher を含まず、 [CDN](#cdn) キャッシュ用。

