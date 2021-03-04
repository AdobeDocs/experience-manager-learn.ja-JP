---
title: AEM FormsワークフローでのJsonデータ要素の値の設定
seo-title: AEM FormsワークフローでのJsonデータ要素の値の設定
description: アダプティブフォームがAEMワークフロー内の別のユーザーにルーティングされる際、フォームの確認者に応じて、特定のフィールドやパネルを非表示または無効にする必要があります。 これらの使用例を満たすために、通常は非表示フィールドの値を設定します。 この非表示フィールドの値に基づいて、適切なパネルまたはフィールドを非表示/無効にするビジネスルールを作成できます。
seo-description: アダプティブフォームがAEMワークフロー内の別のユーザーにルーティングされる際、フォームの確認者に応じて、特定のフィールドやパネルを非表示または無効にする必要があります。 これらの使用例を満たすために、通常は非表示フィールドの値を設定します。 この非表示フィールドの値に基づいて、適切なパネルまたはフィールドを非表示/無効にするビジネスルールを作成できます。
uuid: a4ea6aef-a799-49e5-9682-3fa3b7a442fb
feature: アダプティブフォーム，ワークフロー
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.4
discoiquuid: 548fb2ec-cfcf-4fe2-a02a-14f267618d68
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '764'
ht-degree: 2%

---


# AEM Formsワークフロー{#setting-value-of-json-data-element-in-aem-forms-workflow}のJSONデータ要素の値の設定

アダプティブフォームがAEMワークフロー内の別のユーザーにルーティングされる際、フォームの確認者に応じて、特定のフィールドやパネルを非表示または無効にする必要があります。 これらの使用例を満たすために、通常は非表示フィールドの値を設定します。 この非表示フィールドの値に基づいて、適切なパネルまたはフィールドを非表示/無効にするビジネスルールを作成できます。

![jsonデータ内の要素の値の設定](assets/capture-3.gif)

AEM FormsのOSGIでは、JSONデータ要素の値を設定するためにカスタムOSGiバンドルを作成する必要があります。 バンドルは、このチュートリアルの一部として提供されます。

AEMワークフローでは、プロセスステップを使用します。 このプロセス手順に「Set Value of Element in Json」 OSGiバンドルを関連付けます。

設定値バンドルに2つの引数を渡す必要があります。 最初の引数は、値を設定する必要がある要素へのパスです。 2番目の引数は、設定する必要がある値です。

例えば、上のスクリーンショットでは、intialStep要素の値を「N」に設定しています。

afData.afUnboundData.data.initialStep,N

この例では、単純なTime Off Request Formを用意しています。 このフォームの開始者は、名前と休日を入力します。 送信時に、このフォームはレビューのために「管理者」に送信されます。 マネージャーがフォームを開くと、最初のパネルのフィールドは無効になります。 これは、JSONデータの最初の手順要素の値をNに設定したからです。

最初の手順のフィールドの値に基づいて、「管理者」が要求を承認または拒否できる承認者パネルを表示します。

「初期ステップ」に対するルールセットを確認してください。 initialStepフィールドの値に基づいて、フォームデータモデルを使用してユーザーの詳細を取得し、適切なフィールドにデータを埋め込み、適切なパネルを非表示/無効にします。

アセットをローカルシステムにデプロイするには：

* [DevelopingWithServiceUserBundleのダウンロードと展開](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [setvalueバンドルをダウンロードしてデプロイします](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。これは、送信されたjsonデータ内の要素の値を設定できるカスタムOSGIバンドルです。

* [zipファイルの内容をダウンロードして抽出します。](assets/set-value-jsondata.zip)
   * ブラウザーに[パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を指定します。
      * SetValueOfElementInJSONDataWorkflow.zipを読み込んでインストールします。このパッケージには、フォームに関連付けられているサンプルワークフローモデルとフォームデータモデルが含まれています。

* ブラウザーで[Formsと](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)ドキュメントを指定
* 「作成」をクリックします |ファイルのアップロード
* TimeOffRequestForm.zipファイルのアップロード
   **このフォームはAEM Forms6.4を使用して作成されました。AEM Forms6.4以降を使用していることを確認してください**
* [フォーム](http://localhost:4502/content/dam/formsanddocuments/timeoffrequest/jcr:content?wcmmode=disabled)を開きます
* 「開始」と「終了日」に入力し、フォームを送信します。
* [&quot;受信トレイ&quot;](http://localhost:4502/aem/inbox)に移動
* タスクに関連付けられているフォームを開きます。
* 最初のパネルのフィールドが無効になっています。
* 要求の承認または却下のパネルが表示されます。

>[!NOTE]
>
>ユーザープロファイルを使用してアダプティブフォームに事前入力するので、管理者[ユーザープロファイル情報](http://localhost:4502/security/users.html)を確認してください。 FirstName、LastName、Emailの各フィールドの値は、最低でも設定済みであることを確認してください。
>com.aemforms.setvalue.core.SetValueInJson [のロガーを有効にすると、デバッグログを有効にできます（ここから](http://localhost:4502/system/console/slinglog)）。

>[!NOTE]
>
>JSONデータのデータ要素の値を設定するためのOSGiバンドルでは、現在、1つの要素値を一度に設定する機能をサポートしています。 複数の要素の値を設定する場合は、プロセスステップを複数回使用する必要があります。
>
>アダプティブフォームの送信オプションのデータファイルのパスが「Data.xml」に設定されていることを確認します。 これは、プロセス手順のコードがペイロードフォルダーの下でData.xmlというファイルを探すためです。
