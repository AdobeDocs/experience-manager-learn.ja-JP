---
title: AEM Sitesでのページプロパティの拡張
description: Adobe Experience Manager Sitesでページプロパティのメタデータフィールドを拡張する方法を説明します。 このビデオでは、Sling Resource Merger の機能を使用してこれを実現する最も効果的な方法を説明します。
topic: Development
feature: Core Components
role: Developer
version: Cloud Service
kt: 243
thumbnail: 25173.jpg
exl-id: 500f4e07-2686-42a2-8e44-d96dde02a112
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '250'
ht-degree: 1%

---

# ページプロパティの拡張 {#extending-page-properties-in-aem-sites}

ページプロパティのメタデータフィールドのカスタマイズは、Sites の導入で一般的に必要とされる要件です。 このビデオでは、Sling Resource Merger の機能を使用してこれを実現する最も効果的な方法を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/25173?quality=12&learn=on)

上記のビデオでは、 [WKND 参照サイト](https://github.com/adobe/aem-guides-wknd).

## サンプルの WKND ページプロパティパッケージ

提供された [サンプル WKND ページプロパティパッケージ](./assets/WKND-PageProperties-Example-Dialog-1.0.zip) 次を含む **WKND** および **基本** 上記のビデオで表示されるタブのカスタマイズ。 この **SocialMedia** タブのカスタマイズは、 [WKND ページコンポーネント](https://github.com/adobe/aem-guides-wknd/blob/main/ui.apps/src/main/content/jcr_root/apps/wknd/components/page/.content.xml#L5) は、WCM コアコンポーネントの V3 バージョンと、V3 バージョンでは [ソーシャル共有は廃止されました](https://github.com/adobe/aem-core-wcm-components/pull/1930).

ただし、学習目的で、 `sling:resourceSuperType` プロパティ値を選択し、 [ソーシャルメディア](https://github.com/adobe/aem-core-wcm-components/blob/main/content/src/content/jcr_root/apps/core/wcm/components/page/v2/page/_cq_dialog/.content.xml#L95) タブをクリックします。 詳しくは、 [ページプロパティの設定](https://experienceleague.adobe.com/docs/experience-manager-64/developing/extending-aem/page-properties-views.html#configuring-your-page-properties)

このサンプルパッケージは、学習目的で、ローカルのAEM SDK またはAEM 6.X.X インスタンスにインストールする必要があります。
