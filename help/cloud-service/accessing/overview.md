---
title: AEM as a Cloud Service へのアクセスの設定
description: AEM as a Cloud ServiceはAEMアプリケーションを利用するクラウドネイティブな方法です。そのため、Adobe IMS(Identity Management System) を利用して、管理者と通常のユーザーの両方のユーザーを AEM Author サービスにログインしやすくします。 Adobe IMSユーザー、ユーザーグループ、製品プロファイルを、AEMグループおよび権限と組み合わせて使用し、AEM オーサーへの特定のアクセスを提供する方法について説明します。
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Beginner
kt: 5882
thumbnail: KT-5882.jpg
last-substantial-update: 2022-10-06T00:00:00Z
exl-id: 4846a394-cf8e-4d52-8f8b-9e874f2f457b
source-git-commit: 4c91ab68f6e31f0eb549689c7ecfd0ee009801d9
workflow-type: tm+mt
source-wordcount: '626'
ht-degree: 7%

---

# AEM as a Cloud Service へのアクセスの設定 {#configuring-access-to-aem-as-a-cloud-service}

>[!CONTEXTUALHELP]
>id="aemcloud_adobeims_overview"
>title="Adobe IMSの概要"
>abstract="AEM as a Cloud Serviceでは、Adobe IMS(Identity Management System) を利用して、管理者と通常のユーザーの両方を AEM オーサーサービスにログインしやすくします。 Adobe IMSのユーザー、グループ、製品プロファイルをAEMグループおよび権限と連携させて使用し、AEM オーサーサービスへの詳細なアクセスを提供する方法について説明します。"

AEM as a Cloud ServiceはAEMアプリケーションを利用するクラウドネイティブな方法です。そのため、Adobe IMS(Identity Management System) を利用して、管理者と通常のユーザーの両方を AEM Author サービスにログインしやすくします。

![Adobe Admin Console](./assets/hero.png)

Adobe IMSのユーザー、グループ、製品プロファイルをAEMグループおよび権限と連携させて使用し、AEM オーサーサービスへの詳細なアクセスを提供する方法について説明します。

## Adobe IMSユーザー

AEM オーサーサービスへのアクセスを必要とするユーザーは、次のように管理されます。 [Adobe IMSユーザー](https://helpx.adobe.com/jp/enterprise/using/set-up-identity.html) in [Adobeの AdminConsole](https://adminconsole.adobe.com). ユーザーのAdobe IMS、およびユーザーがAdmin Consoleでアクセスおよび管理される方法について説明します。

>[!NOTE]
>
>IMS ユーザーを AdminConsole から削除しても、AEMからは自動的に削除されませんが、AEMセッション（トークン）の有効期限が切れると、AEMにログインできなくなります。


[Adobe IMSユーザーの詳細](./adobe-ims-users.md)

## Adobe IMSのユーザーグループ

AEM オーサーサービスにアクセスするユーザーは、 [Adobe IMSのユーザーグループ](https://helpx.adobe.com/jp/enterprise/using/user-groups.html) in [Adobeの AdminConsole](https://adminconsole.adobe.com). Adobe IMSのユーザーグループは、AEMへの直接の権限やアクセス権を提供しません ( これは、 [Adobe IMS製品プロファイル](#adobe-ims-product-profiles)ただし、AEMグループと権限を使用して、AEM オーサーサービス内の特定のレベルのアクセスに変換できる、ユーザーの論理グループを定義する優れた方法です。

[ユーザーグループのAdobe IMSの詳細](./adobe-ims-user-groups.md)

## Adobe IMS製品プロファイル

[Adobe IMS製品プロファイル](https://helpx.adobe.com/enterprise/using/manage-permissions-and-roles.html)、で管理 [Adobeの AdminConsole](https://adminconsole.adobe.com)は、が提供するメカニズムです。 [Adobe IMSユーザー](#adobe-ims-users) 基本レベルのアクセス権を持つ AEM オーサーサービスにログインするためのアクセス権。

+ この __AEM Users__ 製品プロファイルを使用すると、AEM寄稿者グループのメンバーシップを介してAEMに対して読み取り専用アクセス権をユーザーに付与できます。
+ この __AEM Administrators__ 製品プロファイルを使用すると、ユーザーはAEMに対して完全な管理者アクセス権を持ちます。

[製品プロファイルのAdobe IMS](./adobe-ims-product-profiles.md)

## AEMユーザーグループと権限

Adobe Experience Managerは、Adobe IMSユーザー、ユーザーグループおよび製品プロファイルに基づいて構築され、AEMへのカスタマイズ可能なアクセスをユーザーに提供します。 AEMのグループと権限を構築する方法、およびAdobe IMSの抽象概念と連携して、AEMへのシームレスでカスタマイズ可能なアクセスを提供する方法について説明します。

[AEMのユーザー、グループ、権限について説明します](./aem-users-groups-and-permissions.md)

## アクセスおよび権限のウォークスルー

Adobeの AdminConsole でのAdobe IMSユーザー、ユーザーグループ、Adobe IMSプロファイルの設定に関する簡潔な説明と、AEM Author でこれらの製品の抽象概念を活用して特定のグループベースの権限を定義および管理する方法。

[AEMアクセスと権限のウォークスルー](./walk-through.md)

## その他のAdobe Admin Consoleリソース

以下のドキュメントカバー [Adobe Admin Console](https://adminconsole.adobe.com)Adobe Admin Consoleをより深く理解し、それを使用してユーザーやExperience Cloud製品間のアクセスを管理するのに役立つ、具体的な詳細や懸念事項。

+ [Adobe Admin Console ID の概要](https://helpx.adobe.com/jp/enterprise/using/identity.html)
+ [Adobe Admin Console管理者ロール](https://helpx.adobe.com/jp/enterprise/using/admin-roles.html)
+ [Adobe Admin Console Developer Roles](https://helpx.adobe.com/jp/enterprise/using/manage-developers.html)
