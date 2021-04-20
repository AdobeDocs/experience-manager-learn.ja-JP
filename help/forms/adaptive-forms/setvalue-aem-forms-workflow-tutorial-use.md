---
title: AEM Formsワークフローでのsetvalueの使用
seo-title: AEM Formsワークフローでのsetvalueの使用
description: アダプティブFormsでの要素の値の設定で、AEM FormsOSGIに送信されたデータ
seo-description: アダプティブFormsでの要素の値の設定で、AEM FormsOSGIに送信されたデータ
uuid: fe431e48-f05b-4b23-94d2-95d34d863984
feature: アダプティブフォーム
topics: developing
audience: implementer
doc-type: article
activity: setup
discoiquuid: dbd87302-f770-4e61-b5ad-3fc5831b4613
topic: 開発
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '550'
ht-degree: 2%

---


# AEM Formsワークフローでのsetvalueの使用

アダプティブFormsのXML要素の値の設定：AEM FormsOSGIワークフローで送信されたデータです。

![SetValue](assets/setvalue.png)

XML要素の値を設定できるset valueコンポーネントを持つLiveCycle。

この値に基づいて、フォームにXMLが入力された場合に、フォームの特定のフィールドまたはパネルを非表示/無効にできます。

AEM FormsのOSGIでは、XMLに値を設定するためのカスタムOSGiバンドルを作成する必要があります。 バンドルは、このチュートリアルの一部として提供されます。
AEMワークフローでは、プロセスステップを使用します。 このプロセス手順に「Set Value of Element in XML」 OSGiバンドルを関連付けます。
設定値バンドルに2つの引数を渡す必要があります。 最初の引数は、値を設定する必要があるXML要素のXPathです。 2番目の引数は、設定する必要がある値です。
例えば、上のスクリーンショットでは、intialstep要素の値を「N」に設定しています。
この値に基づいて、アダプティブFormsの特定のパネルが非表示または表示されます。
この例では、単純なTime Off Request Formを用意しています。 このフォームの開始者は、名前と休日を入力します。 送信時に、このフォームはレビューのために「管理者」に送信されます。 管理者がフォームを開くと、最初のパネルのフィールドは無効になります。 これは、XMLの最初のstep要素の値を「N」に設定したからです。

最初の手順のフィールドの値に基づいて、2番目のパネルを表示します。このパネルでは、「管理者」が要求を承認または拒否できます。

ルールエディターを使用して、「Time Off Requested by」フィールドに対して設定されたルールを確認してください。

ローカルシステムにアセットをデプロイするには、次の手順に従います。

* [Developingwithserviceuserバンドルのデプロイ](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [サンプルバンドルをデプロイします](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。これは、送信されたxmlデータ内の要素の値を設定できるカスタムOSGIバンドルです

* [zipファイルの内容をダウンロードして抽出します。](assets/setvalueassets.zip)
* ブラウザーに[パッケージマネージャー](http://localhost:4502/crx/packmgr/index.jsp)を指定します。
* setValueWorkflow.zipを読み込んでインストールします。 これはサンプルのワークフローモデルです。
* ブラウザーで[Formsと](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)ドキュメントを指定
* 「作成」をクリックします |ファイルのアップロード
* TimeOfRequestForm.zipをアップロードします。
* [TimeOffRequestform](http://localhost:4502/content/dam/formsanddocuments/timeoffapplication/jcr:content?wcmmode=disabled)を開きます。
* 3つの必須フィールドに入力し、
* AEMに「管理者」としてログインする（まだログインしていない場合）
* [&quot;AEM受信トレイ&quot;](http://localhost:4502/aem/inbox)に移動
* 「レビュータイムオフリクエスト」フォームを開きます
* 最初のパネルのフィールドが無効になっています。 これは、レビュー担当者がフォームを開いているためです。 また、要求の承認または却下のパネルが表示されます

>[!NOTE]
>
>ロガーを有効にして、
>com.aemforms.setvalue.core.SetValueinXml
>ブラウザーでhttp://localhost:4502/system/console/slinglogを指定して、

>[!NOTE]
>
>アダプティブフォームの送信オプションのデータファイルのパスが「Data.xml」に設定されていることを確認します。 これは、プロセスステップがペイロードフォルダーの下でData.xmlというファイルを探すためです
