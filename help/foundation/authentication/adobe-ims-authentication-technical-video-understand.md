---
title: Adobe Managed ServicesでのAdobeIMS認証についてAEM
description: Adobe Experience Managerでは、Managed Services上のAEMで、AEMインスタンスとAdobeIMS(Identity Managementシステム)ベースの認証のAdmin Consoleサポートが導入されました。   この統合により、AEM Managed Servicesのお客様は、1つの統合WebコンソールですべてのExperience Cloudユーザーを管理できます。 ユーザーとグループをAEMインスタンスに関連付けられた製品プロファイルに割り当て、特定のAEMインスタンスに対して一元的に管理されたアクセスを許可できます。
version: 6.4, 6.5
feature: 'ユーザーとグループ '
topics: authentication, security
activity: understand
audience: administrator, architect, developer, implementer
doc-type: technical video
kt: 781
topic: アーキテクチャ
role: Architect
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 4%

---


# Adobe Managed ServicesのAEMを使用したAdobeIMS認証について{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Managerでは、Managed Services上のAEMで、AEMインスタンスとAdobeIMS(Identity Managementシステム)ベースの認証のAdmin Consoleサポートが導入されました。   この統合により、AEM Managed Servicesのお客様は、1つの統合WebコンソールですべてのExperience Cloudユーザーを管理できます。 AEMインスタンスに関連付けられた製品プロファイルにユーザーとグループを割り当て、特定のAEMインスタンスに対して一元的に管理されたアクセスを許可できます。

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* Adobe Experience Manager IMS認証のサポートは、「内部」ユーザー（作成者、レビュー担当者、管理者、開発者など）に対してのみ有効で、Webサイト訪問者などの外部エンドユーザーには適用されません。
* [Admin Console](https://adminconsole.adobe.com/) は、AEM Managed Servicesのお客様をIMS組織として、AEMインスタンスを製品コンテキストとして表します。Admin Consoleシステムおよび製品管理者は、を定義および管理できます。
* AEM Managed Servicesは、トポロジをAdmin Consoleと同期し、製品コンテキストとAEMインスタンスの間に1対1のマッピングを作成します。
* ユーザーがアクセスできるAEMインスタンスは、Admin Console内の製品プロファイルによって決まります。
* 認証のサポートには、SSO用の顧客のSAML2準拠IDPが含まれます。
* Enterprise IDまたはFederated IDのみ（お客様SSO用）がサポートされます(個人AdobeIDはサポートされていません)。

**この機能は、Adobe Managed Servicesのお客様向けにAEM 6.4 SP3以降でサポートされます。*

## ベストプラクティス {#best-practices}

### 権限のAdmin Console

ユーザーレベルで権限とアクセスを適用する場合は、Admin ConsoleとAdobe Experience Managerの両方で避ける必要があります。

Admin Consoleでは、ユーザーは製品コンテキストレベルでユーザーグループを介してアクセス権を付与される必要があります。 通常、ユーザーグループは、Adobe Experience Cloud製品全体でグループの再利用性を高めるために、組織内の論理的な役割によって最も適切に表現されます。

>[!NOTE]
>
> AEMをCloud Serviceとして使用する場合は、Admin Consoleユーザーを製品プロファイルに直接割り当てます。 AEM as a Cloud Serviceでは、Admin Consoleユーザーグループを介した製品プロファイルへのAdmin Consoleユーザー間の推移的な権限はサポートされていません。

### Adobe Experience Managerでの権限の適用

Adobe Experience Managerでは、AdobeIMSから同期されたユーザーグループは、AEMが提供するユーザーグループ](https://helpx.adobe.com/jp/experience-manager/6-4/sites/administering/using/security.html)に追加される必要があります。このグループは、AEMで特定のタスクのセットを実行するための適切な権限で事前設定されます。 [AdobeIMSから同期されたユーザーは、AEMが提供するユーザーグループ](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html)に直接追加しないでください。[
