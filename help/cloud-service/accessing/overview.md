---
title: Cloud ServiceとしてのAEMへのアクセスの設定
description: 'AEMは、AEMCloud Serviceをクラウドネイティブで活用する手段です。したがって、AdobeIMS(Identity Managementシステム)を利用して、ユーザー、管理者、正規ユーザーの両方をAEM Authorサービスに容易にログインできます。 AdobeのIMSユーザー、ユーザーグループ、製品プロファイルを、AEMグループと共に使用し、AEM Authorへの特定のアクセス権を提供する方法について説明します。  '
feature: users-and-groups
topics: authentication
version: cloud-service
activity: setup
audience: administrator
doc-type: article
kt: 5882
thumbnail: KT-5882.jpg
translation-type: tm+mt
source-git-commit: 9cf01dbf9461df4cc96d5bd0a96c0d4d900af089
workflow-type: tm+mt
source-wordcount: '537'
ht-degree: 5%

---


# Cloud ServiceとしてのAEMへのアクセスの設定

AEMは、AEMCloud Serviceをクラウドネイティブで活用する手段です。したがって、AdobeIMS(Identity Managementシステム)を利用して、ユーザー、管理者、正規ユーザーの両方をAEM Authorサービスに容易にログインできます。 AdobeIMSユーザー、グループ、製品プロファイルがAEMグループと連携して、AEM Authorサービスへの詳細なアクセス権を提供する方法を説明します。

## AdobeIMSユーザー

AEM Authorサービスへのアクセスを必要とするユーザーは、[AdobeのAdminConsole](https://adminconsole.adobe.com)で、[AdobeIMSユーザー](https://helpx.adobe.com/jp/enterprise/using/set-up-identity.html)として管理されます。 AdobeIMSユーザーの概要、およびAdmin Consoleでのユーザーのアクセス方法と管理方法について説明します。

[AdobeIMSユーザーについて](./adobe-ims-users.md)

## AdobeIMSユーザーグループ

AEM Authorサービスにアクセスするユーザーは、[AdobeのAdminConsole](https://adminconsole.adobe.com)の[AdobeIMSユーザーグループ](https://helpx.adobe.com/enterprise/using/user-groups.html)を使用して論理グループにまとめる必要があります。 AdobeIMSユーザーグループは、直接権限やAEMへのアクセス権を提供しません(これは[AdobeIMSプロファイル](#adobe-ims-product-profiles)の職務です)が、AEMグループと権限を使用して、AEM Authorサービスで特定のレベルのアクセス権に変換できるユーザーの論理グループを定義する優れた方法です。

[AdobeのIMSユーザーグループについて説明します](./adobe-ims-user-groups.md)

## AdobeIMS製品プロファイル

[AdobeIMS製品プロファイル](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html)は、 [AdobeのAdminConsoleで管理され、](https://adminconsole.adobe.com) [](#adobe-ims-users) AdobeIMSユーザーがアクセスの基本レベルでAEM Authorサービスにログインするためのアクセス権を提供するメカニズムです。

+ __AEM Users__&#x200B;製品プロファイルを使用すると、AEM Contributorsグループのメンバーシップを介したAEMへの読み取り専用アクセスが可能になります。
+ __AEM管理者__&#x200B;製品プロファイルは、ユーザーに対してフルな管理アクセスを提供します。

[AdobeIMS製品プロファイルについて説明します。](./adobe-ims-product-profiles.md)

## AEMユーザーのグループと権限

Adobe Experience Managerは、AdobeIMSユーザー、ユーザーグループ、および製品プロファイルを基に構築されており、ユーザーがAEMへのカスタマイズ可能なアクセスを提供します。 AEMグループと権限を構築する方法、およびAdobe IMSの抽象概念と連携してAEMへのシームレスでカスタマイズ可能なアクセスを提供する方法について説明します。

[AEMユーザー、グループ、権限について説明します](./aem-users-groups-and-permissions.md)

## アクセスと権限のウォークスルー

Adobe管理コンソールのAdobeIMSユーザー、ユーザーグループ、製品プロファイルを構成する簡潔なチュートリアル、およびAEM AuthorでこれらのAdobeIMSの抽象概念を活用して、特定のグループベースの権限を定義および管理する方法を示します。

[AEMアクセスと権限のウォークスルー](./walk-through.md)

## その他のAdobe Admin Console資源

以下のドキュメントは、[Adobe Admin Console](https://adminconsole.adobe.com)に固有の詳細と懸念事項を扱っており、Adobe Admin Consoleをより深く理解し、それを使用してExperience Cloudを管理し、複数の製品にアクセスするのに役立ちます。

+ [Adobe Admin Consoleアイデンティティの概要](https://helpx.adobe.com/enterprise/using/identity.html)
+ [Adobe Admin Console管理者ロール](https://helpx.adobe.com/jp/enterprise/using/admin-roles.html)
+ [Adobe Admin Console開発者ロール](https://helpx.adobe.com/jp/enterprise/using/manage-developers.html)