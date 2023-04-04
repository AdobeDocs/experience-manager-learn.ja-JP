---
title: AEMでのプロジェクトマスターの使用方法
description: プロジェクトマスターは、AEM Projects を使用したユーザーおよびチームの管理を大幅に簡素化します。
version: 6.4, 6.5, Cloud Service
topic: Content Management, Collaboration
feature: Projects
level: Intermediate
role: User
kt: 256
thumbnail: 17740.jpg
exl-id: 78ff62ad-1017-4a02-80e9-81228f9e01eb
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '368'
ht-degree: 0%

---

# プロジェクトマスターを使用

プロジェクトマスターは、 [!DNL AEM Projects].

>[!VIDEO](https://video.tv.adobe.com/v/17740?quality=12&learn=on)

管理者は、 **[!DNL Master Project]** ユーザーをプロジェクトチームの一部として役割/権限に割り当てます。 プロジェクトは、チームプロジェクトからマスターを作成し、チームメンバーシップを自動的に継承できます。 これには、次のようないくつかの利点があります。

* 複数のプロジェクトで既存のチームを再利用
* チームを手動で再作成する必要がないので、プロジェクト作成を高速化します
* 一元的な場所からチームメンバーシップを管理し、チームに対する更新はプロジェクトに自動的に継承されます
* パフォーマンスの問題を引き起こす可能性のある重複 ACL の作成を回避

[!DNL Master Projects] は、 [!UICONTROL マスター] フォルダーの下 [!UICONTROL AEM Projects]. 作成されたマスタープロジェクトは、新しいプロジェクトの作成時にウィザードで使用可能なテンプレートと共にオプションとして表示されます。

[!DNL Project Masters] URL （ローカル AEM オーサーインスタンス）: [http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## 削除 [!DNL Project Masters]

マスタープロジェクトを削除すると、派生プロジェクトが使用できなくなります。

マスタープロジェクトを削除する前に、すべての派生プロジェクトが完了し、AEMから削除されていることを確認します。 派生プロジェクトを削除する前に、必要なプロジェクトデータを必ず保存してください。 すべての派生プロジェクトをAEMから削除すると、マスタープロジェクトを安全に削除できます。

## トンボ [!DNL Project Masters] 非アクティブ

プロジェクトのプロパティでマスタープロジェクトのステータスを非アクティブに変更すると、非アクティブなマスタープロジェクトがマスタープロジェクトリストに表示されなくなります。

非アクティブなマスタープロジェクトを表示するには、上部バー（リスト表示切り替えの横）にある「アクティブなフィルターを表示」ボタンを切り替えます。 非アクティブなプロジェクトを再度アクティブにするには、非アクティブなマスタープロジェクトを選択し、プロジェクトのプロパティを編集し、再度アクティブに設定します。

## 理解 [!DNL Project Masters]

![プロジェクトマスターのテクニカルビュー](assets/use-project-masters/project-masters-architecture.png)

[!DNL Project Masters] は、AEMユーザーグループのセット（所有者、編集者、監視者）を定義し、派生プロジェクトがそれらの一元的に定義されたユーザーグループを参照および再利用できるようにすることで機能します。

これにより、AEMで必要なユーザーグループの総数が削減されます。 前 [!DNL Project Masters]各プロジェクトは、権限設定を実施するために、付随する ACE を持つ 3 つのユーザーグループを作成したので、100 個のプロジェクトに 300 個のユーザーグループが作成されました。 プロジェクトマスターは、共有メンバーシップがプロジェクト全体のビジネス要件に従っていると仮定して、任意の数のプロジェクトが同じ 3 つのグループを再利用できます。
