---
title: DAMでのAEM Forms DoRのタグ付けと保存
seo-title: DAMでのAEM Forms DoRのタグ付けと保存
description: この記事では、AEM DAMでAEM Formsによって生成されたDoRの保存とタグ付けの使用例について説明します。 ドキュメントのタグ付けは、送信されたフォームデータに基づいておこなわれます。
seo-description: この記事では、AEM DAMでAEM Formsによって生成されたDoRの保存とタグ付けの使用例について説明します。 ドキュメントのタグ付けは、送信されたフォームデータに基づいておこなわれます。
uuid: b9ba13ed-52d5-4389-a7d5-bf85e58fea49
feature: アダプティブForms，ワークフロー
topics: developing
audience: implementer
doc-type: article
activity: develop
version: 6.4,6.5
discoiquuid: 53961454-633b-4cd8-aef7-e64ab4e528e4
topic: 開発
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '659'
ht-degree: 1%

---


# AEM Forms DoRのDAM {#tagging-and-storing-aem-forms-dor-in-dam}でのタグ付けと保存

この記事では、AEM DAMでAEM Formsによって生成されたDoRの保存とタグ付けの使用例について説明します。 ドキュメントのタグ付けは、送信されたフォームデータに基づいておこなわれます。

よくあるお客様からの質問は、AEM DAMでAEM Formsによって生成されたレコードのドキュメント(DoR)を保存し、タグ付けすることです。 ドキュメントのタグ付けは、アダプティブFormsの送信済みデータに基づいて行う必要があります。 例えば、送信されたデータの雇用ステータスが「退職」の場合、ドキュメントに「退職」タグを付け、ドキュメントをDAMに保存します。

使用例を次に示します。

* ユーザーがアダプティブフォームに記入します。 アダプティブフォームでは、ユーザーの婚姻状況（例：独身）と雇用状況（例：退職）が取り込まれます。
* フォームの送信時に、AEM Workflowがトリガーされます。 婚姻状況（単一）と雇用状況（退職）をドキュメントにタグ付けし、ドキュメントをDAMに保存します。
* ドキュメントをDAMに保存したら、管理者は、これらのタグでドキュメントを検索できるようになります。 例えば、「Single」または「Lytied」の検索では、適切なDoRが取得されます。

この使用例を満たすために、カスタムプロセスステップが記述されました。 この手順では、送信されたデータから適切なデータ要素の値を取得します。 次に、この値を使用してタグタイルを作成します。 例えば、婚姻状況要素の値が「Single」の場合、タグのタイトルは**Peak:EmploymentStatus/Singleになります。 ** TagManager APIを使用して、タグを見つけ、DoRにタグを適用します。

次のコードスニペットは、タグを検索し、タグをドキュメントに適用する方法を示しています。

```java
Tag tagFound = tagManager.resolveByTitle(tagTitle+xmlElement.getTextContent());
//tagTitle is "Peak:EmploymentStatus/" and the xmlElement.getTextContent() will return the value Single. So the tag title becomes Peak:EmploymentStatus/Single. Once the tag is found we put the tag in array and apply the tags to the resource as shown below
tagArray[i] = tagFound;
tagManager.setTags(metadata, tagArray, true);
```

このサンプルをシステムで動作させるには、次の手順に従ってください。
* [Developingwithserviceuserバンドルをデプロイします。](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [setvalueバンドルをダウンロードしてデプロイします](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。これは、送信されたフォームデータからタグを設定するカスタムOSGIバンドルです。

* [サンプルのアダプティブフォームのダウンロード](assets/tag-and-store-in-dam-assets.zip)

* [Formsとドキュメントに移動](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* 「作成」をクリックします。 |ファイルサンプルのadaptiveform.zipをアップロードしてアップロードします。

* [AEMパッケージマネージャーを使](assets/tag-and-store-in-dam-assets.zip) 用して記事アセットを読み込みます。
* [サンプルフォームをプレビューモードで開きます。](http://localhost:4502/content/dam/formsanddocuments/summit/peakform/jcr:content?wcmmode=disabled) 「人」セクションに入力し、フォームを送信します。
* [DAMのPeakフォルダーに移動します](http://localhost:4502/assets.html/content/dam/Peak)。PeakフォルダにDoRが表示されます。 ドキュメントのプロパティを確認します。 適切にタグ付けする必要があります。
これで完了です!! システムにサンプルが正常にインストールされました

* フォーム送信時にトリガーされる[ワークフロー](http://localhost:4502/editor.html/conf/global/settings/workflow/models/TagAndStoreDoRinDAM.html)を調べます。
* ワークフローの最初の手順では、応募者名と居住地区を連結して一意のファイル名を作成します。
* ワークフローの2番目の手順では、タグ階層と、タグ付けが必要なフォームフィールド要素を渡します。 プロセスステップは、送信されたデータから値を抽出し、ドキュメントにタグ付けする必要があるタグタイトルを作成します。
* DAMの別のフォルダーにDoRを保存する場合は、以下のスクリーンショットに示す設定プロパティを使用して、フォルダーの場所を指定します。

その他の2つのパラメーターは、アダプティブフォーム送信オプションで指定されたDoRとデータファイルパスに固有です。 ここで指定した値が、アダプティブフォーム送信オプションで指定した値と一致することを確認してください。

![タグDor](assets/tag_dor_service_configuration.gif)

