---
title: ドラフトレターの保存と取得
description: ドラフトレターを保存して取得する方法を説明します。
feature: Interactive Communication
doc-type: article
version: Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
jira: KT-10208
exl-id: dc6f64a0-7059-4392-9c29-e66bdef4fd4d
duration: 116
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '227'
ht-degree: 100%

---

# ドラフトレターの保存と取得

次のコードは、レターインスタンスの保存に使用されます。レターインスタンスのメタデータは _icdrafts_ テーブルに格納されます。一意の文字列（draftID）が生成されて、返されます。この一意の文字列は、保存されたレターインスタンスを取得するために使用されます。

```java
public String save(CCRDocumentInstance letterToSave) throws CCRDocumentException {
  String insertRowSQL = "INSERT INTO aemformstutorial.icdrafts(draftID,documentID,status,owner,name) VALUES(?,?,?,?,?)";
  log.debug(" in save IC Draft" + letterToSave.getDocumentId() + letterToSave.getName());
  UUID uuid = UUID.randomUUID();
  String uuidString = uuid.toString();
  Connection connection = getConnection();
  PreparedStatement pstmt = null;
  Document icData = letterToSave.getData();
  try {
    pstmt = connection.prepareStatement(insertRowSQL);
    pstmt.setString(1, uuidString);
    pstmt.setString(2, letterToSave.getDocumentId());
    pstmt.setString(3, "DRAFT");
    pstmt.setString(4, letterToSave.getCreatedBy());
    pstmt.setString(5, letterToSave.getName());
    icData.copyToFile(new File(uuidString + ".xml"));
    log.debug("Executing the insert statment  " + pstmt.executeUpdate());
    connection.commit();
  } catch (IOException | SQLException e) {
    log.debug("The error is " + e.getMessage());
  } finally {
    if (pstmt != null) {
      try {
        pstmt.close();
      } catch (SQLException e) {
        log.debug("Error in closing prepared statment" + e.getMessage());
      }
    }
    if (connection != null) {
      try {
        log.debug("Closing the connection in Save Letter Draft");
        connection.close();
      } catch (SQLException e) {
        log.debug("Error in closing connection" + e.getMessage());
      }
    }

  }

  return uuidString;
}
```

## レターの取得

保存されたドラフトレターを取得するために、次のコードが作成されました。
保存したレターインスタンスを読み込むには、draftID を指定する必要があります。この draftID に基づいて、データベースに対してクエリを実行し、レターに関する追加のメタデータを取得します。同じ draftID を使用して、ファイルシステムから適切な xml を読み取って、レターのデータを作成します。次に、CCRDocumentInstance オブジェクトが作成されて、返されます。


```java
@Override
public CCRDocumentInstance get(String draftID) throws CCRDocumentException {

  String selectStatement = "Select documentID from aemformstutorial.icdrafts where draftID='" + draftID + "'";
  log.debug("The select statement is " + selectStatement);
  Connection connection = getConnection();
  Statement statement = null;
  String documentID = "";
  try {
    statement = connection.createStatement();
    ResultSet rs = statement.executeQuery(selectStatement);
    while (rs.next()) {
      documentID = rs.getString("documentID");

    }
  } catch (SQLException e) {
    log.debug("The error is " + e.getMessage());
  }
  Document draftData = new Document(new File(draftID + ".xml"));
  CCRDocumentInstance draftInstance = new CCRDocumentInstance(draftData, "abc", documentID, CCRDocumentInstance.Status.DRAFT);
  draftInstance.setId(draftID);
  return draftInstance;
}
```

### レターの更新

保存されたレターインスタンスを更新するために、次のコードを使用しました。更新されたレターのデータは、レター ID を使用してファイルシステムに書き込まれます。

```java
public void update(CCRDocumentInstance letterInstanceToUpdate) throws CCRDocumentException {
        Document icData = letterInstanceToUpdate.getData();
        String draftID = letterInstanceToUpdate.getId();
        log.debug("updating letter instance with draft id =  "+draftID);
        try
            {
                icData.copyToFile(new File(draftID+".xml"));
            } 
        catch (IOException e)
            {
                log.debug("Error updating "+e.getMessage());;
            }
        
    }
```

### 保存されているすべてのレターの取得

AEM Forms には、保存されたレターの一覧を表示する標準のユーザーインターフェイスはありません。この記事では、アダプティブフォームを使用して、保存したレターインスタンスを表形式で一覧表示します。
保存されたレターインスタンスを取得するように、クエリをカスタマイズできます。この例では、保存済みのレターインスタンスを「admin」でクエリしています。

```java
    public List < CCRDocumentInstance > getAll(String arg0, Date arg1, Date arg2, Map < String, Object > arg3) throws CCRDocumentException {
      String selectStatement = "Select * from aemformstutorial.icdrafts where owner = 'admin'";
      Connection connection = getConnection();
      Statement statement = null;
      String documentID = "";
      List < CCRDocumentInstance > listOfDrafts = new ArrayList < CCRDocumentInstance > ();
      String draftID;
      String savedInstanceName = "";
      try {
        statement = connection.createStatement();
        ResultSet rs = statement.executeQuery(selectStatement);
        while (rs.next()) {
          documentID = rs.getString("documentID");
          draftID = rs.getString("draftID");
          savedInstanceName = rs.getString("name");
          Document draftData = new Document(new File(draftID + ".xml"));
          CCRDocumentInstance draftLetter = new CCRDocumentInstance(draftData, savedInstanceName, documentID, CCRDocumentInstance.Status.DRAFT);
          listOfDrafts.add(draftLetter);
        }
      } catch (SQLException e) {
        log.debug("The error is " + e.getMessage());
      } finally {
        if (statement != null) {
          try {
            statement.close();
          } catch (SQLException e) {
            log.debug("error in closing statement" + e.getMessage());
          }
        }
        if (connection != null) {
          try {
            connection.close();
          } catch (SQLException e) {
            log.debug("error in closing connection" + e.getMessage());
          }
        }
      }

      return listOfDrafts;
    }
```

### Eclipse プロジェクト

サンプルを実装した Eclipse プロジェクトは、[こちらからダウンロード](assets/icdrafts-eclipse-project.zip)できます。
