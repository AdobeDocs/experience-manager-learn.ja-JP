---
title: AEMでプロジェクトマスターを使用
description: プロジェクトマスターは、AEMプロジェクトを使用したユーザーおよびチーム管理を大幅にシンプル化します。
version: 6.4, 6.5
feature: projects, users-and-groups
topics: administration, collaboration, performance
activity: use
audience: administrator, implementer, architect
doc-type: article
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '368'
ht-degree: 0%

---


# プロジェクトマスターの使用

プロジェクトマスターを使用すると、ユーザーとチームの管理が大幅にシンプルになり [!DNL AEM Projects]ます。

>[!VIDEO](https://video.tv.adobe.com/v/17740/?quality=9&learn=on)

管理者は、プロジェクトチームの一部として、を作成 **[!DNL Master Project]** し、ユーザーをロール/権限に割り当てることができるようになりました。 プロジェクトはマスタープロジェクトから作成でき、チームメンバーシップを自動的に継承します。 このオファーには、次のいくつかの利点があります。

* 複数のプロジェクトで既存のチームを再利用
* チームが手作業で再作成する必要がないため、プロジェクトの作成を高速化
* 一元的な場所からチームメンバーシップを管理し、チームに対するすべての更新はプロジェクトに自動的に継承されます
* パフォーマンスの問題を引き起こす可能性のある重複ACLの作成を回避

[!DNL Master Projects] は、 [!UICONTROL AEMプロジェクトの] マスター [!UICONTROL フォルダの下に作成できます]。 を作成す [!DNL Master Project] ると、新しいプロジェクトが作成されたときに、ウィザードで使用可能なテンプレートの横にオプションとして表示されます。

[!DNL Project Masters] URL（ローカルAEM作成者インスタンス）: [http://localhost:4502/projects.html/content/projects/masters](http://localhost:4502/projects.html/content/projects/masters)

## 削除 [!DNL Project Masters]

マスタープロジェクトを削除すると、使用できない派生プロジェクトになります。

マスタープロジェクトを削除する前に、すべての派生プロジェクトが完了し、AEMから削除されていることを確認します。 派生プロジェクトを削除する前に、必要なプロジェクトデータを必ず保存してください。 すべての派生プロジェクトをAEMから削除すると、マスタープロジェクトを安全に削除できます。

## 非アクティブ [!DNL Project Masters] にする

プロジェクトのプロパティでマスタープロジェクトの状態を非アクティブに変更すると、非アクティブなマスタープロジェクトはマスタープロジェクトリストから消えます。

非アクティブなマスタープロジェクトを表示するには、上部バー(リスト表示の切り替えの横)の「アクティブ」フィルターボタンを切り替えます。 非アクティブなプロジェクトを再度アクティブにするには、非アクティブなマスタープロジェクトを選択し、プロジェクトプロパティを編集し、もう一度アクティブに設定します。

## 理解する [!DNL Project Masters]

![プロジェクトマスターの技術表示](assets/use-project-masters/project-masters-architecture.png)

[!DNL Project Masters] aemユーザーグループのセット（所有者、編集者、監視者）を定義し、派生プロジェクトがそれらの一元的に定義されたユーザーグループを参照して再利用することを可能にする作業。

これにより、AEMで必要なユーザーグループの全体数が減ります。 以前は、各プロジェクト [!DNL Project Masters]で3つのユーザグループを作成し、それに付随するACEを使用して権限設定を実施していたため、100のプロジェクトに300のユーザグループが作成されていました。 プロジェクトマスターでは、共有メンバーシップがプロジェクト全体のビジネス要件に適合すると仮定して、任意の数のプロジェクトが同じ3つのグループを再利用できます。
