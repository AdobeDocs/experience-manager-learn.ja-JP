---
title: Adobe Managed Services の AEM を使用した Adobe IMS 認証について
description: Adobe Experience Manager では、AEM インスタンスの Admin Console サポートと、Managed Services 上の AEM の Adobe IMS（Identity Management システム）ベースの認証が導入されています。この統合により、AEM Managed Services のお客様は、1 つの統合 web コンソールですべての Experience Cloud ユーザーを管理できるようになります。AEM インスタンスに関連付けられた製品プロファイルにユーザーとグループを割り当て、特定の AEM インスタンスへのアクセスを一元管理できます。
version: Experience Manager 6.4, Experience Manager 6.5
feature: User and Groups
doc-type: Technical Video
jira: KT-781
topic: Architecture
role: Architect
level: Experienced
exl-id: 52dd8a3f-6461-4acb-87ca-5dd9567d15a6
last-substantial-update: 2022-10-01T00:00:00Z
thumbnail: KT-781.jpg
duration: 405
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 100%

---

# Adobe Managed Services の AEM を使用した Adobe IMS 認証について{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Manager では、AEM インスタンスに対する Admin Console サポートと、Managed Services 上の AEM 用 Adobe Identity Management システム（IMS）ベースの認証が導入されています。この統合により、AEM Managed Services のお客様は、1 つの統合 web コンソールですべての Experience Cloud ユーザーを管理できるようになります。AEM インスタンスに関連付けられた製品プロファイルにユーザーとグループを割り当て、特定の AEM インスタンスへのアクセスを一元管理できます。

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* Adobe Experience Manager IMS 認証のサポートは、「内部」ユーザー（作成者、レビュー担当者、管理者、開発者など）のみが対象で、web サイトの訪問者など外部のエンドユーザーには適用されません。
* [Admin Console](https://adminconsole.adobe.com/) は、AEM Managed Services の顧客を IMS 組織として、AEM インスタンスを製品コンテキストとして表します。Admin Console システムと製品管理者は、定義と管理ができます。
* AEM Managed Services は、トポロジを Admin Console と同期し、製品コンテキストと AEM インスタンスの間に 1 対 1 のマッピングを作成します。
* Admin Console の製品プロファイルによって、ユーザーがアクセスできる AEM インスタンスが決まります。
* 認証サポートには、SSO 用のお客様の SAML2 準拠 IDP が含まれます。
* Enterprise ID または Federated ID （お客様の SSO 用）のみがサポートされています（個人の Adobe ID はサポートされていません )。

*&#42;この機能は、Adobe Managed Services のお客様向けの AEM 6.4 SP3 以降でサポートされます。*

## ベストプラクティス {#best-practices}

### Admin Console での権限の適用

Admin Console と Adobe Experience Manager の両方で、ユーザーレベルで権限とアクセスを適用することは避ける必要があります。

Admin Console では、製品コンテキストレベルでユーザーグループを介してユーザーにアクセス権を付与する必要があります。ユーザーグループは、通常、組織内の論理的な役割によって表現され、Adobe Experience Cloud 製品間でのグループの再利用性を促進するのに最適です。

### Adobe Experience Manager での権限の適用

Adobe Experience Manager では、Adobe IMS から同期されたユーザーグループを [AEM 提供のユーザーグループ](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=ja)に追加する必要があります。このユーザーグループには、AEM で特定のタスクセットを実行するための適切な権限があらかじめ設定されています。Adobe IMS から同期されたユーザーは、[AEM 提供のユーザーグループ](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security.html?lang=ja)に直接追加しないでください。
