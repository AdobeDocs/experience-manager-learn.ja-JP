---
title: AEM でのプロジェクトマスターの使用方法
description: プロジェクトマスターは、AEM プロジェクトを使用したユーザーおよびチームの管理を大幅に簡素化します。
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Content Management, Collaboration
feature: Projects
level: Intermediate
role: User
jira: KT-256
thumbnail: 17740.jpg
doc-type: Feature Video
exl-id: 78ff62ad-1017-4a02-80e9-81228f9e01eb
duration: 260
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 100%

---

# プロジェクトマスターの使用

プロジェクトマスターは、 [!DNL AEM Projects]を使用したユーザーおよびチームの管理を大幅に簡素化します。

>[!VIDEO](https://video.tv.adobe.com/v/17740?quality=12&learn=on)

管理者は **[!DNL Master Project]** を作成し、 プロジェクトチームの一部としてユーザーに役割／権限を割り当てます。 プロジェクトはマスタープロジェクトから作成され、自動的にチームメンバーシップを継承します。 これには、次のようないくつかの利点があります。

* 複数のプロジェクトで既存のチームを再利用できます
* チームを手動で再作成する必要がないので、チームとしてのプロジェクト作成を加速化できます
* 一元的な場所からチームメンバーシップを管理し、チームに対する更新がプロジェクトに自動的に継承されます
* パフォーマンスの問題を引き起こす可能性のある重複 ACL の作成を回避できます

[!DNL Master Projects] は、 [!UICONTROL AEM プロジェクト]にある[!UICONTROL マスター]フォルダーで作成できます。作成されたマスタープロジェクトは、新しいプロジェクトを作成する際のウィザードで、使用可能なテンプレートと共にオプションとして表示されます。

[!DNL Project Masters] URL（ローカル AEM オーサーインスタンス）：[http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## [!DNL Project Masters] の削除

マスタープロジェクトを削除すると、派生プロジェクトが使用できなくなります。

マスタープロジェクトを削除する前に、すべての派生プロジェクトが完了し、AEM から削除されていることを確認します。 派生プロジェクトを削除する前に、必要なプロジェクトデータを必ず保存してください。 すべての派生プロジェクトを AEM から削除すると、マスタープロジェクトを安全に削除できます。

## 非アクティブとして [!DNL Project Masters] をマーク

プロジェクトのプロパティでマスタープロジェクトのステータスを非アクティブに変更すると、非アクティブなマスタープロジェクトがマスタープロジェクトリストに表示されなくなります。

非アクティブなマスタープロジェクトを表示するには、上部バー（リスト表示切り替えの横）にある「アクティブを表示」フィルターボタンを切り替えます。 非アクティブなプロジェクトを再度アクティブにするには、非アクティブなマスタープロジェクトを選択し、プロジェクトのプロパティを編集して再度アクティブに設定します。

## [!DNL Project Masters] について

![プロジェクトマスターのテクニカルビュー](assets/use-project-masters/project-masters-architecture.png)

[!DNL Project Masters] は、AEM ユーザーグループのセット（所有者、編集者、監視者）を定義し、派生プロジェクトがそれらの一元的に定義されたユーザーグループを参照および再利用できるようにします。

これにより、AEM で必要なユーザーグループの全体的な数が削減されます。 [!DNL Project Masters] 以前、各プロジェクトでは権限設定を実施するために、付随する ACE を持つ 3 つのユーザーグループが作成されていました。つまり、100 個のプロジェクトでは 300 のユーザーグループが生成されていました。 プロジェクトマスターを使用すると、共有メンバーシップがプロジェクト全体のビジネス要件に従っていると想定して、同じ 3 つのグループを任意の数のプロジェクトで再利用できます。
