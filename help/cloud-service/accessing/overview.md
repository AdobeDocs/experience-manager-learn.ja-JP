---
title: AEM as a Cloud Service へのアクセスの設定
description: AEM as a AEMCloud Serviceは、アプリケーションを利用するクラウドネイティブな方法です。そのため、Adobe IMS(Identity Management System)を活用して、管理者と通常のユーザーの両方をAEM Authorサービスにログインしやすくします。 Adobe IMSユーザー、ユーザーグループ、製品プロファイルを、AEMグループと共に使用し、AEMオーサーへの特定のアクセス権を付与する方法について説明します。
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Beginner
kt: 5882
thumbnail: KT-5882.jpg
exl-id: 4846a394-cf8e-4d52-8f8b-9e874f2f457b
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '598'
ht-degree: 7%

---

# AEM as a Cloud Service へのアクセスの設定

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Adobe IMSの概要"
>abstract="AEM as aCloud Serviceでは、Adobe IMS(Identity Management System)を利用して、管理者と通常のユーザーの両方をAEMオーサーサービスにログインしやすくします。 AEMオーサーサービスへの詳細なアクセスを提供するために、Adobe IMSのユーザー、グループ、製品プロファイルをAEMグループおよび権限と連携させて使用する方法について説明します。"

AEM as a AEMCloud Serviceは、アプリケーションを利用するクラウドネイティブな方法です。そのため、Adobe IMS(Identity Management System)を活用して、管理者と通常のユーザーの両方をAEM Authorサービスにログインしやすくします。

![Adobe Admin Console](./assets/hero.png)

AEMオーサーサービスへの詳細なアクセスを提供するために、Adobe IMSのユーザー、グループ、製品プロファイルをAEMグループおよび権限と連携させて使用する方法について説明します。

## Adobe IMSユーザー

AEMオーサーサービスへのアクセスを必要とするAdobe IMSは、[AdobeのAdminConsole](https://adminconsole.adobe.com)で[ユーザーユーザー](https://helpx.adobe.com/jp/enterprise/using/set-up-identity.html)として管理されます。 Adobe IMSユーザーとは何か、およびユーザーがAdmin Consoleでアクセスし、管理する方法について説明します。

[Adobe IMSユーザー](./adobe-ims-users.md)

## Adobe IMSユーザーグループ

AEMオーサーサービスにアクセスするユーザーは、[AdobeのAdminConsole](https://adminconsole.adobe.com)の[Adobe IMSユーザーグループ](https://helpx.adobe.com/enterprise/using/user-groups.html)を使用して論理グループに編成する必要があります。 Adobe IMSユーザーグループは、AEMへの直接の権限やアクセス権を提供しません([Adobe IMS製品プロファイル](#adobe-ims-product-profiles)のジョブです)が、AEMグループと権限を使用して、AEMオーサーサービスの特定のレベルのアクセス権に変換できます。

[ユーザーユーザーグループのAdobe IMS](./adobe-ims-user-groups.md)

## Adobe IMS製品プロファイル

[Adobe IMS製品プロファイル](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html)は、 [AdobeのAdminConsoleで管理され](https://adminconsole.adobe.com)、基本レベルのアクセス権を持つAEMオーサーサービスにログインするためのAdobe IMSユーザーアクセスを提供するメカニズムで [](#adobe-ims-users) す。

+ __AEM Users__&#x200B;製品プロファイルを使用すると、AEM寄稿者グループのメンバーシップを介してAEMに対する読み取り専用アクセス権をユーザーに付与できます。
+ __AEM Administrators__&#x200B;製品プロファイルを使用すると、AEMに対する管理者アクセス権をユーザーに付与できます。

[製品プロファイルのAdobe IMS](./adobe-ims-product-profiles.md)

## AEMユーザーのグループと権限

Adobe Experience Managerは、Adobe IMSユーザー、ユーザーグループ、製品プロファイルに基づいて構築され、AEMへのカスタマイズ可能なアクセスをユーザーに提供します。 AEMのグループと権限を構築する方法、およびAdobe IMSの抽象概念と連携してAEMへのシームレスでカスタマイズ可能なアクセスを提供する方法について説明します。

[AEMのユーザー、グループ、および権限について説明します](./aem-users-groups-and-permissions.md)

## アクセスおよび権限の手順

AdobeAdminConsoleでAdobe IMSユーザー、ユーザーグループ、Adobe IMSプロファイルを構成する簡潔なチュートリアル、およびAEM Authorでこれらの製品の抽象概念を活用して特定のグループベースの権限を定義および管理する方法。

[AEMのアクセスと権限のウォークスルー](./walk-through.md)

## その他のAdobe Admin Consoleリソース

次のドキュメントでは、Adobe Admin Console](https://adminconsole.adobe.com)に関する詳細と懸念事項を説明します。Adobe Admin Consoleをより深く理解し、を使用してExperience Cloud製品間のユーザー管理やアクセスを行う際に役立ちます。[

+ [Adobe Admin Console IDの概要](https://helpx.adobe.com/enterprise/using/identity.html)
+ [Adobe Admin Console管理者ロール](https://helpx.adobe.com/jp/enterprise/using/admin-roles.html)
+ [Adobe Admin Console開発者ロール](https://helpx.adobe.com/jp/enterprise/using/manage-developers.html)
