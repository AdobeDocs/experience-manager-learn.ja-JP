---
title: AEM FormsとMarketo（パート3）
seo-title: AEM FormsとMarketo（パート3）
description: AEM Formsフォームデータモデルを使用したAEM FormsとMarketoの統合に関するチュートリアルです。
seo-description: AEM Formsフォームデータモデルを使用したAEM FormsとMarketoの統合に関するチュートリアルです。
feature: アダプティブForms、フォームデータモデル
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
topic: 開発
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '403'
ht-degree: 13%

---


# データソースの設定

AEM Forms のデータ統合機能により、複数の異なるデータソースを設定して接続することができます。以下のタイプがサポートされています。これらのタイプは、すぐに使用することができます。ただし、少しカスタマイズすれば、他のデータソースとも統合できます。

1. リレーショナルデータベース - MySQL、Microsoft SQL Server、IBM DB2、Oracle RDBMS
1. AEM ユーザープロファイル
1. RESTful Web サービス
1. SOAP ベース Web サービス
1. OData サービス

AEM FormsとMarketoの統合には、RESTful Webサービスを使用します。 統合の最初の手順は、[データソースを設定することです。](https://helpx.adobe.com/experience-manager/6-4/forms/using/configure-data-sources.html#ConfigureRESTfulwebservices) このチュートリアルの一部として提供されているSwaggerファイルを使用してください。次のスクリーンショットは、データソースの設定時に指定する必要がある重要なプロパティを示しています。
![datasource](assets/datasource.jfif)

「marketo.json」はSwaggerファイルで、このチュートリアルのアセットの一部として提供されます。
プロパティホストは、Marketoインスタンスに固有です。
認証タイプはカスタムで、認証実装は「AemFormsとMarketo」に一致する必要があります。 （コード内でこれを変更していない限り）。

## フォームデータモデルを作成

データソースを設定した後、次の手順では、前の手順で設定したデータソースに基づくフォームデータモデルを作成します。 フォームデータモデルを作成するには、次の手順に従います。

ブラウザーで[データ統合ページを参照します。](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) これには、AEMインスタンスで作成されたすべてのデータ統合が表示されます。

1. 「作成」をクリックします。 |フォームデータモデル
1. FormsAndMarketoなどの意味のあるタイトルを指定し、「次へ」をクリックします。
1. 前の手順で設定したデータソースを選択し、「作成と編集」をクリックしてフォームデータモデルを編集モードで開きます
1. 「FormsAndMarketo」ノードを展開します。 「サービス」ノードを展開します。
1. 最初の「Get」操作を選択します。
1. 「選択項目を追加」をクリックします。
1. 「関連モデルオブジェクトを追加」(Add Associated Model Objects)ダイアログボックスで「すべて選択」(Select All)をクリックし、「追加」(Add)をクリックします。
1. 「保存」ボタンをクリックして、フォームデータモデルを保存します。
1. 「サービス」タブにタブを移動します。
1. 表示される唯一のサービスを選択し、「 Test Service 」をクリックします。
1. 有効なleadIdを入力し、「 Test 」をクリックします。 すべてがうまくいけば、下のスクリーンショットに示すように、リードの詳細を戻す必要があります
   ![testresults](assets/testresults.jfif)
