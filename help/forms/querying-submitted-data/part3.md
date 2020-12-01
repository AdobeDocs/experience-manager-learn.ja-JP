---
title: JSONスキーマとデータを持つAEM Forms[Part3]
seo-title: JSONスキーマとデータを持つAEM Forms[Part3]
description: マルチパートチュートリアルを参照し、JSONスキーマを使用したアダプティブフォームの作成、送信されたデータのクエリに関する手順を実行してください。
seo-description: マルチパートチュートリアルを参照し、JSONスキーマを使用したアダプティブフォームの作成、送信されたデータのクエリに関する手順を実行してください。
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 0%

---


# JSONスキーマのデータベースへの格納{#storing-json-schema-in-database}


送信されたデータをクエリするには、送信されたフォームに関連付けられたJSONスキーマを保存する必要があります。 JSONスキーマは、クエリの構築にクエリビルダーで使用されます。

アダプティブフォームが送信されるときに、関連するJSONスキーマがデータベース内にあるかどうかを確認します。 JSONスキーマが存在しない場合は、JSONスキーマを取得し、スキーマを適切なテーブルに格納します。 また、フォーム名をJSONスキーマに関連付けます。 次のスクリーンショットは、JSONスキーマが保存されている表を示しています。

![jsonschema](assets/jsonschemas.gif)

```java
public String getJSONSchema(String afPath) {
  // TODO Auto-generated method stub
  afPath = afPath.replaceAll("/content/dam/formsanddocuments/", "/content/forms/af/");
  Resource afResource = getResolver.getServiceResolver().getResource(afPath + "/jcr:content/guideContainer");
  javax.jcr.Node resNode = afResource.adaptTo(Node.class);
  String schemaNode = null;
  try {
   schemaNode = resNode.getProperty("schemaRef").getString();
  } catch (ValueFormatException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (PathNotFoundException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (RepositoryException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  if (schemaNode.startsWith("/content/dam")) {
   log.debug("The schema is in the dam");
   afResource = getResolver.getServiceResolver()
     .getResource(schemaNode + "/jcr:content/renditions/original/jcr:content");
   resNode = afResource.adaptTo(Node.class);
   InputStream jsonSchemaStream = null;
   try {
    jsonSchemaStream = resNode.getProperty("jcr:data").getBinary().getStream();
    Charset charset = StandardCharsets.UTF_8;
    String jasonSchemaString = IOUtils.toString(jsonSchemaStream, charset);
    log.debug("The Schema is " + jasonSchemaString);
    return jasonSchemaString;
   } catch (ValueFormatException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   } catch (PathNotFoundException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   } catch (RepositoryException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   } catch (IOException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   }
  }
  if (schemaNode.startsWith("/assets")) {
   afResource = getResolver.getServiceResolver()
     .getResource(afPath + "/jcr:content/guideContainer/" + schemaNode + "/jcr:content");
   resNode = afResource.adaptTo(Node.class);
   InputStream jsonSchemaStream = null;
   try {
    jsonSchemaStream = resNode.getProperty("jcr:data").getBinary().getStream();
    Charset charset = StandardCharsets.UTF_8;
    String jasonSchemaString = IOUtils.toString(jsonSchemaStream, charset);
    log.debug("The Schema is " + jasonSchemaString);
    return jasonSchemaString;
   } catch (ValueFormatException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   } catch (PathNotFoundException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   } catch (RepositoryException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   } catch (IOException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   }
  }

  return null;

 }
```

>[!NOTE]
>
>アダプティブフォームの作成時には、リポジトリ内のJSONスキーマを使用するか、JSONスキーマをアップロードすることができます。 上記のコードは、両方のケースで機能します。

取得したスキーマは、標準のJDBC操作を使用してデータベースに保存されます。 次のコードは、スキーマをデータベースに挿入します

```java
public void insertJsonSchema(JSONObject jsonSchema, String afForm) {
  log.debug("$$$$ in insert Schema" + afForm);
  log.debug("$$$$$ The jsonSchema is  " + jsonSchema);
  Connection con = getConnection();
  log.debug("$$$$ got connection is insertJsonSchema");
  String insertTableSQL = "INSERT INTO leads.jsonschemas(jsonschema,formname) VALUES(?,?)";
  PreparedStatement pstmt = null;
  try {

   // org.json.JSONObject jsonSchemaObj = new
   // org.json.JSONObject(jsonSchema);
   pstmt = con.prepareStatement(insertTableSQL);
   pstmt.setString(1, jsonSchema.toString());
   pstmt.setString(2, afForm);
   log.debug("Executing the insert  json schema statment  " + pstmt.executeUpdate());
   con.commit();
  } catch (SQLException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } finally {
   if (con != null) {
    try {
     con.close();
    } catch (SQLException e) {
     // TODO Auto-generated catch block
     e.printStackTrace();
    }
   }
  }

 }
```

まとめると、我々は今までに以下のことを行った。

* JSONスキーマに基づくアダプティブフォームの作成
* フォームが送信中の場合は、フォームに関連付けられたJSONスキーマを初めてデータベースに格納します。
* アダプティブフォームの連結データをデータベースに保存します。

次の手順は、QueryBuilderを使用して、JSONスキーマに基づいて検索するフィールドを表示することです


