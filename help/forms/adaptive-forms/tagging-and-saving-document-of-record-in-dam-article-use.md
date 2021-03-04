---
title: DAMでのAEM FormsDoRのタグ付けと保存
seo-title: DAMでのAEM FormsDoRのタグ付けと保存
description: この記事では、AEM DAMでAEM Formsが生成したDoRの保存とタグ付けの使用例について説明します。 ドキュメントのタグ付けは、送信されたフォームデータに基づいて行われます。
seo-description: この記事では、AEM DAMでAEM Formsが生成したDoRの保存とタグ付けの使用例について説明します。 ドキュメントのタグ付けは、送信されたフォームデータに基づいて行われます。
uuid: b9ba13ed-52d5-4389-a7d5-bf85e58fea49
feature: アダプティブForms，ワークフロー
topics: developing
audience: implementer
doc-type: article
activity: develop
version: 6.4,6.5
discoiquuid: 53961454-633b-4cd8-aef7-e64ab4e528e4
topic: 開発
role: デベロッパー
level: 経験豊富な
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '661'
ht-degree: 1%

---


# DAMにAEM FormsDoRをタグ付けして保存する{#tagging-and-storing-aem-forms-dor-in-dam}

この記事では、AEM DAMでAEM Formsが生成したDoRの保存とタグ付けの使用例について説明します。 ドキュメントのタグ付けは、送信されたフォームデータに基づいて行われます。

お客様からの一般的な質問は、AEM DAMでAEM Formsが生成したレコード(DoR)のドキュメントを保存し、タグ付けすることです。 ドキュメントのタグ付けは、アダプティブFormsが送信したデータに基づく必要があります。 例えば、送信されたデータの雇用ステータスが「除・売却」の場合、ドキュメントに「除・売却」タグを付け、ドキュメントをDAMに保存します。

使用例は次のとおりです。

* ユーザーがアダプティブフォームに入力します。 アダプティブフォームでは、ユーザーの婚姻状況（独身の場合）と雇用状況（退職後の場合）が取り込まれます。
* フォームの送信時に、AEMワークフローがトリガーされます。 このワークフローでは、婚姻状況（独身）と雇用状況（退職）をドキュメントにタグ付けし、ドキュメントをDAMに保存します。
* ドキュメントがDAMに保存されると、管理者は、これらのタグでドキュメントを検索できるようになります。 例えば、「単一」または「除外」で検索すると、適切なDoRが取得されます。

この使用例を満たすために、カスタムプロセスステップが記述されました。 この手順では、送信されたデータから適切なデータ要素の値を取得します。 次に、この値を使用してタグタイルを作成します。 例えば、婚姻状況の要素の値が「Single」の場合、タグのタイトルは**Peak:EmploymentStatus/Singleになります。 **TagManager APIを使用して、タグを見つけ、DoRにタグを適用します。

次のコードスニペットに、タグの検索方法とドキュメントへのタグの適用方法を示します。

```java
Tag tagFound = tagManager.resolveByTitle(tagTitle+xmlElement.getTextContent());
//tagTitle is "Peak:EmploymentStatus/" and the xmlElement.getTextContent() will return the value Single. So the tag title becomes Peak:EmploymentStatus/Single. Once the tag is found we put the tag in array and apply the tags to the resource as shown below
tagArray[i] = tagFound;
tagManager.setTags(metadata, tagArray, true);
```

このサンプルをシステムで動作させるには、次の手順に従ってください。
* [Developingwithserviceuserバンドルのデプロイ](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [setvalueバンドルをダウンロードしてデプロイします](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)。これは、送信済みフォームデータのタグを設定するカスタムOSGIバンドルです。

* [サンプルアダプティブフォームのダウンロード](assets/tag-and-store-in-dam-assets.zip)

* [Forms&amp;ドキュメントへ](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* 「作成」をクリックします。 |ファイルのアップロードとsampleadaptiveform.zipのアップロード

* [AEM Package Managerを使用して記事](assets/tag-and-store-in-dam-assets.zip) アセットを読み込む
* [サンプルフォームをプレビューモード](http://localhost:4502/content/dam/formsanddocuments/summit/peakform/jcr:content?wcmmode=disabled)で開きます。 「人」セクションに入力し、フォームを送信します。
* [DAMのPeakフォルダーに移動します](http://localhost:4502/assets.html/content/dam/Peak)。PeakフォルダーにDoRが表示されます。 ドキュメントのプロパティを確認します。 適切にタグ付けする必要があります。
これで完了です!! サンプルがシステムに正常にインストールされました

* フォーム送信時にトリガーされる[ワークフロー](http://localhost:4502/editor.html/conf/global/settings/workflow/models/TagAndStoreDoRinDAM.html)を調べます。
* ワークフローの最初の手順では、申込者の名前と住所の州名を連結して、一意のファイル名を作成します。
* ワークフローの2番目の手順では、タグ階層と、タグ付けが必要なフォームフィールド要素を渡します。 プロセス手順は、送信されたデータから値を抽出し、ドキュメントへのタグ付けが必要なタグタイトルを作成します。
* DoRをDAMの別のフォルダーに保存する場合は、次のスクリーンショットに示す設定プロパティを使用して、フォルダーの場所を指定します。

その他の2つのパラメーターは、アダプティブフォーム送信オプションで指定されたDoRおよびデータファイルパスに固有です。 ここで指定する値が、アダプティブフォーム送信オプションで指定した値と一致することを確認してください。

![タグDor](assets/tag_dor_service_configuration.gif)

