---
title: AEM Forms Workflow での Json データ要素の値の設定
description: アダプティブフォームが AEM ワークフロー内のさまざまなユーザーにルーティングされるので、フォームのレビュー担当者に応じて、特定のフィールドまたはパネルを非表示にしたり無効にしたりする必要があります。これらのユースケースを満たすために、通常は非表示フィールドの値を設定します。 この非表示フィールドの値に基づいて、ビジネスルールを作成して、適切なパネルやフィールドを非表示／無効にすることができます。
feature: Adaptive Forms
version: Experience Manager 6.4
topic: Development
role: Developer
level: Experienced
exl-id: fbe6d341-7941-46f5-bcd8-58b99396d351
last-substantial-update: 2021-06-09T00:00:00Z
duration: 126
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '656'
ht-degree: 100%

---

# AEM Forms Workflow での JSON データ要素の値の設定 {#setting-value-of-json-data-element-in-aem-forms-workflow}

アダプティブフォームが AEM ワークフロー内のさまざまなユーザーにルーティングされるので、フォームのレビュー担当者に応じて、特定のフィールドまたはパネルを非表示にしたり無効にしたりする必要があります。これらのユースケースを満たすために、通常は非表示フィールドの値を設定します。 この非表示フィールドの値に基づいて、ビジネスルールを作成して、適切なパネルやフィールドを非表示／無効にすることができます。

![JSON データ内要素の値の設定](assets/capture-3.gif)

AEM Forms OSGi で - JSON データ要素の値を設定するカスタム OSGi バンドルを作成する必要があります。 バンドルは、このチュートリアルの一部として提供されます。

ここでは、AEM ワークフローのプロセスステップを使用します。「JSON 内要素の値を設定」OSGi バンドルをこのプロセスステップに関連付けます。

設定値のバンドルに 2 つの引数を渡す必要があります。最初の引数は、値を設定する必要がある要素へのパスです。 2 番目の引数は、設定する必要がある値です。

例えば、上のスクリーンショットでは、 intialStep 要素の値を「N」に設定しています。

afData.afUnboundData.data.initialStep,N

この例では、シンプルなリクエストフォームを作成します。このフォームのイニシエーターは、名前と休日を入力します。このフォームは送信時に、レビュー用に「管理者」に送信されます。 管理者がフォームを開くと、最初のパネルのフィールドが無効になります。 これは、JSON データの最初のステップ要素の値が N に設定されているからです。

最初のステップフィールドの値に基づいて、「管理者」がリクエストを承認または却下できる承認者パネルを表示します。

「最初のステップ」に対するルールセットを確認してください。「initialStep」フィールドの値に基づいて、フォームデータモデルを使用してユーザーの詳細を取得し、適切なフィールドに値を入力して、適切なパネルを非表示／無効にします。

ローカルシステムにアセットをデプロイするには：

* [DevelopingWithServiceUserBundle のダウンロードとデプロイ](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [setvalue バンドルのダウンロードとデプロイ](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。 これは、カスタム OSGi バンドルで、送信された JSON データ内の要素の値を設定できます。

* [ZIP ファイルの中身をダウンロードして解凍する](assets/set-value-jsondata.zip)
   * ブラウザーで[パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を指定します
      * SetValueOfElementInJSONDataWorkflow.zip を読み込んでインストールします。このパッケージには、サンプルのワークフローモデルとフォームデータモデルが関連付けられています。

* ブラウザーで [Forms とドキュメント](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)を指定します
* 「作成 | ファイルのアップロード」をクリックする
* TimeOffRequestForm.zip ファイルのアップロード
  **このフォームは AEM Forms 6.4 を使用して作成されました。AEM Forms 6.4 以降を使用していることを確認してください**
* [フォーム](http://localhost:4502/content/dam/formsanddocuments/timeoffrequest/jcr:content?wcmmode=disabled)を開きます
* 開始日と終了日を入力し、フォームを送信します。
* [「インボックス」](http://localhost:4502/aem/inbox)に移動します
* タスクに関連付けられているフォームを開きます。
* 最初のパネルのフィールドは無効になっています。
* リクエストを承認または却下するためのパネルが表示されます。

>[!NOTE]
>
>ユーザープロファイルを使用してアダプティブフォームに事前入力するので、管理者の[ユーザープロファイル情報](http://localhost:4502/security/users.html)を確認してください。少なくとも、FirstName、LastName、Email の各フィールド値が設定されていることを確認してください。
>[ここから](http://localhost:4502/system/console/slinglog) com.aemforms.setvalue.core.SetValueInJson のロガーを有効にすることで、デバッグのログを有効にできます

>[!NOTE]
>
>JSON データのデータ要素の値を設定する OSGi バンドルは、現在、一度に 1 つの要素の値を設定する機能をサポートしています。 複数の要素の値を設定する場合は、プロセスステップを複数回使用する必要があります。
>
>アダプティブフォームの送信オプションのデータファイルパスが「Data.xml」に設定されていることを確認します。これは、プロセスステップのコードが、ペイロードフォルダーの下で Data.xml というファイルを探すからです。
