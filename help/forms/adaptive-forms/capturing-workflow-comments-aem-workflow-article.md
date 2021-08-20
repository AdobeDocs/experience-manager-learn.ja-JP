---
title: アダプティブワークフローでのワークフローコメントの取り込みForms Workflow
description: AEM Workflowでのワークフローコメントの取り込み
feature: ワークフロー
version: 6.3,6.4
topic: 開発
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '382'
ht-degree: 0%

---


# アダプティブワークフローでのワークフローコメントの取り込みForms Workflow{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[AEM Forms 6.4にのみ適用されます。AEM Forms 6.5では、変数機能を使用してこの使用例を実現してください。]

一般的なリクエストとしては、タスクのレビュー担当者が入力したコメントをEメールに含める機能があります。 AEM Forms 6.4では、ユーザーが入力したコメントを取り込んで電子メールに含めるメカニズムは標準では用意されていません。

この要件を満たすために、コメントをキャプチャし、これらのコメントをワークフローメタデータプロパティとして保存するために使用できる、サンプルのOSGiバンドルが用意されています。

次のスクリーンショットは、[AEM Workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)のプロセスステップを使用してコメントを取り込み、メタデータプロパティとして保存する方法を示しています。 「Capture Workflow Comments」は、プロセスステップで使用する必要があるJavaクラスの名前です。 コメントを保持するメタデータプロパティ名を渡す必要があります。 以下のスクリーンショットでは、managerCommentsは、コメントを保存するメタデータプロパティです。

![workflowcomments1](assets/workflowcomments1.gif)

ご使用のシステムでこの機能をテストするには、次の手順に従います。
* [ワークフローのプロセスステップで、「ワークフローコメントの取り込み」を使用するように設定されていることを確認します。](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [Developingwithserviceuserバンドルをデプロイします。](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [SetValueバンドルをデプロイします](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。このバンドルには、コメントを取り込み、メタデータプロパティとして保存するサンプルコードが含まれています

* [この記事に関連するアセットをダウンロードして、ファイルシステムに解凍します。アセットに](assets/capturecomments.zip) は、ワークフローモデルとサンプルのアダプティブフォームが含まれています。

* パッケージマネージャーを使用して2つのzipファイルをAEMに読み込みます。

* [このURLを参照してフォームをプレビューする](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* フォームフィールドに入力し、フォームを送信します

* [AEMインボックスを確認する](http://localhost:4502/aem/inbox)

* インボックスからタスクを開き、フォームを送信します。 プロンプトが表示されたら、コメントを入力してください。

コメントは、crxのmanagerCommentsというメタデータプロパティに保存されます。 コメントが管理者としてcrxにログインしているかどうかを確認する。 ワークフローインスタンスは、次のパスに保存されます

/var/workflow/instances/server0

適切なワークフローインスタンスを選択し、メタデータノードでmanagerCommentsプロパティを確認します。

