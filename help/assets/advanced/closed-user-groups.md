---
title: AEM Assetsの非公開ユーザーグループ
description: Closed User Groups(CUGs)は、公開済みサイト上の特定のユーザーグループに対してコンテンツへのアクセスを制限するために使用される機能です。 このビデオでは、閉じたユーザーグループをAdobe Experience Managerアセットと共に使用して、アセットの特定のフォルダへのアクセスを制限する方法を示します。
version: 6.3, 6.4, 6.5, cloud-service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
kt: 649
thumbnail: 22155.jpg
translation-type: tm+mt
source-git-commit: 407840a0e0c90c4f004390a052d036f9b69fa8df
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 2%

---


# 閉じられたユーザーグループ{#using-closed-user-groups-with-aem-assets}

Closed User Groups(CUGs)は、公開済みサイト上の特定のユーザーグループに対してコンテンツへのアクセスを制限するために使用される機能です。 このビデオでは、閉じたユーザーグループをAdobe Experience Managerアセットと共に使用して、アセットの特定のフォルダへのアクセスを制限する方法を示します。 AEM Assetsを使用したクローズドユーザーグループのサポートは、AEM 6.4で初めて導入されました。

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## Closed User Group(CUG)とAEM Assets

* AEM発行インスタンスのアセットへのアクセスを制限するように設計されています。
* 一連のユーザー/グループに対する読み取りアクセス権を付与します。
* CUGは、フォルダーレベルでのみ設定できます。 CUGは個々のアセットに設定できません。
* CUGポリシーは、すべてのサブフォルダーと適用されたアセットに自動的に継承されます。
* CUGポリシーは、新しいCUGポリシーを設定することで、サブフォルダーによって上書きできます。 これは慎重に使用する必要があり、ベストプラクティスとは見なされません。

## 閉じたユーザーグループとアクセス制御リスト{#closed-user-groups-vs-access-control-lists}

Closed User Groups(CUG)とアクセス制御リスト(ACL)は共に、AEMのコンテンツへのアクセスを制御するのに使用され、AEMセキュリティユーザーとグループに基づいています。 ただし、これらの機能の適用と実装は非常に異なります。 次の表に、2つの機能の違いをまとめます。

|  | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| 用途 | **現在の** AEMインスタンスのコンテンツに対する権限を設定し、適用します。 | AEM **作成者**&#x200B;インスタンスのコンテンツに対するCUGポリシーを設定します。 AEM **パブリッシュ**&#x200B;インスタンスのコンテンツにCUGポリシーを適用します。 |
| 権限レベル | すべてのレベルのユーザー/グループに許可/拒否された権限を定義します。読み取り、変更、作成、削除、ACLの読み取り、ACLの編集、複製を行います。 | 一連のユーザー/グループに対する読み取りアクセス権を付与します。 *他のすべての*&#x200B;ユーザー/グループへの読み取りアクセスを拒否します。 |
| パブリケーション | ACLは、コンテンツと共に&#x200B;**&#x200B;公開されていません。 | CUGポリシー&#x200B;*は、コンテンツと共に*&#x200B;公開されます。 |

## サポートリンク{#supporting-links}

* [アセットおよび閉じたユーザーグループの管理](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=en#closed-user-group)
* [閉じられたユーザーグループの作成](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html)
* [Oak Closed User Groupドキュメント](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
