---
title: AEM Assetsの非公開ユーザーグループ
description: 'Closed User Groups(CUGs)は、公開済みサイト上の特定のユーザーグループに対してコンテンツへのアクセスを制限するために使用される機能です。 このビデオでは、閉じたユーザーグループをAdobe Experience Managerアセットと共に使用して、アセットの特定のフォルダへのアクセスを制限する方法を示します。 AEM Assetsを使用したクローズドユーザーグループのサポートは、AEM 6.4で初めて導入されました。 '
feature: asset-share
topics: authoring, collaboration, operations, sharing
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 10784dce34443adfa1fc6dc324242b1c021d2a17
workflow-type: tm+mt
source-wordcount: '486'
ht-degree: 2%

---


# 閉じられたユーザーグループ{#using-closed-user-groups-with-aem-assets}

Closed User Groups(CUGs)は、公開済みサイト上の特定のユーザーグループに対してコンテンツへのアクセスを制限するために使用される機能です。 このビデオでは、閉じたユーザーグループをAdobe Experience Managerアセットと共に使用して、アセットの特定のフォルダへのアクセスを制限する方法を示します。 AEM Assetsを使用したクローズドユーザーグループのサポートは、AEM 6.4で初めて導入されました。

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=9&learn=on)

## Closed User Group(CUG)とAEM Assets

* AEM発行インスタンスのアセットへのアクセスを制限するように設計されています。
* 一連のユーザー/グループに対する読み取りアクセス権を付与します。
* CUGは、フォルダーレベルでのみ設定できます。 CUGは個々のアセットに設定できません。
* CUGポリシーは、すべてのサブフォルダーと適用されたアセットに自動的に継承されます。
* CUGポリシーは、新しいCUGポリシーを設定することで、サブフォルダーによって上書きできます。 これは慎重に使用する必要があり、ベストプラクティスとは見なされません。

## JCR {#cug-representation-in-the-jcr}のCUG表現

![JCRでのCUG表現](assets/closed-user-groups/folder-properties-closed-user-groups.png)

We.Retail Members GroupがClosed User Groupとしてフォルダーに追加されました：/content/dam/we-retail/en/beta-products

**rep:CugMixin**&#x200B;のミックスインが&#x200B;**/content/dam/we-retail/en/beta-products**&#x200B;フォルダーに適用されます。 **rep:cugPolicy**&#x200B;のノードがフォルダーの下に追加され、we-retail-membersがプリンシパルとして指定されます。 **granite:AuthenticationRequired**&#x200B;の別のミックスインがベータ製品フォルダーに適用され、プロパティ** granite:loginPath**は、ユーザーが認証されずに&#x200B;**beta-products**&#x200B;フォルダーの下のアセットを要求しようとする場合に使用するログインページを指定します。

以下のJCRの説明：

```xml
/beta-products
    - jcr:primaryType = sling:Folder
    - jcr:mixinTypes = rep:CugMixin, granite:AuthenticationRequired
    - granite:loginPath = /content/we-retail/us/en/community/signin
    + rep:cugPolicy
         - jcr:primaryType = rep:CugPolicy
         - rep:principalNames = we-retail-members
```

## 閉じたユーザーグループとアクセス制御リスト{#closed-user-groups-vs-access-control-lists}

Closed User Groups(CUG)とアクセス制御リスト(ACL)は共に、AEMのコンテンツへのアクセスを制御するのに使用され、AEMセキュリティユーザーとグループに基づいています。 ただし、これらの機能の適用と実装は非常に異なります。 次の表に、2つの機能の違いをまとめます。

|  | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| 用途 | **現在の** AEMインスタンスのコンテンツに対する権限を設定し、適用します。 | AEM **作成者**&#x200B;インスタンスのコンテンツに対するCUGポリシーを設定します。 AEM **パブリッシュ**&#x200B;インスタンスのコンテンツにCUGポリシーを適用します。 |
| 権限レベル | すべてのレベルのユーザー/グループに許可/拒否された権限を定義します。読み取り、変更、作成、削除、ACLの読み取り、ACLの編集、複製を行います。 | 一連のユーザー/グループに対する読み取りアクセス権を付与します。 他のすべてのユーザー/グループに対する読み取りアクセスを拒否します。 |
| レプリケーション | ACLはコンテンツと共に複製されません。 | CUGポリシーはコンテンツと共に複製されます。 |

## サポートリンク{#supporting-links}

* [アセットおよび閉じたユーザーグループの管理](https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets-touch-ui.html#ClosedUserGroup)
* [閉じられたユーザーグループの作成](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/cug.html)
* [Oak Closed User Groupドキュメント](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
