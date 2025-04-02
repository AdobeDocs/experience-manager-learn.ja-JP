---
title: アダプティブフォームワークフローでのワークフローコメントの取得
description: AEM ワークフローでのワークフローコメントの取得
feature: Workflow
version: Experience Manager 6.4
topic: Development
role: Developer
level: Experienced
exl-id: 5c250bbb-bac6-427d-8aca-1fbb1229e02c
last-substantial-update: 2020-10-10T00:00:00Z
duration: 73
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '350'
ht-degree: 100%

---

# アダプティブフォームワークフローでのワークフローコメントの取得{#capturing-workflow-comments-in-adaptive-forms-workflow}

>[AEM Forms 6.4 にのみ適用されます。AEM Forms 6.5 では、変数機能を使用してこのユースケースを実現してください。]

タスクレビュアーに入力されたコメントをメールに含める機能がよくリクエストされます。AEM Forms 6.4 では、ユーザーが入力したコメントを取得してメールに含めるメカニズムは標準では用意されていません。

この要件を満たすために、コメントを取得してワークフローメタデータプロパティとして保存するのに使用できるサンプルの OSGi バンドルが用意されています。

次のスクリーンショットは、[AEM ワークフロー](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)でプロセスステップを使用してコメントを取得し、それをメタデータプロパティとして保存する方法を示しています。 このプロセスステップで使用する必要がある Java クラスの名前は、「Capture Workflow Comments」です。 コメントを保持するメタデータプロパティ名を渡す必要があります。 以下のスクリーンショットでは、managerComments が、コメントを保存するメタデータプロパティです。

![workflowcomments1](assets/workflowcomments1.gif)

お使いのシステムでこの機能をテストするには、次の手順に従ってください。
* [Capture Workflow Comments を使用するようにワークフロー内のプロセスステップが設定されていることを確認します。](http://localhost:4502/editor.html/conf/global/settings/workflow/models/CaptureComments.html)

* [Developingwithserviceuser バンドルをデプロイします。](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [SetValue バンドルをデプロイします](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。このバンドルには、コメントを取得してメタデータプロパティとして保存するサンプルコードが含まれています。

* [この記事に関連するアセットをファイルシステムにダウンロードして解凍します](assets/capturecomments.zip)。このアセットには、ワークフローモデルとサンプルのアダプティブフォームが含まれています。

* パッケージマネージャーを使用して 2 つの zip ファイルを AEM に読み込みます。

* [この URL を参照してフォームをプレビューします](http://localhost:4502/content/dam/formsanddocuments/capturecomments/jcr:content?wcmmode=disabled)。

* フォームフィールドに入力し、フォームを送信します。

* [AEM インボックスを確認します](http://localhost:4502/aem/inbox)。

* インボックスからタスクを開き、フォームを送信します。 プロンプトが表示されたら、コメントを入力してください。

コメントは、AEM リポジトリ内の `managerComments` という名前のメタデータプロパティに保存されます。 コメントを確認するには、crx に管理者としてログインします。 ワークフローインスタンスは、次のパスに保存されています。

`/var/workflow/instances/server0`

適切なワークフローインスタンスを選択し、metadata ノードで managerComments プロパティを確認します。
