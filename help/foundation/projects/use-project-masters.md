---
title: AEMでのプロジェクトマスターの使用方法
description: プロジェクトマスターは、AEM Projectsを使用したユーザーおよびチームの管理を大幅に簡素化します。
version: 6.4, 6.5, Cloud Service
topic: Content Management, Collaboration
feature: Projects
level: Intermediate
role: User
kt: 256
thumbnail: 17740.jpg
exl-id: 78ff62ad-1017-4a02-80e9-81228f9e01eb
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '368'
ht-degree: 0%

---

# プロジェクトマスター

プロジェクトマスターは、[!DNL AEM Projects]を使用してユーザーとチームの管理を大幅に簡素化します。

>[!VIDEO](https://video.tv.adobe.com/v/17740/?quality=12&learn=on)

管理者は、**[!DNL Master Project]**&#x200B;を作成し、プロジェクトチームの一部として役割/権限にユーザーを割り当てることができるようになりました。 プロジェクトは、マスタープロジェクトから作成し、チームメンバーシップを自動的に継承します。 これには、次のような利点があります。

* 複数のプロジェクトでの既存のチームの再利用
* チームを手動で再作成する必要がないので、プロジェクト作成を高速化
* 一元的な場所からチームメンバーシップを管理し、チームに対する更新は、プロジェクトに自動的に継承されます
* パフォーマンスの問題を引き起こす可能性のある重複ACLの作成を回避

[!DNL Master Projects] は、  AEM Projects [!UICONTROL のMastersフォルダー]に作成できます。作成されたマスタープロジェクトは、新しいプロジェクトの作成時にウィザードで使用可能なテンプレートと共にオプションとして表示されます。

[!DNL Project Masters] URL（ローカルAEMオーサーインスタンス）: [http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## 削除 [!DNL Project Masters]

マスタープロジェクトを削除すると、使用できない派生プロジェクトが発生します。

マスタープロジェクトを削除する前に、すべての派生プロジェクトが完了し、AEMから削除されていることを確認します。 派生プロジェクトを削除する前に、必要なプロジェクトデータを必ず保存してください。 すべての派生プロジェクトをAEMから削除すると、マスタープロジェクトを安全に削除できます。

## [!DNL Project Masters]を非アクティブにする

プロジェクトのプロパティでマスタープロジェクトの状態を非アクティブに変更すると、非アクティブなマスタープロジェクトがマスタープロジェクトリストから消えます。

非アクティブなマスタープロジェクトを表示するには、上部バー（リスト表示切り替えの横）の「アクティブなフィルターを表示」ボタンを切り替えます。 非アクティブなプロジェクトを再度アクティブにするには、非アクティブなマスタープロジェクトを選択し、プロジェクトのプロパティを編集して、もう一度アクティブに設定します。

## [!DNL Project Masters]を理解する

![プロジェクトマスターのテクニカルビュー](assets/use-project-masters/project-masters-architecture.png)

[!DNL Project Masters] は、一連のAEMユーザーグループ（所有者、エディター、監視者）を定義し、派生プロジェクトがそれらの一元的に定義されたユーザーグループを参照および再利用できるようにすることで動作します。

これにより、AEMで必要なユーザーグループの総数が削減されます。 [!DNL Project Masters]より前に、各プロジェクトは3つのユーザーグループを作成し、それに伴うACEを付けて権限付与を実施し、100のプロジェクトは300個のユーザーグループを作成しました。 プロジェクトマスターは、共有メンバーシップがプロジェクト全体のビジネス要件に従っていると仮定して、任意の数のプロジェクトで同じ3つのグループを再利用できます。
