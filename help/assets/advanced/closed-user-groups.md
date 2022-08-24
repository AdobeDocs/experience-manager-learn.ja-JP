---
title: AEM Assetsの閉じられたユーザーグループ
description: 閉じられたユーザーグループ (CUG) は、公開されたサイト上の特定のユーザーグループに対するコンテンツへのアクセスを制限するために使用する機能です。 このビデオでは、閉じられたユーザーグループをAdobe Experience Manager Assets と共に使用して、アセットの特定のフォルダーへのアクセスを制限する方法を示します。
version: 6.4, 6.5, Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
kt: 649
thumbnail: 22155.jpg
exl-id: a2bf8a82-15ee-478c-b7c3-de8a991dfeb8
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '377'
ht-degree: 4%

---

# 閉じられたユーザーグループ{#using-closed-user-groups-with-aem-assets}

閉じられたユーザーグループ (CUG) は、公開されたサイト上の特定のユーザーグループに対するコンテンツへのアクセスを制限するために使用する機能です。 このビデオでは、閉じられたユーザーグループをAdobe Experience Manager Assets と共に使用して、アセットの特定のフォルダーへのアクセスを制限する方法を示します。 AEM Assetsでの閉じられたユーザーグループのサポートは、AEM 6.4 で初めて導入されました。

>[!VIDEO](https://video.tv.adobe.com/v/22155?quality=12&learn=on)

## AEM Assetsを使用した閉じられたユーザーグループ (CUG)

* AEM パブリッシュインスタンス上のアセットへのアクセスを制限するように設計されています。
* 一連のユーザー/グループに対する読み取りアクセス権を付与します。
* CUG は、フォルダーレベルでのみ設定できます。 CUG は個々のアセットに設定できません。
* CUG ポリシーは、任意のサブフォルダーと適用されたアセットに自動的に継承されます。
* CUG ポリシーは、新しい CUG ポリシーを設定することで、サブフォルダーによって上書きできます。 これは慎重に使用する必要があり、ベストプラクティスとは見なされません。

## 閉じられたユーザーグループとアクセス制御リスト {#closed-user-groups-vs-access-control-lists}

閉じられたユーザーグループ (CUG) とアクセス制御リスト (ACL) の両方を使用して、AEMのコンテンツへのアクセスを制御し、AEMセキュリティのユーザーとグループに基づいて制御します。 ただし、これらの機能の適用と実装は大きく異なります。 次の表に、2 つのフィーチャの違いをまとめます。

|  | ACL | CUG |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| 使用目的 | コンテンツに対する権限の設定と適用 **現在** AEMインスタンス。 | AEM上のコンテンツに対する CUG ポリシーの設定 **作成者** インスタンス。 AEMのコンテンツに対する CUG ポリシーの適用 **公開** インスタンス。 |
| 権限レベル | すべてのレベルのユーザー/グループに対して許可/拒否された権限を定義します。読み取り、変更、作成、削除、ACL の読み取り、ACL の編集、複製。 | 一連のユーザー/グループに対する読み取りアクセス権を付与します。 への読み取りアクセスを拒否 *その他* ユーザー/グループ。 |
| パブリケーション | ACL は *not* コンテンツと共に公開されました。 | CUG ポリシー *が* コンテンツと共に公開されました。 |

## サポートリンク {#supporting-links}

* [アセットと閉じられたユーザーグループの管理](https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/manage-assets.html?lang=en#closed-user-group)
* [閉じられたユーザーグループの作成](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/cug.html)
* [Oak 閉じられたユーザーグループドキュメント](https://jackrabbit.apache.org/oak/docs/security/authorization/cug.html)
