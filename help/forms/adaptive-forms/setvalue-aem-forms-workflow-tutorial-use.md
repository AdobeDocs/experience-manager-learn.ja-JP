---
title: AEM Formsワークフローでの setvalue の使用
description: AEM Forms OSGi で送信されたアダプティブFormsのデータの要素の値を設定する
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: 3919efee-6998-48e8-85d7-91b6943d23f9
last-substantial-update: 2020-01-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 2%

---

# AEM Formsワークフローでの setvalue の使用

AEM Forms OSGi ワークフローで、アダプティブFormsで XML 要素の値を送信したデータを設定します。

![SetValue](assets/setvalue.png)

LiveCycleは、XML 要素の値を設定できる set value コンポーネントを持っています。

この値に基づいて、フォームに XML が入力された場合に、フォームの特定のフィールドやパネルの表示/非表示を切り替えることができます。

AEM Forms OSGi - XML で値を設定するためのカスタム OSGi バンドルを作成する必要があります。 バンドルは、このチュートリアルの一部として提供されます。
ここでは、AEMワークフローのプロセスステップを使用します。 このプロセスステップに「Set Value of Element in XML」OSGi バンドルを関連付けます。
設定値バンドルに 2 つの引数を渡す必要があります。 最初の引数は、値を設定する必要がある XML 要素の XPath です。 2 番目の引数は、設定する必要がある値です。
例えば、上のスクリーンショットでは、 intialstep 要素の値を「N」に設定しています。
この値に基づいて、アダプティブForms内の特定のパネルが非表示または表示されます。
この例では、単純なタイムオフリクエストフォームを使用します。 このフォームの開始者は、名前と休日を入力します。 このフォームは送信時に、レビュー用に「管理者」に送信されます。 管理者がフォームを開くと、最初のパネルのフィールドは無効になります。 これは、XML の最初の step 要素の値が「N」に設定されているからです。

最初のステップフィールドの値に基づいて、2 番目のパネルに表示されます。このパネルでは、「管理者」がリクエストを承認または却下できます

ルールエディターを使用して、「Time Off Requested by」フィールドに対して設定されたルールを確認してください。

ローカルシステムにアセットをデプロイするには、次の手順に従います。

* [Developingwithserviceuser バンドルをデプロイします。](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [サンプルバンドルのデプロイ](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). これは、送信された xml データ内の要素の値を設定できるカスタム OSGi バンドルです

* [zip ファイルの内容をダウンロードして抽出します。](assets/setvalueassets.zip)
* ブラウザーで次の場所を指定します。 [パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)
* setValueWorkflow.zip を読み込んでインストールします。 これには、サンプルのワークフローモデルが含まれています。
* ブラウザーで次の場所を指定します。 [Formsとドキュメント](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 「作成」をクリックします。 |ファイルのアップロード
* TimeOfRequestForm.zip をアップロードします。
* を開きます。 [TimeOffRequestform](http://localhost:4502/content/dam/formsanddocuments/timeoffapplication/jcr:content?wcmmode=disabled)
* 3 つの必須フィールドに入力し、
* AEMに「管理者」としてログイン（まだログインしていない場合）
* に移動します。 [&quot;AEM Inbox&quot;](http://localhost:4502/aem/inbox)
* 「レビュータイムオフリクエスト」フォームを開きます。
* 最初のパネルのフィールドは無効になっています。 これは、レビュー担当者がフォームを開いているためです。 また、要求の承認または却下のパネルが表示されます

>[!NOTE]
>
>次のロガーを有効にすることで、デバッグログを有効にできます：
>com.aemforms.setvalue.core.SetValueinXml
>ブラウザーでhttp://localhost:4502/system/console/slinglogを指し示して、

>[!NOTE]
>
>アダプティブフォームの送信オプションのデータファイルパスが「Data.xml」に設定されていることを確認します。 これは、プロセスステップがペイロードフォルダーの下で Data.xml と呼ばれるファイルを探すためです
