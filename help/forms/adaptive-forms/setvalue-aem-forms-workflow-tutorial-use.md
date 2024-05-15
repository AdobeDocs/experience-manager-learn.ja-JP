---
title: AEM Forms ワークフローでの SetValue の使用
description: AEM Forms OSGi で送信されたアダプティブフォームのデータの要素の値を設定する
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: 3919efee-6998-48e8-85d7-91b6943d23f9
last-substantial-update: 2020-01-09T00:00:00Z
duration: 105
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '503'
ht-degree: 100%

---

# AEM Forms ワークフローでの SetValue の使用

AEM Forms OSGi ワークフローで、アダプティブフォームで XML 要素の値を送信したデータを設定します。

![SetValue](assets/setvalue.png)

LiveCycle には、XML 要素の値を設定できる set value コンポーネントが備わっていました。

この値に基づいて、フォームに XML が入力された場合に、フォームの特定のフィールドやパネルの表示／非表示を切り替えることができます。

AEM Forms OSGi - XML で値を設定するために、カスタム OSGi バンドルを作成する必要があります。バンドルは、このチュートリアルの一部として提供されます。
ここでは、AEM ワークフローのプロセスステップを使用します。このプロセスステップに「Set Value of Element in XML」OSGi バンドルを関連付けます。
設定値のバンドルに 2 つの引数を渡す必要があります。最初の引数は、値を設定する必要がある XML 要素の XPath です。2 番目の引数は、設定する必要がある値です。
例えば、上のスクリーンショットでは、 intialstep 要素の値を「N」に設定しています。
この値に基づいて、アダプティブフォーム内の特定のパネルが非表示または表示されます。
この例では、シンプルなリクエストフォームを作成します。このフォームのイニシエーターは、名前と休日を入力します。このフォームは送信時に、レビュー用に「管理者」に送信されます。管理者がフォームを開くと、最初のパネルのフィールドは無効になります。これは、XML の最初の step 要素の値が「N」に設定されているからです。

最初のステップフィールドの値に基づいて、2 番目のパネルに表示されます。このパネルでは、「管理者」がリクエストを承認または却下できます。

ルールエディターを使用して、「Time Off Requested by」フィールドに対して設定されたルールを確認してください。

ローカルシステムにアセットをデプロイするには、次の手順に従います。

* [Developingwithserviceuser バンドルをデプロイします。](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [サンプルバンドルをデプロイする](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。これは、送信された xml データ内の要素の値を設定できるカスタム OSGi バンドルです。

* [ZIP ファイルの中身をダウンロードして解凍する](assets/setvalueassets.zip)
* ブラウザーで[パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を指定します
* setValueWorkflow.zip を読み込んでインストールする。これには、サンプルのワークフローモデルが含まれています。
* ブラウザーで [Forms とドキュメント](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)を指定します
* 「作成 | ファイルのアップロード」をクリックする
* TimeOfRequestForm.zip をアップロードする
* [TimeOffRequestform](http://localhost:4502/content/dam/formsanddocuments/timeoffapplication/jcr:content?wcmmode=disabled) を開く
* 3 つの必須フィールドに入力する
* AEMに「管理者」としてログインする（まだログインしていない場合）
* 「[AEM インボックス](http://localhost:4502/aem/inbox)」に移動する
* 「休暇リクエストのレビュー」フォームを開く
* 最初のパネルのフィールドは無効になっています。これは、レビュー担当者がフォームを開いているためです。また、リクエストの承認または却下のパネルが表示されます。

>[!NOTE]
>
>ブラウザーで http://localhost:4502/system/console/slinglog を参照し、
>com.aemforms.setvalue.core.SetValueXml
>のロガーを有効にすることで、デバッグログを有効にできます

>[!NOTE]
>
>アダプティブフォームの送信オプションのデータファイルパスが「Data.xml」に設定されていることを確認します。これは、プロセスステップがペイロードフォルダーの下で Data.xml と呼ばれるファイルを探すためです。
