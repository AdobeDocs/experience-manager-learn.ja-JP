---
title: AEM FormsとMarketo（第 3 部）
description: AEM Formsフォームデータモデルを使用したAEM FormsとMarketoの統合に関するチュートリアル
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 7096340b-8ccf-4f5e-b264-9157232e96ba
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '379'
ht-degree: 13%

---

# データソースの設定

AEM Forms のデータ統合機能により、複数の異なるデータソースを設定して接続することができます。以下のタイプがサポートされています。これらのタイプは、すぐに使用することができます。ただし、少しカスタマイズすれば、他のデータソースとも統合できます。

1. リレーショナルデータベース - MySQL、Microsoft SQL Server、IBM DB2、Oracle RDBMS
1. AEM ユーザープロファイル
1. RESTful Web サービス
1. SOAP ベース Web サービス
1. OData サービス

AEM FormsとMarketoの統合には、RESTful Web サービスを使用します。 統合の最初の手順は、 [データソース。](https://helpx.adobe.com/experience-manager/6-4/forms/using/configure-data-sources.html#ConfigureRESTfulwebservices) このチュートリアルの一部として提供されている Swagger ファイルを使用してください。 次のスクリーンショットは、データソースの設定時に指定する必要がある重要なプロパティを示しています。
![datasource](assets/datasource.jfif)

「marketo.json」は Swagger ファイルで、このチュートリアルのアセットの一部として提供されます。
プロパティホストはMarketoインスタンスに固有です。
認証タイプはカスタムで、認証実装は「AemForms とMarketo」を一致させる必要があります。 （コード内でこれを変更していない場合）。

## フォームデータモデルの作成

データソースを設定した後、次の手順では、前の手順で設定したデータソースに基づくフォームデータモデルを作成します。 フォームデータモデルを作成するには、次の手順に従ってください。

ブラウザーで [データ統合ページを参照してください。](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) これには、AEMインスタンスで作成されたすべてのデータ統合が表示されます。

1. 「作成」をクリックします。 |フォームデータモデル
1. FormsAndMarketo などの意味のあるタイトルを入力し、「次へ」をクリックします。
1. 前の手順で設定したデータソースを選択し、「作成と編集」をクリックしてフォームデータモデルを編集モードで開きます
1. 「FormsAndMarketo」ノードを展開します。 「サービス」ノードを展開します。
1. 最初の「Get」操作を選択します。
1. 「選択項目を追加」をクリックします。
1. [ 関連モデルオブジェクトを追加 ] ダイアログボックスで [ すべて選択 ] をクリックし、[ 追加 ] をクリックします。
1. 「保存」ボタンをクリックしてフォームデータモデルを保存します
1. 「サービス」タブにタブを移動します。
1. 表示される唯一のサービスを選択し、「 Test Service 」をクリックします。
1. 有効な leadId を入力し、「テスト」をクリックします。 すべてがうまくいけば、下のスクリーンショットに示すように、リードの詳細を返す必要があります
   ![testresults](assets/testresults.jfif)
