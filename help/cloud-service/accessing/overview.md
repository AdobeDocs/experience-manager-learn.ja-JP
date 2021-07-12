---
title: AEM as a Cloud Service へのアクセスの設定
description: 'AEM as a AEMCloud Serviceは、アプリケーションを利用するクラウドネイティブな方法です。そのため、AdobeIMS(Identity Management System)を活用して、管理者と通常のユーザーの両方をAEM Authorサービスにログインしやすくします。 AdobeのIMSユーザー、ユーザーグループ、製品プロファイルを、AEMグループと共に使用し、AEMオーサーへの特定のアクセス権を付与する方法について説明します。  '
feature: 'ユーザーとグループ '
topics: authentication
version: cloud-service
activity: setup
audience: administrator
doc-type: article
kt: 5882
thumbnail: KT-5882.jpg
topic: 管理、セキュリティ
role: Admin
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 7%

---


# AEM as a Cloud Service へのアクセスの設定

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="AdobeIMSの概要"
>abstract="AEM as aCloud Serviceでは、AdobeIMS(Identity Management System)を利用して、管理者と通常のユーザーの両方をAEMオーサーサービスにログインしやすくします。 AEMオーサーサービスへの詳細なアクセスを提供するために、AdobeのIMSユーザー、グループ、製品プロファイルをAEMグループおよび権限と連携させて使用する方法について説明します。"

AEM as a AEMCloud Serviceは、アプリケーションを利用するクラウドネイティブな方法です。そのため、AdobeIMS(Identity Management System)を活用して、管理者と通常のユーザーの両方をAEM Authorサービスにログインしやすくします。 AEMオーサーサービスへの詳細なアクセスを提供するために、AdobeのIMSユーザー、グループ、製品プロファイルをAEMグループおよび権限と連携させて使用する方法について説明します。

## AdobeIMSユーザー

AEMオーサーサービスへのアクセスを必要とするAdobeは、[AdobeのAdminConsole](https://adminconsole.adobe.com)で[ユーザーIMSユーザー](https://helpx.adobe.com/jp/enterprise/using/set-up-identity.html)として管理されます。 AdobeIMSユーザーとは何か、およびユーザーがAdmin Consoleでアクセスし、管理する方法について説明します。

[AdobeIMSユーザーの詳細](./adobe-ims-users.md)

## AdobeIMSユーザーグループ

AEMオーサーサービスにアクセスするユーザーは、[AdobeのAdminConsole](https://adminconsole.adobe.com)の[AdobeIMSユーザーグループ](https://helpx.adobe.com/enterprise/using/user-groups.html)を使用して論理グループに編成する必要があります。 AdobeのIMSAdobeグループは、AEMへの直接の権限やアクセス権を提供しません（[ユーザーのIMS製品プロファイル](#adobe-ims-product-profiles)のジョブです）が、AEMグループと権限を使用して、AEMオーサーサービス内の特定のレベルのアクセス権に変換できます。

[AdobeIMSユーザーグループの詳細](./adobe-ims-user-groups.md)

## AdobeIMS製品プロファイル

[AdobeIMS製品プロファイル](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html)は、 [AdobeのAdminConsole](https://adminconsole.adobe.com)で管理され、基本レベルのアクセスを使用してAEM Authorサービスにログインするための [AdobeIMSユーザーアクセスを提供するメカニズムで](#adobe-ims-users) す。

+ __AEM Users__&#x200B;製品プロファイルを使用すると、AEM寄稿者グループのメンバーシップを介してAEMに対する読み取り専用アクセス権をユーザーに付与できます。
+ __AEM Administrators__&#x200B;製品プロファイルを使用すると、AEMに対する管理者アクセス権をユーザーに付与できます。

[AdobeIMS製品プロファイルの詳細](./adobe-ims-product-profiles.md)

## AEMユーザーのグループと権限

Adobe Experience Managerは、AdobeIMSユーザー、ユーザーグループ、製品プロファイルに基づいて構築され、AEMへのカスタマイズ可能なアクセスをユーザーに提供します。 AEMのグループと権限を構築する方法、およびAdobeのIMS抽象概念と連携してAEMへのシームレスでカスタマイズ可能なアクセスを提供する方法について説明します。

[AEMのユーザー、グループ、および権限について説明します](./aem-users-groups-and-permissions.md)

## アクセスおよび権限の手順

AdobeAdminConsoleでのAdobeIMSユーザー、ユーザーグループ、Adobeプロファイルの設定、およびAEM Authorでこれらの製品IMSの抽象概念を活用して特定のグループベースの権限を定義および管理する方法に関する簡潔なチュートリアルです。

[AEMのアクセスと権限のウォークスルー](./walk-through.md)

## その他のAdobe Admin Consoleリソース

次のドキュメントでは、Adobe Admin Console](https://adminconsole.adobe.com)に関する詳細と懸念事項を説明します。Adobe Admin Consoleをより深く理解し、を使用してExperience Cloud製品間のユーザー管理やアクセスを行う際に役立ちます。[

+ [Adobe Admin Console IDの概要](https://helpx.adobe.com/enterprise/using/identity.html)
+ [Adobe Admin Console管理者ロール](https://helpx.adobe.com/jp/enterprise/using/admin-roles.html)
+ [Adobe Admin Console開発者ロール](https://helpx.adobe.com/jp/enterprise/using/manage-developers.html)