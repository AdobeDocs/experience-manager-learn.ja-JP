---
title: AEM Assets のクローズドユーザーグループ
description: クローズドユーザーグループ（CUG）は、公開されたサイト上の特定のユーザーグループに対するコンテンツへのアクセスを制限するために使用する機能です。このビデオでは、クローズドユーザーグループを Adobe Experience Manager Assets と共に使用して、アセットの特定のフォルダーへのアクセスを制限する方法を示します。
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
jira: KT-649
thumbnail: 22155.jpg
last-substantial-update: 2022-06-06T00:00:00Z
doc-type: Feature Video
exl-id: a2bf8a82-15ee-478c-b7c3-de8a991dfeb8
duration: 321
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '357'
ht-degree: 100%

---

# クローズドユーザーグループ{#using-closed-user-groups-with-aem-assets}

クローズドユーザーグループ（CUG）は、公開されたサイト上の特定のユーザーグループに対するコンテンツへのアクセスを制限するために使用する機能です。このビデオでは、クローズドユーザーグループを Adobe Experience Manager Assets と共に使用して、アセットの特定のフォルダーへのアクセスを制限する方法を示します。AEM Assets でのクローズドユーザーグループのサポートは、AEM 6.4 で初めて導入されました。

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## AEM Assets を使用したクローズドユーザーグループ（CUG）

* AEM パブリッシュインスタンス上のアセットへのアクセスを制限するように設計されています。
* 一連のユーザー／グループに対する読み取りアクセス権を付与します。
* CUG は、フォルダーレベルでのみ設定できます。CUG は個々のアセットでは設定できません。
* CUG ポリシーは、任意のサブフォルダーと適用されたアセットに自動的に継承されます。
* CUG ポリシーは、新しい CUG ポリシーを設定することで、サブフォルダーによって上書きできます。これは慎重に使用する必要があり、ベストプラクティスとは見なされません。

## クローズドユーザーグループとアクセス制御リスト {#closed-user-groups-vs-access-control-lists}

クローズドユーザーグループ（CUG）とアクセス制御リスト（ACL）の両方を使用して、AEM のコンテンツへのアクセスを制御し、AEM セキュリティのユーザーとグループに基づいて制御します。ただし、これらの機能の適用と実装は大きく異なります。この 2 つの機能の違いを次の表にまとめました。

|                   | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| 使用目的 | **現在の** AEM インスタンスのコンテンツに対する権限を設定し、適用します。 | AEM **オーサー**&#x200B;インスタンスのコンテンツに CUG ポリシーを設定します。AEM **パブリッシュ**&#x200B;インスタンス上のコンテンツに CUG ポリシーを適用します。 |
| 権限レベル | すべてのレベルのユーザー／グループに対して許可／拒否された権限を定義します。読み取り、変更、作成、削除、ACL の読み取り、ACL の編集、複製。 | 一連のユーザー／グループに対する読み取りアクセス権を付与します。*他のすべての*&#x200B;ユーザー／グループの読み取りアクセスを拒否します。 |
| パブリケーション | ACL はコンテンツと一緒に公開されることは&#x200B;*ありません*。 | CUG ポリシー&#x200B;*が*&#x200B;コンテンツと共に公開されます。 |

## サポートリンク {#supporting-links}

* [アセットとクローズドたユーザーグループの管理](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=ja#closed-user-group)
* [クローズドユーザーグループの作成](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html?lang=ja)
* [Oak クローズドユーザーグループのドキュメント](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
