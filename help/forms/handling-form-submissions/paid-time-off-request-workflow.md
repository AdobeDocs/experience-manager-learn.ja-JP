---
title: 単純な有料オフリクエストワークフロー
description: AEMワークフローでのアダプティブフォームパネルの表示と非表示
uuid: 28ceb72b-24d9-488e-92af-7e85775dc682
feature: アダプティブフォーム
topics: workflow
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
discoiquuid: 1c4822e6-76ce-446b-98cb-408900d68b24
topic: 開発
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '335'
ht-degree: 0%

---


# 単純な有料オフリクエストワークフロー

この記事では、有料休暇のリクエストに使用する簡単なワークフローについて説明します。 ビジネス要件は次のとおりです。

* ユーザーAは、アダプティブフォームに入力することでタイムオフを要求します。
* フォームはAEM管理者ユーザーにルーティングされます（実際には、送信者のマネージャーにルーティングされます）
* 管理者がフォームを開きます。 管理者は、送信者が入力した情報を編集できません。
* 承認者セクションは、承認者(この場合はAEM管理者ユーザー)に表示されます。

上記の要件を達成するには、フォーム内で&#x200B;**initialstep**&#x200B;という非表示フィールドを使用し、そのデフォルト値を「はい」に設定します。フォームが送信されると、ワークフローの最初のステップで「いいえ」に設定されます。 フォームには、初期手順の値に基づいて適切なセクションの表示と非表示を切り替えるビジネスルールが含まれています。

**フォームをトリガーAEMワークフローに設定**

>[!VIDEO](https://video.tv.adobe.com/v/28406?quality=9&learn=on)

**ワークフローの手順**

>[!VIDEO](https://video.tv.adobe.com/v/28407?quality=9&learn=on)

**送信者の「オフリクエスト時間」フォームの表示**

![initialstep](assets/initialstep.gif)

**フォームの承認者ビュー**

![approveview](assets/approversview.gif)

承認者ビューでは、承認者は送信されたデータを編集できません。 承認者専用の新しい節も追加されました。

お使いのシステムでこのワークフローをテストするには、次の手順に従います。
* [DevelopingWithServiceUserBundleのダウンロードとデプロイ](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [SetValueカスタムOSGIバンドルのダウンロードとデプロイ](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
* [この記事に関連するアセットをAEMに読み込む](assets/helpxworkflow.zip)
* [Time Off Requestフォーム](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)を開きます。
* 詳細を入力し、
* [インボックス](http://localhost:4502/mnt/overlay/cq/inbox/content/inbox.html)を開きます。 新しいタスクが割り当てられているはずです。 フォームを開きます。 送信者のデータは読み取り専用で、新しい承認者セクションが表示されます。
* [ワークフローモデル](http://localhost:4502/editor.html/conf/global/settings/workflow/models/helpxworkflow.html)を調べます。
* プロセスステップを参照します。 これは、initialstepの値をNoに設定する手順です。
