---
title: アダプティブForms Workflowでのワークフローコメントのキャプチャ
seo-title: アダプティブForms Workflowでのワークフローコメントのキャプチャ
description: AEMワークフローでのワークフローコメントの取り込み
seo-description: AEMワークフローでのワークフローコメントの取り込み
uuid: df41fc6f-9abf-47b4-a014-b3b9fb58b6f7
feature: ワークフロー
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4
discoiquuid: d5e40650-3b1f-4875-91b2-e22d932b5e7c
topic: 開発
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '395'
ht-degree: 0%

---


# アダプティブForms Workflowでのワークフローコメントのキャプチャ{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[AEM Forms6.4にのみ適用されます。AEM Forms6.5では、変数機能を使用してこの使用例を達成してください]

一般的な要求は、タスクのレビュー担当者が入力したコメントを電子メールに含めることです。 AEM Forms6.4では、ユーザーが入力したコメントを取り込み、これらのコメントを電子メールに含めるメカニズムはあらかじめ用意されていません。

この要件を満たすために、コメントを取り込み、これらのコメントをワークフローメタデータプロパティとして保存するために使用できるサンプルのOSGiバンドルが提供されます。

次のスクリーンショットは、[AEMワークフロー](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)のプロセス手順を使用してコメントを取り込み、メタデータプロパティとして保存する方法を示しています。 「Capture Workflow Comments」は、プロセスステップで使用する必要があるJavaクラスの名前です。 コメントを保持するメタデータプロパティ名を渡す必要があります。 下のスクリーンショットでは、managerCommentsはコメントを保存するメタデータプロパティです。

![workflowcomments1](assets/workflowcomments1.gif)

お使いのシステムでこの機能をテストするには、次の手順に従ってください。
* [ワークフローのプロセスステップで、「Capture Workflow Comments」を使用するように設定されていることを確認します。](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [Developingwithserviceuserバンドルのデプロイ](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [SetValueバンドルをデプロイします](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。このバンドルには、コメントを取り込み、メタデータプロパティとして保存するためのサンプルコードが含まれています

* [この記事に関連するアセットをダウンロードし、ファイル](assets/capturecomments.zip) システムに解凍します。アセットには、ワークフローモデルとサンプルのアダプティブフォームが含まれています。

* パッケージマネージャーを使用して2つのzipファイルをAEMに読み込みます

* [このURLを参照してフォームをプレビューする](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)

* フォームフィールドに入力し、フォームを送信します

* [AEM受信トレイを確認する](http://localhost:4502/aem/inbox)

* インボックスからタスクを開き、フォームを送信します。 指示に従ってコメントを入力してください。

コメントは、crxのmanagerCommentsというメタデータプロパティに保存されます。 crxに対するコメントのログインを管理者として確認する場合。 ワークフローインスタンスは次のパスに保存されます

/var/workflow/instances/server0

適切なワークフローインスタンスを選択し、メタデータノード内のproperty managerCommentsを確認します。

