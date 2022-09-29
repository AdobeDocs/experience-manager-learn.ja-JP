---
title: JSON スキーマとデータを使用したAEM Forms[Part4]
seo-title: AEM Forms with JSON Schema and Data[Part4]
description: マルチパートチュートリアルで、JSON スキーマを使用したアダプティブフォームの作成と、送信されたデータのクエリに関する手順を説明します。
seo-description: Multi-Part tutorial to walk you through the steps involved in creating Adaptive Form with JSON schema and querying the submitted data.
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: a8d8118d-f4a1-483f-83b4-77190f6a42a4
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 0%

---

# 送信されたデータに対するクエリ


次の手順では、送信されたデータを照会し、結果を表形式で表示します。 これを実現するには、次のソフトウェアを使用します。

[QueryBuilder](https://querybuilder.js.org/)  — クエリを作成する UI コンポーネント

[データテーブル](https://datatables.net/) — クエリ結果を表形式で表示します。

送信済みデータのクエリを有効にする次の UI が構築されました。 JSON スキーマで必要とマークされた要素のみをクエリできます。 以下のスクリーンショットでは、deliverypref が SMS の送信をすべて照会しています。

送信されたデータに対してクエリを実行するサンプル UI は、QueryBuilder で使用できる高度な機能をすべて使用しているわけではありません。 自分で試すのがおすすめです。

![querybuilder](assets/querybuilderui.gif)

>[!NOTE]
>
>このチュートリアルの現在のバージョンでは、複数の列のクエリはサポートされていません。

クエリを実行するフォームを選択すると、に対してGET呼び出しが実行されます。 **/bin/getdatakeysfromschema**. このGET呼び出しは、フォームのスキーマに関連付けられた必須フィールドを返します。 次に、必須フィールドが QueryBuilder のドロップダウンリストに入力され、クエリを作成できます。

次のコードスニペットは、JSONSchemaOperations サービスの getRequiredColumnsFromSchema メソッドを呼び出します。 このメソッド呼び出しにスキーマのプロパティと必要な要素を渡します。 この関数呼び出しで返される配列は、クエリビルダードロップダウンリストの

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

GetResult ボタンがクリックされると、 **&quot;/bin/querydata&quot;**. QueryBuilder UI で作成したクエリを、クエリパラメーターを使用してサーブレットに渡します。 次に、サーブレットは、このクエリを SQL クエリにマッセージし、データベースのクエリに使用できます。 例えば、「Mouse」という名前のすべての製品を取得する検索を行う場合、Query Builder のクエリ文字列は次のようになります。 `$.productname = 'Mouse'`. このクエリは、次のように変換されます

選択 &#42; を aemformswithjson からダウンロードします。  JSON_EXTRACT( formsubmissions .formdata,&quot;$.productName &quot;)= &#39;Mouse&#39;の場合の formsubmissions

次に、このクエリの結果が返され、UI にテーブルが入力されます。

このサンプルをローカルシステムで実行するには、次の手順を実行してください

1. [ここで説明するすべての手順に従っていることを確認します。](part2.md)
1. [AEM Package Manager を使用して Dashboardv2.zip を読み込みます。](assets/dashboardv2.zip) このパッケージには、データを照会するために必要なすべてのバンドル、設定、カスタム送信、サンプルページが含まれています。
1. サンプルの json スキーマを使用してアダプティブフォームを作成する
1. 「customsubmithelpx」カスタム送信アクションを送信するアダプティブフォームを設定する
1. フォームに入力して送信
1. ブラウザーで次の場所を指定します。 [dashboard.html](http://localhost:4502/content/AemForms/dashboard.html)
1. フォームを選択し、単純なクエリを実行します
