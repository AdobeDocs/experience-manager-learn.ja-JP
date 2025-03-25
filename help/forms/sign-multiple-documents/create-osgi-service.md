---
title: OSGi サービスの作成
description: 署名するフォームを保存する OSGi サービスの作成
feature: Workflow
version: Experience Manager 6.4, Experience Manager 6.5
thumbnail: 6886.jpg
jira: KT-6886
topic: Development
role: Developer
level: Experienced
exl-id: 49e7bd65-33fb-44d4-aaa2-50832dffffb0
duration: 150
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 100%

---

# OSGi サービスの作成

署名が必要なフォームを保存するために、次のコードが記述されています。署名する各フォームは、一意の GUID と顧客 ID に関連付けられています。1 つまたは複数のフォームを同じ顧客 ID に関連付けることはできますが、フォームには一意の GUID を割り当てます。

## インターフェイス

次に、使用されたインターフェイス宣言を示します。

```java
package com.aem.forms.signmultipleforms;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
public interface SignMultipleForms
{
    public void insertData(String []formNames,String formData,String serverURL,WorkItem workItem, WorkflowSession workflowSession);
    public String getNextFormToSign(int customerID);
    public void updateSignatureStatus(String formData,String guid);
    public String getFormData(String guid);
}
```



## データの挿入

insertData メソッドは、データソースによって識別されるデータベースに行を挿入します。データベースの各行は 1 つのフォームに対応し、GUID と顧客 ID で一意に識別されます。フォームデータとフォーム URL も、この行に格納されます。ステータス列は、フォームが入力され、署名されているかどうかを示します。値が 0 の場合は、フォームがまだ署名されていないことを示します。

```java
@Override
public void insertData(String[] formNames, String formData, String serverURL, WorkItem workItem, WorkflowSession workflowSession) {
  String insertTableSQL = "INSERT INTO aemformstutorial.signingforms(formName,formData,guid,status,customerID) VALUES(?,?,?,?,?)";
  Random r = new Random();
  Connection con = getConnection();
  PreparedStatement pstmt = null;
  int customerIDGnerated = r.nextInt((1000 - 1) + 1) + 1;
  log.debug("The number of forms to insert are  " + formNames.length);
  try {
    for (int i = 0; i < formNames.length; i++) {
      log.debug("Inserting form name " + formNames[i]);
      UUID uuid = UUID.randomUUID();
      String randomUUIDString = uuid.toString();
      String formUrl = serverURL + "/content/dam/formsanddocuments/formsandsigndemo/" + formNames[i] + "/jcr:content?wcmmode=disabled&guid=" + randomUUIDString + "&customerID=" + customerIDGnerated;
      pstmt = con.prepareStatement(insertTableSQL);
      pstmt.setString(1, formUrl);
      pstmt.setString(2, formData.replace("<guid>3938</guid>", "<guid>" + uuid + "</guid>"));
      pstmt.setString(3, uuid.toString());
      pstmt.setInt(4, 0);
      pstmt.setInt(5, customerIDGnerated);
      log.debug("customerIDGnerated the insert statment  " + pstmt.execute());
      if (i == 0) {
        WorkflowData wfData = workItem.getWorkflowData();
        wfData.getMetaDataMap().put("formURL", formUrl);
        workflowSession.updateWorkflowData(workItem.getWorkflow(), wfData);
        log.debug("$$$$ Done updating the map");

}

}
    con.commit();
  }
  catch(Exception e) {
    log.debug(e.getMessage());
  }
  finally {
    if (pstmt != null) {
      try {
        pstmt.close();
      } catch(SQLException e) {

log.debug(e.getMessage());
      }
    }
    if (con != null) {
      try {
        con.close();
      } catch(SQLException e) {
        log.debug(e.getMessage());
      }
    }
  }

}
```


## フォームデータの取得

次のコードは、指定された GUID に関連付けられたアダプティブフォームのデータを取得するために使用します。その後、アダプティブフォームに事前入力するためにこのフォームデータが使用されます。

```java
@Override
public String getFormData(String guid) {
  log.debug("### Getting form data asscoiated with guid " + guid);
  Connection con = getConnection();
  try {
    Statement st = con.createStatement();
    String query = "SELECT formData FROM aemformstutorial.signingforms where guid = '" + guid + "'" + "";
    log.debug(" The query being consrtucted " + query);
    ResultSet rs = st.executeQuery(query);
    while (rs.next()) {
      return rs.getString("formData");
    }
  } catch(SQLException e) {
    // TODO Auto-generated catch block
    log.debug(e.getMessage());
  }

  return null;

}
```

## 署名ステータスの更新

署名が正常に完了すると、フォームに関連付けられた AEM ワークフローがトリガーされます。ワークフローの最初の手順は、GUID と顧客 ID で識別される行のステータスをデータベース内で更新するプロセス手順です。また、フォームデータの署名済み要素の値を Y に設定し、フォームが入力および署名済みであることを示します。 アダプティブフォームにはこのデータが入力され、XML データ内の署名済みデータ要素の値を使用して、適切なメッセージが表示されます。updateSignatureStatus コードが、カスタムプロセス手順から呼び出されます。


```java
public void updateSignatureStatus(String formData, String guid) {
  String updateStatment = "update aemformstutorial.signingforms SET formData = ?, status = ? where guid = ?";
  PreparedStatement updatePS = null;
  Connection con = getConnection();
  try {
    updatePS = con.prepareStatement(updateStatment);
    updatePS.setString(1, formData.replace("<signed>N</signed>", "<signed>Y</signed>"));
    updatePS.setInt(2, 1);
    updatePS.setString(3, guid);
    log.debug("Updated the signature status " + String.valueOf(updatePS.execute()));
  } catch(SQLException e) {
    log.debug(e.getMessage());
  }
  finally {
    try {
      con.commit();
      updatePS.close();
      con.close();
    } catch(SQLException e) {
      
      log.debug(e.getMessage());
    }

  }

}
```

## 署名する次のフォームの取得

ステータスが 0 の指定された顧客 ID の署名のために、以下のコードを使用して次のフォームを取得します。SQL クエリが行を返さない場合は、文字列「**AllDone**」を返します。これは、指定された顧客 ID に署名するためのフォームがこれ以上ないことを示します。

```java
@Override
public String getNextFormToSign(int customerID) {
  System.out.println("### inside my next form to sign " + customerID);
  String selectStatement = "SELECT formName FROM aemformstutorial.signingforms where status = 0 and customerID=" + customerID;
  Connection con = getConnection();
  try
   {
    Statement st = con.createStatement();
    ResultSet rs = st.executeQuery(selectStatement);
    while (rs.next()) {
      log.debug("Got result set object");
      return rs.getString("formName");
    }
    if (!rs.next()) {
      return "AllDone";
    }
  } catch(SQLException e) {
    log.debug(e.getMessage());
  }
  finally {
    try {
      con.close();
    } catch(SQLException e) {
      // TODO Auto-generated catch block
      log.debug(e.getMessage());
    }
  }

  return null;

}
```



## Assets

上記のサービスを含む OSGi バンドルは、[こちらからダウンロード](assets/sign-multiple-forms.jar)できます。

## 次の手順

[最初のフォーム送信を処理するメインワークフローの作成](./create-main-workflow.md)
