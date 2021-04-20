---
title: JSONスキーマとデータを持つAEM Forms[Part4]
seo-title: JSONスキーマとデータを持つAEM Forms[Part4]
description: マルチパートチュートリアルを参照し、JSONスキーマを使用したアダプティブフォームの作成、送信されたデータのクエリに関する手順を実行してください。
seo-description: マルチパートチュートリアルを参照し、JSONスキーマを使用したアダプティブフォームの作成、送信されたデータのクエリに関する手順を実行してください。
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: 開発
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

---


# 送信データのクエリ


次のステップは、送信されたデータをクエリし、結果を表形式で表示することです。 これを達成するために、次のソフトウェアを使用します

[QueryBuilder](https://querybuilder.js.org/) -クエリを作成するUIコンポーネント

[データテーブル](https://datatables.net/)-クエリ結果を表形式で表示します。

次のUIは、送信されたデータに対してクエリを実行できるように設計されています。 JSONスキーマで必須とマークされた要素のみが、クエリに対して使用可能になります。 以下のスクリーンショットでは、配信環境がSMSであるすべての送信をクエリしています。

送信データをクエリするサンプルUIがQueryBuilderで使用できる高度な機能のすべてを使用しているわけではありません。 一人でやってみることが奨励されている。

![querybuilder](assets/querybuilderui.gif)

>[!NOTE]
>
>このチュートリアルの現在のバージョンでは、複数の列のクエリはサポートされていません。

クエリを実行するフォームを選択すると、**/bin/getdatakeysfromschema**&#x200B;に対するGET呼び出しが行われます。 このGET呼び出しは、フォームのスキーマに関連付けられた必須フィールドを返します。 その後、必須フィールドがQueryBuilderのドロップダウンリストに入力され、クエリを作成できます。

次のコードスニペットでは、JSONSchemaOperationsサービスのgetRequiredColumnsFromSchemaメソッドを呼び出します。 このメソッド呼び出しに、スキーマのプロパティと必須要素を渡します。 次に、この関数呼び出しによって返される配列を使用して、クエリビルダーのドロップダウンリストを設定します

```java
public JSONArray getData(String formName) throws SQLException, IOException {

  org.json.JSONArray arrayOfDataKeys = new org.json.JSONArray();
  JSONObject jsonSchema = jsonSchemaOperations.getJSONSchemaFromDataBase(formName);
  Map<String, String> refKeys = new HashMap<String, String>();

  try {
   JSONObject properties = jsonSchema.getJSONObject("properties");
   JSONArray requiredFields = jsonSchema.has("required") ? jsonSchema.getJSONArray("required") : null;
   jsonSchemaOperations.getRequiredColumnsFromSchema(properties, arrayOfDataKeys, "", jsonSchema, refKeys,
     requiredFields);
  } catch (JSONException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return arrayOfDataKeys;

 }
```

GetResultボタンがクリックされると、**&quot;/bin/querydata&quot;**&#x200B;に対するGet呼び出しが行われます。 QueryBuilder UIで作成したクエリを、クエリパラメータを介してサーブレットに渡します。 次に、サーブレットはこのクエリをSQLクエリにマッセージし、データベースのクエリに使用できます。 例えば、&#39;Mouse&#39;という名前のすべてのクエリを取得しようとする場合、製品ビルダーのクエリ文字列は$.productname = &#39;Mouse&#39;になります。 このクエリは、次の形式に変換されます。

aemformswithjsonから*を選択します。  JSON_EXTRACT( formsubmissions .formdata,&quot;$.productName &quot;)= &#39;Mouse&#39;のフォーム送信

次に、このクエリの結果が返され、UIにテーブルが設定されます。

このサンプルをローカルシステムで実行するには、次の手順を実行してください

1. [ここで説明するすべての手順に従っていることを確認します](part2.md)
1. [AEM Package Managerを使用してDashboardv2.zipを読み込みます。](assets/dashboardv2.zip) このパッケージには、必要なすべてのバンドル、設定、カスタム送信、およびサンプルページがクエリデータに含まれています。
1. サンプルのjsonスキーマを使用したアダプティブフォームの作成
1. 「customsubmithelpx」カスタム送信アクションに送信するようにアダプティブフォームを設定する
1. フォームに入力し、送信
1. ブラウザーで[ダッシュボード.html](http://localhost:4502/content/AemForms/dashboard.html)を指定します。
1. フォームを選択し、単純なクエリを実行します

