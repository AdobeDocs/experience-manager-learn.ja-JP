---
title: AEM での Adobe Target Cloud Service アカウントの作成
description: Cloud Service および Adobe IMS 認証を使用した、Adobe Experience Manager as a Cloud Service と Adobe Target の統合
jira: KT-6044
thumbnail: 41244.jpg
topic: Integrations
feature: Integrations
role: Admin
level: Intermediate
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service、AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: dd6c17ae-8e08-4db3-95f9-081cc7dbd86e
duration: 316
source-git-commit: 8ebaba01d4b470a1ca7c1630f55b756796f3640d
workflow-type: tm+mt
source-wordcount: '191'
ht-degree: 70%

---

# Adobe Target Cloud Service アカウントの作成 {#adobe-target-cloud-service}

次のビデオでは、AEM as a Cloud ServiceとAdobe Targetを接続する方法に関するウォークスルーを提供します。

この統合により、AEM オーサーサービスは Adobe Target と直接通信し、エクスペリエンスフラグメントを AEM から Target にオファーとしてプッシュできるようになります。この統合では *AEM Sitesの web ページにAdobe Target JavaScript（AT.js* が追加されないため、[AEMと Target 拡張機能を使用するタグ ](../experience-platform/data-collection/tags/connect-aem-tag-property-using-ims.md) が追加されます。

>[!WARNING]
>
>このビデオでは、AEMをAdobe Targetに接続するための、非推奨（廃止予定）の JWT 認証方法について説明します。 ただし、OAuth サーバー間認証方法を使用することをお勧めします。詳しくは、[AEM の JWT から OAuth への資格情報の移行](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/foundation/authentication/jwt-to-oauth-migration)を参照してください。アドビでは、この変更を反映するために、ビデオの更新に取り組んでいます。


>[!VIDEO](https://video.tv.adobe.com/v/41244?quality=12&learn=on)

>[!CAUTION]
>
>ビデオに示されるように、Adobe Target Cloud Services の設定には既知の問題があります。 この問題が解決されるまで、ビデオと同じ手順に従いますが、[従来の Adobe Target Cloud Services の設定](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/aem-target-implementation/using-aem-cloud-services.html?lang=ja)を使用します。
