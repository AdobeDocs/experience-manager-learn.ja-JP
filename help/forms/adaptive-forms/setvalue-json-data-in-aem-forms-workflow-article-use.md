---
title: AEM FormsワークフローでのJsonデータ要素の値の設定
seo-title: AEM FormsワークフローでのJsonデータ要素の値の設定
description: アダプティブフォームがAEMワークフロー内の別のユーザーにルーティングされるので、フォームのレビュー担当者に基づいて特定のフィールドやパネルの表示/非表示を切り替える必要があります。 これらの使用例を満たすために、通常は非表示フィールドの値を設定します。 この非表示フィールドの値に基づいて、ビジネスルールを作成し、適切なパネルまたはフィールドを非表示/無効にできます。
seo-description: アダプティブフォームがAEMワークフロー内の別のユーザーにルーティングされるので、フォームのレビュー担当者に基づいて特定のフィールドやパネルの表示/非表示を切り替える必要があります。 これらの使用例を満たすために、通常は非表示フィールドの値を設定します。 この非表示フィールドの値に基づいて、ビジネスルールを作成し、適切なパネルまたはフィールドを非表示/無効にできます。
uuid: a4ea6aef-a799-49e5-9682-3fa3b7a442fb
feature: アダプティブフォーム
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.4
discoiquuid: 548fb2ec-cfcf-4fe2-a02a-14f267618d68
topic: 開発
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '765'
ht-degree: 2%

---


# AEM FormsワークフローのJSONデータ要素の値の設定{#setting-value-of-json-data-element-in-aem-forms-workflow}

アダプティブフォームがAEMワークフロー内の別のユーザーにルーティングされるので、フォームのレビュー担当者に基づいて特定のフィールドやパネルの表示/非表示を切り替える必要があります。 これらの使用例を満たすために、通常は非表示フィールドの値を設定します。 この非表示フィールドの値に基づいて、ビジネスルールを作成し、適切なパネルまたはフィールドを非表示/無効にできます。

![JSONデータでの要素の値の設定](assets/capture-3.gif)

AEM Forms OSGiでは、JSONデータ要素の値を設定するためにカスタムOSGiバンドルを作成する必要があります。 バンドルは、このチュートリアルの一部として提供されます。

AEMワークフローでプロセスステップを使用します。 このプロセスステップに、「Set Value of Element in Json」OSGiバンドルを関連付けます。

設定値バンドルに2つの引数を渡す必要があります。 最初の引数は、値を設定する必要がある要素へのパスです。 2番目の引数は、設定する必要がある値です。

例えば、上のスクリーンショットでは、 intialStep要素の値を「N」に設定しています。

afData.afUnboundData.data.initialStep,N

この例では、単純なオフリクエストフォームを使用します。 このフォームの開始者は、自分の名前と休日を入力します。 このフォームは、送信時に「管理者」に送信され、レビューされます。 マネージャーがフォームを開くと、最初のパネルのフィールドは無効になります。 これは、JSONデータの最初の手順要素の値をNに設定したからです。

「管理者」がリクエストを承認または却下できる承認者パネルを、最初のステップフィールドの値に基づいて表示します。

「最初のステップ」に対するルールセットをご覧ください。 initialStepフィールドの値に基づいて、フォームデータモデルを使用してユーザーの詳細を取得し、適切なフィールドに値を入力して、適切なパネルを非表示/無効にします。

ローカルシステムにアセットをデプロイするには：

* [DevelopingWithServiceUserBundleのダウンロードとデプロイ](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [setvalueバンドルをダウンロードしてデプロイします](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。これは、送信されたJSONデータ内の要素の値を設定できるカスタムOSGiバンドルです。

* [zipファイルの内容をダウンロードして抽出します。](assets/set-value-jsondata.zip)
   * ブラウザーで[パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を参照します。
      * SetValueOfElementInJSONDataWorkflow.zipを読み込んでインストールします。このパッケージには、フォームに関連付けられたサンプルのワークフローモデルとフォームデータモデルが含まれています。

* ブラウザーで[Formsとドキュメント](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)を参照します。
* 「作成」をクリックします。 |ファイルのアップロード
* TimeOffRequestForm.zipファイルのアップロード
   **このフォームはAEM Forms 6.4を使用して構築されています。AEM Forms 6.4以降を使用していることを確認してください。**
* [フォーム](http://localhost:4502/content/dam/formsanddocuments/timeoffrequest/jcr:content?wcmmode=disabled)を開きます。
* 開始日と終了日を入力し、フォームを送信します。
* [&quot;Inbox&quot;](http://localhost:4502/aem/inbox)に移動します
* タスクに関連付けられたフォームを開きます。
* 最初のパネルのフィールドは無効になっています。
* 要求の承認または拒否のパネルが表示されます。

>[!NOTE]
>
>ユーザープロファイルを使用してアダプティブフォームに事前入力するので、管理者の[ユーザープロファイル情報](http://localhost:4502/security/users.html)を必ずお読みください。 最低でも、FirstName、LastNameおよびEmailフィールドの値が設定されていることを確認してください。
>com.aemforms.setvalue.core.SetValueInJson [のロガーを有効にすることで、デバッグのログを有効にできます。](http://localhost:4502/system/console/slinglog)

>[!NOTE]
>
>JSONデータのデータ要素の値を設定するためのOSGiバンドルは、現在、1つの要素値を一度に設定する機能をサポートしています。 複数の要素値を設定する場合は、プロセスステップを複数回使用する必要があります。
>
>アダプティブフォームの送信オプションのデータファイルのパスが「Data.xml」に設定されていることを確認します。 これは、プロセスステップのコードがペイロードフォルダーの下でData.xmlというファイルを探すからです。
