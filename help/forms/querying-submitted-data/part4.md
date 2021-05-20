---
title: JSONスキーマとデータを使用したAEM Forms[Part4]
seo-title: JSONスキーマとデータを使用したAEM Forms[Part4]
description: マルチパートチュートリアルでは、JSONスキーマを使用したアダプティブフォームの作成と送信済みデータのクエリに関する手順について説明します。
seo-description: マルチパートチュートリアルでは、JSONスキーマを使用したアダプティブフォームの作成と送信済みデータのクエリに関する手順について説明します。
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: 開発
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 0%

---


# 送信されたデータに対するクエリ


次の手順では、送信されたデータをクエリし、結果を表形式で表示します。 これを実現するために、次のソフトウェアを使用します

[QueryBuilder](https://querybuilder.js.org/)  — クエリを作成するUIコンポーネント

[データテーブル](https://datatables.net/) — クエリ結果を表形式で表示します。

送信されたデータに対してクエリを実行できるように、次のUIが構築されました。 JSONスキーマで必要とマークされた要素のみをクエリできます。 以下のスクリーンショットでは、deliveryprefがSMSであるすべての送信を照会しています。

送信されたデータに対してクエリを実行するサンプルUIは、QueryBuilderで使用できる高度な機能の一部を使用しているわけではありません。 一人で試してみるのがおすすめです。

![querybuilder](assets/querybuilderui.gif)

>[!NOTE]
>
>このチュートリアルの現在のバージョンでは、複数の列のクエリはサポートされていません。

クエリを実行するフォームを選択すると、**/bin/getdatakeysfromschema**&#x200B;に対してGET呼び出しが実行されます。 このGET呼び出しは、フォームのスキーマに関連付けられた必須フィールドを返します。 次に、必須フィールドがQueryBuilderのドロップダウンリストに入力され、クエリを作成できます。

次のコードスニペットは、JSONSchemaOperationsサービスのgetRequiredColumnsFromSchemaメソッドを呼び出します。 このメソッド呼び出しに、スキーマのプロパティと必要な要素を渡します。 この関数呼び出しで返される配列は、Query Builderドロップダウンリストの

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

「GetResult」ボタンがクリックされると、Getが&#x200B;**&quot;/bin/querydata&quot;**&#x200B;に対して呼び出されます。 QueryBuilder UIで作成されたクエリを、クエリパラメーターを通じてサーブレットに渡します。 次に、サーブレットは、このクエリをSQLクエリにマッセージし、データベースのクエリに使用できます。 例えば、&#39;Mouse&#39;という名前のすべての製品を取得する場合、Query Builderのクエリ文字列は$.productname = &#39;Mouse&#39;になります。 このクエリは、次のように変換されます

「 * 」をaemformswithjsonから選択します。  JSON_EXTRACT( formsubmissions .formdata,&quot;$.productName &quot;)= &#39;Mouse&#39;のformsubmissions

次に、このクエリの結果が返され、UIにテーブルが入力されます。

このサンプルをローカルシステムで実行するには、次の手順を実行します

1. [ここに記載されているすべての手順に従っていることを確認します。](part2.md)
1. [AEM Package Managerを使用してDashboardv2.zipを読み込みます。](assets/dashboardv2.zip) このパッケージには、データをクエリするために必要なすべてのバンドル、設定、カスタム送信、サンプルページが含まれています。
1. サンプルのjsonスキーマを使用してアダプティブフォームを作成する
1. 「customsubmithelpx」カスタム送信アクションを送信するアダプティブフォームの設定
1. フォームに入力し、
1. ブラウザーで[dashboard.html](http://localhost:4502/content/AemForms/dashboard.html)を参照します。
1. フォームを選択し、単純なクエリを実行します

