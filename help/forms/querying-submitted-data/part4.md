---
title: JSON スキーマとデータを使用した AEM Forms（パート 4）
description: 複数のパートで構成されているチュートリアルでは、JSON スキーマを使用したアダプティブフォームの作成と、送信されたデータのクエリに関する手順を説明します。
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: a8d8118d-f4a1-483f-83b4-77190f6a42a4
duration: 99
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '432'
ht-degree: 100%

---

# 送信されたデータに対するクエリ


次の手順では、送信されたデータを照会し、結果を表形式で表示します。これには、次のソフトウェアを使用します。

[QueryBuilder](https://querybuilder.js.org/) - クエリを作成する UI コンポーネント

[データテーブル](https://datatables.net/) - クエリ結果を表形式で表示します。

送信済みデータのクエリを有効化するために、次の UI が構築されました。JSON スキーマで必要とマークされた要素のみをクエリできます。以下のスクリーンショットでは、deliverypref が SMS のすべての送信を照会しています。

送信されたデータに対してクエリを実行するサンプル UI は、QueryBuilder で使用できる高度な機能をすべて使用しているわけではありません。お客様自身でお試しください。

![querybuilder](assets/querybuilderui.gif)

>[!NOTE]
>
>このチュートリアルの現在のバージョンでは、複数の列のクエリはサポートされていません。

クエリを実行するフォームを選択すると、**/bin/getdatakeysfromschema** に対して GET 呼び出しが実行されます。この GET 呼び出しでは、フォームのスキーマに関連付けられた必須フィールドが返されます。次に、クエリを作成するための必須フィールドが QueryBuilder のドロップダウンリストに入力されます。

次のコードスニペットは、JSONSchemaOperations サービスの getRequiredColumnsFromSchema メソッドを呼び出します。このメソッド呼び出しに、プロパティとスキーマの必須要素を渡します。この関数呼び出しで返される配列は、クエリビルダーのドロップダウンリストへの入力に使用されます

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

「GetResult」ボタンをクリックすると、Get 呼び出しが&#x200B;**「/bin/querydata」**&#x200B;に対して行われます。QueryBuilder UI で作成したクエリを、クエリパラメーターを使用してサーブレットに渡します。次に、サーブレットがこのクエリを SQL クエリに変換し、データベースのクエリに使用できる状態になります。例えば、「Mouse」という名前のすべての製品を検索する場合、クエリビルダーのクエリ文字列は `$.productname = 'Mouse'` になります。このクエリは、次のように変換されます

aemformswithjson から &#42; を選択します。formsubmissions  where JSON_EXTRACT(  formsubmissions .formdata,&quot;$.productName &quot;)= &#39;Mouse&#39;

次に、このクエリの結果が返され、UI のテーブルに入力されます。

このサンプルをローカルシステムで実行するには、次の手順を実行します

1. [ここで説明するすべての手順に従っていることを確認します](part2.md)
1. [AEM パッケージマネージャーを使用して Dashboardv2.zip を読み込みます。](assets/dashboardv2.zip) このパッケージには、データを照会するために必要なすべてのバンドル、設定、カスタム送信、サンプルページが含まれています。
1. サンプルの json スキーマを使用してアダプティブフォームを作成します
1. 「customsubmithelpx」カスタム送信アクションに送信するように、アダプティブフォームを設定します
1. フォームに入力して送信します
1. ブラウザーで [dashboard.html](http://localhost:4502/content/AemForms/dashboard.html) にアクセスします
1. フォームを選択し、シンプルなクエリを実行します
