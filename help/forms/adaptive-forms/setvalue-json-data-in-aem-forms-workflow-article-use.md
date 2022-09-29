---
title: AEM Forms Workflow での Json データ要素の値の設定
description: アダプティブフォームがAEMワークフロー内の別のユーザーにルーティングされるので、フォームのレビュー担当者に応じて、特定のフィールドやパネルの表示/非表示を切り替える必要があります。 これらの使用例を満たすために、通常は非表示フィールドの値を設定します。 この非表示フィールドの値に基づいて、ビジネスルールを作成して、適切なパネルやフィールドを非表示/無効にすることができます。
feature: Adaptive Forms
version: 6.4
topic: Development
role: Developer
level: Experienced
exl-id: fbe6d341-7941-46f5-bcd8-58b99396d351
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '685'
ht-degree: 2%

---

# AEM Forms Workflow での JSON データ要素の値の設定 {#setting-value-of-json-data-element-in-aem-forms-workflow}

アダプティブフォームがAEMワークフロー内の別のユーザーにルーティングされるので、フォームのレビュー担当者に応じて、特定のフィールドやパネルの表示/非表示を切り替える必要があります。 これらの使用例を満たすために、通常は非表示フィールドの値を設定します。 この非表示フィールドの値に基づいて、ビジネスルールを作成して、適切なパネルやフィールドを非表示/無効にすることができます。

![JSON データ内の要素の値を設定する](assets/capture-3.gif)

AEM Forms OSGi で — JSON データ要素の値を設定するカスタム OSGi バンドルを作成する必要があります。 バンドルは、このチュートリアルの一部として提供されます。

ここでは、AEMワークフローのプロセスステップを使用します。 このプロセスステップに「Set Value of Element in Json」 OSGi バンドルを関連付けます。

設定値バンドルに 2 つの引数を渡す必要があります。 最初の引数は、値を設定する必要がある要素のパスです。 2 番目の引数は、設定する必要がある値です。

例えば、上のスクリーンショットでは、 intialStep 要素の値を「N」に設定しています。

afData.afUnboundData.data.initialStep,N

この例では、単純なタイムオフリクエストフォームを使用します。 このフォームの開始者は、名前と休日を入力します。 このフォームは送信時に、レビュー用に「マネージャー」に送信されます。 マネージャーがフォームを開くと、最初のパネルのフィールドは無効になります。 これは、JSON データの最初の手順要素の値が N に設定されているからです。

最初のステップフィールドの値に基づいて、「管理者」がリクエストを承認または却下できる承認者パネルを表示します。

「最初のステップ」に対するルールセットをご覧ください。 「 initialStep 」フィールドの値に基づいて、フォームデータモデルを使用してユーザーの詳細を取得し、適切なフィールドに値を入力して、適切なパネルを非表示/無効にします。

ローカルシステムにアセットをデプロイするには：

* [DevelopingWithServiceUserBundle のダウンロードとデプロイ](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [setvalue バンドルをダウンロードしてデプロイします。](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). これは、送信された JSON データ内の要素の値を設定できるカスタム OSGi バンドルです。

* [zip ファイルの内容をダウンロードして抽出します。](assets/set-value-jsondata.zip)
   * ブラウザーで次の場所を指定します。 [パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)
      * SetValueOfElementInJSONDataWorkflow.zip を読み込んでインストールします。このパッケージには、サンプルのワークフローモデルとフォームデータモデルが関連付けられています。

* ブラウザーで次の場所を指定します。 [Formsとドキュメント](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* 「作成」をクリックします。 |ファイルのアップロード
* TimeOffRequestForm.zip ファイルのアップロード
   **このフォームはAEM Forms 6.4 を使用して構築されました。AEM Forms 6.4 以降を使用していることを確認してください。**
* を開きます。 [フォーム](http://localhost:4502/content/dam/formsanddocuments/timeoffrequest/jcr:content?wcmmode=disabled)
* 開始日と終了日を入力し、フォームを送信します。
* に移動します。 [&quot;インボックス&quot;](http://localhost:4502/aem/inbox)
* タスクに関連付けられているフォームを開きます。
* 最初のパネルのフィールドは無効になっています。
* リクエストの承認または却下のパネルが表示されます。

>[!NOTE]
>
>ユーザープロファイルを使用してアダプティブフォームに事前入力するので、管理者 [ユーザープロファイル情報 ](http://localhost:4502/security/users.html). 最低でも、FirstName、LastName、Email の各フィールド値が設定されていることを確認してください。
>com.aemforms.setvalue.core.SetValueInJson のロガーを有効にすることで、デバッグのログを有効にできます [ここから](http://localhost:4502/system/console/slinglog)

>[!NOTE]
>
>JSON データのデータ要素の値を設定する OSGi バンドルは、現在、1 つの要素の値を一度に設定する機能をサポートしています。 複数の要素の値を設定する場合は、プロセスステップを複数回使用する必要があります。
>
>アダプティブフォームの送信オプションのデータファイルパスが「Data.xml」に設定されていることを確認します。 これは、プロセスステップのコードが、ペイロードフォルダーの下で Data.xml というファイルを探すからです。
