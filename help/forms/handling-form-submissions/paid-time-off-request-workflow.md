---
title: 簡易有料オフリクエストワークフロー
description: AEM Workflow でアダプティブフォームパネルを表示/非表示にする
uuid: 28ceb72b-24d9-488e-92af-7e85775dc682
feature: Adaptive Forms
topics: workflow
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
discoiquuid: 1c4822e6-76ce-446b-98cb-408900d68b24
topic: Development
role: Developer
level: Experienced
exl-id: 9342bd2f-2ba9-42ee-9224-055649ac3c90
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 0%

---

# 簡易有料オフリクエストワークフロー

この記事では、有料滞在時間のリクエストに使用する簡単なワークフローを見てみましょう。 ビジネス要件は次のとおりです。

* ユーザー A は、アダプティブフォームへの入力によってタイムオフをリクエストします。
* フォームはAEM管理者ユーザーにルーティングされます（実際には、送信者の管理者にルーティングされます）
* 管理者がフォームを開きます。 管理者は、送信者が入力した情報を編集できない。
* 承認者セクションは、承認者に対して表示される必要があります ( この場合はAEM管理者ユーザー )。

上記の要件を達成するには、という非表示フィールドを使用します。 **initialstep** フォーム内で、そのデフォルト値は「はい」に設定されます。フォームが送信されると、ワークフローの最初のステップでは「initialstep」の値が「いいえ」に設定されます。 フォームには、最初の step 値に基づいて適切なセクションの表示と非表示を切り替えるビジネスルールがあります。

**フォームをトリガーAEMワークフローに設定**

>[!VIDEO](https://video.tv.adobe.com/v/28406?quality=9&learn=on)

**ワークフローの手順**

>[!VIDEO](https://video.tv.adobe.com/v/28407?quality=9&learn=on)

**送信者の「タイムオフリクエスト」フォームに対する表示**

![initialstep](assets/initialstep.gif)

**フォームの承認者ビュー**

![承認](assets/approversview.gif)

承認者表示では、承認者は送信済みデータを編集できません。 「承認者」専用の新しい節も追加されました。

お使いのシステム上でこのワークフローをテストするには、次の手順に従ってください。
* [DevelopingWithServiceUserBundle のダウンロードとデプロイ](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [SetValue カスタム OSGI バンドルをダウンロードしてデプロイします。](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
* [この記事に関連するアセットをAEMに読み込む](assets/helpxworkflow.zip)
* を開きます。 [タイムオフリクエストフォーム](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* 詳細を入力し、
* を開きます。 [受信トレイ](http://localhost:4502/mnt/overlay/cq/inbox/content/inbox.html). 新しいタスクが割り当てられているのが確認できます。 フォームを開きます。 送信者のデータは読み取り専用で、新しい承認者セクションが表示されます。
* 関連トピック [ワークフローモデル](http://localhost:4502/editor.html/conf/global/settings/workflow/models/helpxworkflow.html)
* プロセスステップを参照します。 これは、initialstep の値を No に設定するステップです。
