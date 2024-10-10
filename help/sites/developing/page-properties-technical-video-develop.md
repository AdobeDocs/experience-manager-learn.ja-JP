---
title: AEM Sitesでのページプロパティの拡張
description: Adobe Experience Manager Sites でページプロパティのメタデータフィールドを拡張する方法を説明します。 このビデオでは、Sling Resource Merger の機能を使用してこれを実現する、最も効果的な方法を確認できます。
topic: Development
feature: Core Components
role: Developer
level: Intermediate
version: Cloud Service
jira: KT-243
thumbnail: 25173.jpg
doc-type: Technical Video
exl-id: 500f4e07-2686-42a2-8e44-d96dde02a112
duration: 488
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '198'
ht-degree: 100%

---

# ページプロパティの拡張 {#extending-page-properties-in-aem-sites}

ページプロパティのメタデータフィールドのカスタマイズは、一般的にSites の導入で必要となります。 このビデオでは、Sling Resource Merger の機能を使用してこれを実現する、最も効果的な方法を確認できます。

>[!VIDEO](https://video.tv.adobe.com/v/25173?quality=12&learn=on)

上のビデオでは、 [WKND 参照サイト](https://github.com/adobe/aem-guides-wknd)のページプロパティのカスタマイズについて確認できます。

## サンプルの WKND ページプロパティパッケージ

「**基本**」タブのカスタマイズと **WKND** を含む、提供された[サンプル WKND ページプロパティパッケージ](./assets/WKND-PageProperties-Example-Dialog-1.0.zip)を使用できます。[WKND ページコンポーネント](https://github.com/adobe/aem-guides-wknd/blob/main/ui.apps/src/main/content/jcr_root/apps/wknd/components/page/.content.xml#L5)が WCM コアコンポーネントの V3 バージョンを使用し、V3 バージョンで [ソーシャル共有が廃止](https://github.com/adobe/aem-core-wcm-components/pull/1930)されたため、「**ソーシャルメディア**」タブのカスタマイズはサポートされていません。

ただし学習目的で、`sling:resourceSuperType` プロパティ値を使用して WKND ページコンポーネントを WCM コアコンポーネントの V2 バージョンにポイントし、「[ソーシャルメディア](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/page/v2/page/_cq_dialog/.content.xml#L95)」タブをオーバーレイすることができます。詳しくは、[ページプロパティの設定](https://experienceleague.adobe.com/docs/experience-manager-65/developing/extending-aem/page-properties-views.html?lang=ja#configuring-your-page-properties)をご覧ください。

学習目的で、このサンプルパッケージをローカルの AEM SDK または AEM 6.X.X インスタンスにインストールします。
