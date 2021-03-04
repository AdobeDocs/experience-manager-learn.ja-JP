---
title: Adobe Managed ServicesでのAEMとのAdobeIMS認証について
description: Adobe Experience Managerは、Managed Services上のAEM用に、AEMインスタンスとAdobeIMS(Identity Managementシステム)ベースのAdmin Console認証のサポートを導入しました。   この統合により、AEMManaged Servicesのお客様は、1つの統合WebコンソールですべてのExperience Cloudユーザーを管理できます。 ユーザとグループは、AEMインスタンスに関連付けられた製品プロファイルに割り当てることができ、特定のAEMインスタンスに対する一元的な管理アクセスを許可できます。
version: 6.4, 6.5
feature: 'ユーザーとグループ '
topics: authentication, security
activity: understand
audience: administrator, architect, developer, implementer
doc-type: technical video
kt: 781
topic: アーキテクチャ
role: アーキテクト
level: 経験豊富な
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 5%

---


# Adobe Managed ServicesでのAEMとのAdobeIMS認証について{#understanding-adobe-ims-authentication-with-aem-on-adobe-managed-services}

Adobe Experience Managerは、Managed Services上のAEM用に、AEMインスタンスとAdobeIMS(Identity Managementシステム)ベースのAdmin Console認証のサポートを導入しました。   この統合により、AEMManaged Servicesのお客様は、1つの統合WebコンソールですべてのExperience Cloudユーザーを管理できます。 ユーザとグループをAEMインスタンスに関連付けられた製品プロファイルに割り当て、特定のAEMインスタンスに対する一元的な管理アクセスを許可できます。

>[!VIDEO](https://video.tv.adobe.com/v/26170?quality=12&learn=on)

* Adobe Experience ManagerIMS認証のサポートは、「内部」ユーザー（作成者、レビュー担当者、管理者、開発者など）に対してのみ行われ、Webサイト訪問者などの外部エンドユーザーには行われません。
* [管理](https://adminconsole.adobe.com/) コンソールは、AEMManaged Servicesのお客様をIMS Orgsとして、AEMインスタンスを製品コンテキストとして表します。Admin Consoleシステム管理者および製品管理者は、
* AEMManaged Servicesでは、トポロジをAdmin Consoleと同期して、製品コンテキストとAEMインスタンス間の1対1のマッピングを作成します。
* Admin Consoleの製品プロファイルは、ユーザーがアクセスできるAEMインスタンスを決定します。
* 認証サポートには、SSO用の顧客のSAML2準拠IDPが含まれます。
* Enterprise IDまたはFederated ID（お客様のSSO用）のみがサポートされます(PersonalAdobeIDはサポートされません)。

**この機能は、Adobe Managed Servicesのお客様向けにAEM 6.4 SP3以降でサポートされます。*

## ベストプラクティス {#best-practices}

### Admin Consoleでの権限の適用

ユーザーレベルでの権限の適用とアクセスは、Admin ConsoleーとAdobe Experience Managerの両方で避ける必要があります。

Admin Consoleでは、ユーザーは、製品コンテキストレベルでユーザーグループを介してアクセス権を付与される必要があります。 一般的に、ユーザーグループは、Adobe Experience Cloud製品全体でのグループの再利用を促進するために、組織内の論理的な役割で表現するのが最適です。

>[!NOTE]
>
> AEMをCloud Serviceとして使用している場合は、Admin Consoleプロファイルを直接製品ユーザーに割り当てます。 Admin Consoleユーザーグループを介したAdmin Consoleプロファイル間の移行権限は、AEMのCloud Serviceとしてのではサポートされていません。

### Adobe Experience Managerでの権限の適用

Adobe Experience Managerでは、AdobeIMSから同期されたユーザーグループは、AEMで特定のタスクセットを実行する適切な権限で事前に設定された[AEMが提供するユーザーグループ](https://helpx.adobe.com/jp/experience-manager/6-4/sites/administering/using/security.html)に追加する必要があります。 AdobeIMSから同期されたユーザーは、[AEMが提供するユーザーグループ](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/security.html)に直接追加しないでください。
