---
title: AEM Formsとマーケット（パート3）
seo-title: AEM Formsとマーケット（パート3）
description: AEM Formsフォームデータモデルを使用したAEM Formsとマーケティングツールの統合に関するチュートリアルです。
seo-description: AEM Formsフォームデータモデルを使用したAEM Formsとマーケティングツールの統合に関するチュートリアルです。
feature: 「アダプティブForms、フォームデータモデル」
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
topic: 開発
role: デベロッパー
level: 経験豊富な
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 13%

---


# データソースの設定

AEM Forms のデータ統合機能により、複数の異なるデータソースを設定して接続することができます。以下のタイプがサポートされています。これらのタイプは、すぐに使用することができます。ただし、少しカスタマイズすれば、他のデータソースとも統合できます。

1. リレーショナルデータベース - MySQL、Microsoft SQL Server、IBM DB2、Oracle RDBMS
1. AEM ユーザープロファイル
1. RESTful Web サービス
1. SOAP ベース Web サービス
1. OData サービス

AEM FormsとMarketoの統合に関しては、RESTful Webサービスを使用します。 統合の最初の手順は、[データソースを設定することです。](https://helpx.adobe.com/experience-manager/6-4/forms/using/configure-data-sources.html#ConfigureRESTfulwebservices) このチュートリアルの一部として提供されているSwaggerファイルを使用してください。次のスクリーンショットは、データソースの設定時に指定する必要がある重要なプロパティを示しています。
![datasource](assets/datasource.jfif)

「marketo.json」はSwaggerファイルで、このチュートリアルのアセットの一部として提供されます。
プロパティホストは、Marketorインスタンスに固有です。
認証タイプはカスタムで、認証の実装は「AemFormsとMarketor」を一致させる必要があります。 （コード内でこれを変更していない限り）。

## フォームデータモデルを作成

データソースの設定後、次の手順では、前の手順で設定したデータソースに基づくForm Data Modelを作成します。 フォームデータモデルを作成するには、次の手順に従います。

ブラウザーで[データ統合ページを指定します。](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) これは、AEMインスタンスで作成されるすべてのデータ統合をリストします。

1. 「作成」をクリックします |フォームデータモデル
1. FormsAndMarketorなどの意味のあるタイトルを指定し、「次へ」をクリックします。
1. 前の手順で設定したデータソースを選択し、「作成と編集」をクリックして、フォームデータモデルを編集モードで開きます
1. 「FormsAndMarketo」ノードを展開します。 サービスノードを展開します
1. 最初の「Get」操作を選択します
1. [選択追加]をクリック
1. 「関連付けられたモデルオブジェクト」(追加Associated Model Objects)ダイアログボックスの「すべて選択」(Select All)をクリックし、追加をクリックします。
1. 「保存」ボタンをクリックして、フォームデータモデルを保存します
1. 「サービス」タブへのタブ
1. 表示される唯一のサービスを選択し、「Test Service」をクリックします
1. 有効なleadIdを入力し、「Test」をクリックします。 すべてがうまくいけば、下のスクリーンショットに示すように、リードの詳細を戻す必要があります。
   ![testresults](assets/testresults.jfif)
