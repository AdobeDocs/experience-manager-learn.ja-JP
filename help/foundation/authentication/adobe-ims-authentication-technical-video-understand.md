---
title: Adobe Managed Services のAEMを使用したAdobe IMS認証について
description: Adobe Experience Managerでは、Managed Services上のAEMで、AEMインスタンスとAdobe IMS(Identity Management System) に基づく認証をAdmin Consoleでサポートするようになりました。   この統合により、AEM Managed Servicesのお客様は、1 つの統合 Web コンソールですべてのExperience Cloudユーザーを管理できます。 AEMインスタンスに関連付けられた製品プロファイルにユーザーとグループを割り当てて、特定のAEMインスタンスへの一元的な管理アクセスを許可できます。
version: 6.4, 6.5
feature: User and Groups
topics: authentication, security
activity: understand
audience: administrator, architect, developer, implementer
doc-type: technical video
kt: 781
topic: Architecture
role: Architect
level: Experienced
exl-id: 52dd8a3f-6461-4acb-87ca-5dd9567d15a6
last-substantial-update: 2022-10-01T00:00:00Z
thumbnail: KT-781.jpg
source-git-commit: a156877ff4439ad21fb79f231d273b8983924199
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 4%

---

# Adobe Managed Services のAEMを使用したAdobe IMS認証について{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Managerでは、AEMインスタンスと、Managed Services上のAEM用のAdobeIdentity Managementシステム (IMS) ベースの認証に対するAdmin Consoleのサポートが導入されました。   この統合により、AEM Managed Servicesのお客様は、1 つの統合 Web コンソールですべてのExperience Cloudユーザーを管理できます。 AEMインスタンスに関連付けられた製品プロファイルにユーザーとグループを割り当てて、特定のAEMインスタンスへの一元的な管理アクセスを許可できます。

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* Adobe Experience Manager IMS 認証のサポートは、「内部」ユーザー（作成者、レビュー担当者、管理者、開発者など）に対してのみ有効で、Web サイト訪問者などの外部のエンドユーザーには適用されません。
* [Admin Console](https://adminconsole.adobe.com/) は、AEM Managed Servicesのお客様を IMS 組織として、AEMインスタンスを製品コンテキストとして表します。 Admin Consoleシステムおよび製品管理者は、を定義および管理できます。
* AEM Managed Servicesは、トポロジをAdmin Consoleと同期し、製品コンテキストとAEMインスタンスの間に 1 対 1 のマッピングを作成します。
* ユーザーがアクセスできるAEMインスタンスは、Admin Console内の製品プロファイルによって決まります。
* 認証サポートには、SSO 用のお客様の SAML2 準拠の IDP が含まれます。
* Enterprise ID または Federated ID のみ（お客様の SSO 用）がサポートされます ( 個人のAdobeID はサポートされていません )。

*&#42;この機能は、Adobe Managed Services のお客様のAEM 6.4 SP3 以降でサポートされます。*

## ベストプラクティス {#best-practices}

### Admin Consoleでの権限の適用

ユーザーレベルで権限とアクセスを適用する場合は、Admin ConsoleとAdobe Experience Managerの両方で避ける必要があります。

では、Admin Consoleユーザーは、製品コンテキストレベルでユーザーグループを介してアクセス権を付与される必要があります。 ユーザーグループは、通常、Adobe Experience Cloud製品でのグループの再利用性を促進するために、組織内の論理的な役割によって表現するのが最適です。

>[!NOTE]
>
> AEM as a Cloud Serviceを使用している場合は、Admin Consoleユーザーを製品プロファイルに直接割り当てます。 AEM as a Cloud Serviceでは、Admin Consoleユーザーグループを介した製品プロファイルに対するAdmin Consoleユーザー間の移行的な権限はサポートされていません。

### Adobe Experience Managerでの権限の適用

Adobe Experience Managerでは、Adobe IMSから同期されたユーザーグループは、に追加される必要があります。 [AEM提供のユーザーグループ](https://experienceleague.adobe.com/docs/experience-manager-64/administering/security/security.html?lang=ja):AEMで特定のタスクのセットを実行するための適切な権限に事前設定されています。 Adobe IMSから同期されたユーザーは、に直接追加しないでください [AEM提供のユーザーグループ](https://experienceleague.adobe.com/docs/experience-manager-64/administering/security/security.html).
