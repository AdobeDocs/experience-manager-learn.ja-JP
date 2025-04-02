---
title: 簡易有給休暇リクエストワークフロー
description: AEM ワークフローでアダプティブフォームパネルを表示／非表示にする
feature: Adaptive Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
discoiquuid: 1c4822e6-76ce-446b-98cb-408900d68b24
topic: Development
role: Developer
level: Experienced
exl-id: 9342bd2f-2ba9-42ee-9224-055649ac3c90
last-substantial-update: 2020-07-07T00:00:00Z
duration: 592
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '305'
ht-degree: 100%

---

# 簡易有給休暇リクエストワークフロー

この記事では、有休休暇のリクエストに使用する簡単なワークフローを見てみましょう。 ビジネス要件は次のとおりです。

* ユーザー A が、アダプティブフォームを入力して休暇をリクエストします。
* フォームが AEM 管理者ユーザーにルーティングされます（実際には、送信者の管理者にルーティングされます）
* 管理者がフォームを開きます。 管理者は、送信者が入力した情報を編集できません。
* 「承認者」セクションは、承認者に対して表示される必要があります（この場合は AEM 管理者ユーザー）。

上記の要件を達成するには、フォーム内で **initialstep** という非表示フィールドを使用します。そのデフォルト値は「はい」に設定されています。フォームが送信されると、ワークフローの最初のステップでは initialstep の値が「いいえ」に設定されます。フォームには、initialstep 値に基づいて適切なセクションの表示と非表示を切り替えるビジネスルールがあります。

**フォームをトリガー AEM ワークフローの設定**

>[!VIDEO](https://video.tv.adobe.com/v/28406?quality=12&learn=on)

**ワークフローの手順**

>[!VIDEO](https://video.tv.adobe.com/v/28407?quality=12&learn=on)

**送信者に表示される休暇リクエストフォーム**

![initialstep](assets/initialstep.gif)

**承認者に表示されるフォーム**

![approverview](assets/approversview.gif)

承認者に対する表示では、承認者は送信済みデータを編集できません。 承認者専用の新規セクションも追加されました。

お使いのシステム上でこのワークフローをテストするには、次の手順に従ってください。
* [DevelopingWithServiceUserBundle のダウンロードとデプロイ](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [SetValue カスタム OSGI バンドルをダウンロードしてデプロイします。](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
* [この記事に関連するアセットを AEM に読み込む](assets/helpxworkflow.zip)
* [休暇リクエストフォーム](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)を開きます。
* 詳細を入力して送信
* [インボックス](http://localhost:4502/mnt/overlay/cq/inbox/content/inbox.html)を開きます。新しいタスクが割り当てられているのが確認できます。 フォームを開きます。送信者のデータは読み取り専用で、新しい承認者セクションが表示されます。
*  [ワークフローモデル](http://localhost:4502/editor.html/conf/global/settings/workflow/models/helpxworkflow.html)を参照
* プロセスステップを参照します。これは、initialstep の値を「いいえ」に設定するステップです。
