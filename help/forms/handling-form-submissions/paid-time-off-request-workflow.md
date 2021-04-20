---
title: 単純な有料休暇リクエストのワークフロー
description: AEMワークフローでのアダプティブフォームパネルの表示/非表示
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
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '335'
ht-degree: 0%

---


# 単純な有料休暇リクエストのワークフロー

この記事では、有料休暇のリクエストに使用する簡単なワークフローを見ていきます。 ビジネス要件は次のとおりです。

* ユーザーAのリクエストは、アダプティブフォームへの入力によってタイムオフになります。
* フォームはAEM管理者ユーザーにルーティングされます（実際には、送信者の管理者にルーティングされます）
* 管理者がフォームを開きます。 管理者は、送信者が入力した情報を編集できません。
* 「承認者」セクションは、承認者に対して表示される必要があります(この場合は、AEM管理者ユーザーです)。

上記の要件を満たすために、フォームで&#x200B;**initialstep**&#x200B;という名前の非表示フィールドを使用し、そのデフォルト値をYesに設定します。フォームが送信されると、ワークフローの最初の手順でinitialstepの値がNoに設定されます。 フォームには、initialstep値に基づいて適切なセクションの表示と非表示を切り替えるビジネスルールがあります。

**フォームからトリガーAEMワークフローの設定**

>[!VIDEO](https://video.tv.adobe.com/v/28406?quality=9&learn=on)

**ワークフローのチュートリアル**

>[!VIDEO](https://video.tv.adobe.com/v/28407?quality=9&learn=on)

**提出者の「休暇申請」フォームの表示**

![initialstep](assets/initialstep.gif)

**フォームの承認者表示**

![approverview](assets/approversview.gif)

承認者の表示では、承認者は送信されたデータを編集できません。 また、承認者専用の新しいセクションもあります。

お使いのシステムでこのワークフローをテストするには、次の手順に従ってください。
* [DevelopingWithServiceUserBundleのダウンロードと展開](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [SetValueカスタムOSGIバンドルのダウンロードと展開](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
* [この記事に関連するアセットをAEMに読み込む](assets/helpxworkflow.zip)
* [タイムオフ要求フォーム](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)を開く
* 詳細を入力し、
* [インボックス](http://localhost:4502/mnt/overlay/cq/inbox/content/inbox.html)を開きます。 新しいタスクが割り当てられていることが確認できます。 フォームを開きます。 送信者のデータは読み取り専用で、新しい承認者セクションが表示されている必要があります。
* [ワークフローモデル](http://localhost:4502/editor.html/conf/global/settings/workflow/models/helpxworkflow.html)を調べます。
* プロセス手順を調べます。 initialstepの値をNoに設定する手順です。
