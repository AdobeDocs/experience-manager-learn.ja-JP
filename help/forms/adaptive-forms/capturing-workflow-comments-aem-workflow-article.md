---
title: アダプティブForms Workflowでのワークフローコメントのキャプチャ
description: AEM Workflow でのワークフローコメントのキャプチャ
feature: Workflow
version: 6.4
topic: Development
role: Developer
level: Experienced
exl-id: 5c250bbb-bac6-427d-8aca-1fbb1229e02c
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '380'
ht-degree: 0%

---

# アダプティブForms Workflowでのワークフローコメントのキャプチャ{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[AEM Forms 6.4 にのみ適用されます。AEM Forms 6.5 では、変数機能を使用して、この使用例を達成してください。]

一般的なリクエストとしては、タスクのレビュー担当者が入力したコメントを E メールに含める機能があります。 AEM Forms 6.4 では、ユーザーが入力したコメントを取り込み、これらのコメントを電子メールに含めるメカニズムは標準では用意されていません。

この要件を満たすために、コメントをキャプチャし、これらのコメントをワークフローメタデータプロパティとして保存するために使用できる、サンプルの OSGi バンドルが提供されます。

次のスクリーンショットは、 [AEM Workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html) コメントをキャプチャし、メタデータプロパティとして保存する。 「Capture Workflow Comments」は、プロセスステップで使用する必要がある Java クラスの名前です。 コメントを保持するメタデータプロパティ名を渡す必要があります。 以下のスクリーンショットでは、managerComments は、コメントを保存するメタデータプロパティです。

![workflowcomments1](assets/workflowcomments1.gif)

お使いのシステムでこの機能をテストするには、次の手順に従ってください。
* [ワークフローのプロセスステップで、Capture Workflow Comments を使用するようにが設定されていることを確認します。](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [Developingwithserviceuser バンドルをデプロイします。](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [SetValue バンドルをデプロイします。](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). このバンドルには、コメントをキャプチャし、メタデータプロパティとして保存するサンプルコードが含まれています

* [この記事に関連するアセットをファイルシステムにダウンロードして解凍します](assets/capturecomments.zip) アセットには、ワークフローモデルとサンプルのアダプティブフォームが含まれています。

* パッケージマネージャーを使用して 2 つの zip ファイルをAEMに読み込みます。

* [この URL を参照してフォームをプレビュー](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* フォームフィールドに入力し、フォームを送信します

* [AEMインボックスを確認](http://localhost:4502/aem/inbox)

* インボックスからタスクを開き、フォームを送信します。 プロンプトが表示されたら、コメントを入力してください。

コメントは、crx の managerComments というメタデータプロパティに保存されます。 コメントが crx に管理者としてログインしていないかを確認するには、次の手順に従います。 ワークフローインスタンスは次のパスに保存されます

/var/workflow/instances/server0

適切なワークフローインスタンスを選択し、metadata ノードで managerComments プロパティを確認します。
