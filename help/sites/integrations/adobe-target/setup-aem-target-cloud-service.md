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
workflow-type: ht
source-wordcount: '191'
ht-degree: 100%

---

# Adobe Target Cloud Service アカウントの作成 {#adobe-target-cloud-service}

次のビデオでは、AEM as a Cloud Service を Adobe Target に接続する方法について説明します。

この統合により、AEM オーサーサービスは Adobe Target と直接通信し、エクスペリエンスフラグメントを AEM から Target にオファーとしてプッシュできるようになります。この統合では、AEM Sites の web ページに Adobe Target JavaScript（AT.js）が追加&#x200B;*されない*&#x200B;ため、[Target 拡張機能を使用してタグと AEM を統合](../experience-platform/data-collection/tags/connect-aem-tag-property-using-ims.md)します。

>[!WARNING]
>
>このビデオでは、AEM を Adobe Target に接続するための非推奨（廃止予定）の JWT 認証方法を紹介します。ただし、OAuth サーバー間認証方法を使用することをお勧めします。詳しくは、[AEM の JWT から OAuth への資格情報の移行](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/foundation/authentication/jwt-to-oauth-migration)を参照してください。アドビでは、この変更を反映するために、ビデオの更新に取り組んでいます。


>[!VIDEO](https://video.tv.adobe.com/v/41244?quality=12&learn=on)

>[!CAUTION]
>
>ビデオに示されるように、Adobe Target Cloud Services の設定には既知の問題があります。 この問題が解決されるまで、ビデオと同じ手順に従いますが、[従来の Adobe Target Cloud Services の設定](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/aem-target-implementation/using-aem-cloud-services.html?lang=ja)を使用します。
