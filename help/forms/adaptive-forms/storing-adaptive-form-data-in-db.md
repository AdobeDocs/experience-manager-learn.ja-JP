---
title: アダプティブフォームデータの保存
description: AEM ワークフローの一部としてアダプティブフォームデータをデータベースに保存する
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 3dd552da-fc7c-4fc7-97ec-f20b6cc33df0
last-substantial-update: 2020-03-20T00:00:00Z
duration: 146
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '382'
ht-degree: 100%

---

# アダプティブフォーム送信のデータベースへの保存

送信されたフォームデータを選択したデータベースに保存するには、いくつかの方法があります。JDBC データソースを使用すると、データをデータベースに直接保存できます。カスタム OSGi バンドルを書き込むと、データをデータベースに保存できます。この記事では、AEM ワークフローのカスタムプロセス手順を使用してデータを保存します。
ユースケースは、アダプティブフォームの送信時に行う AEM ワークフローのトリガーであり、このワークフローの手順では、送信したデータをデータベースに保存します。



## JDBC 接続プール

* [ConfigMgr](http://localhost:4502/system/console/configMgr) に移動します

   * 「JDBC 接続プール」を検索します。新しい Day Commons JDBC 接続プールを作成します。データベースに固有の設定を指定します。

   * ![JDBC 接続プールの OSGi 設定](assets/aemformstutorial-jdbc.png)

## データベースの詳細を指定

* 「**データベースの詳細を指定**」を検索します
* データベースに固有のプロパティを指定します。
   * DataSourceName：以前に設定したデータソースの名前。
   * TableName - AF データを保存するテーブルの名前
   * FormName - フォームの名前を保存する列名
   * ColumnName - AF データを保存する列名

  ![データベースの詳細に OSGi 設定を指定](assets/specify-database-details.png)



## OSGi 設定のコード

```java
package com.aemforms.dbsamples.core.insertFormData;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;

@ObjectClassDefinition(name = "Specify Database details", description = "Specify Database details")

public @interface InsertFormDataConfiguration {
  @AttributeDefinition(name = "DataSourceName", description = "Data Source Name configured")
  String dataSourceName() default "";
  @AttributeDefinition(name = "TableName", description = "Name of the table")
  String tableName() default "";
  @AttributeDefinition(name = "FormName", description = "Column Name for form name")
  String formName() default "";
  @AttributeDefinition(name = "columnName", description = "Column Name for form data")
  String columnName() default "";

}
```

## 設定値の読み取り

```java
package com.aemforms.dbsamples.core.insertFormData;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;

@Component(service={InsertFormDataConfigurationService.class})
@Designate(ocd=InsertFormDataConfiguration.class)

public class InsertFormDataConfigurationService {
    public String TABLE_NAME;
    public String DATA_SOURCE_NAME;
    public String COLUMN_NAME;
    public String FORM_NAME;
    @Activate      
      protected final void activate(InsertFormDataConfiguration insertFormDataConfiguration)
      {
        TABLE_NAME = insertFormDataConfiguration.tableName();
        DATA_SOURCE_NAME = insertFormDataConfiguration.dataSourceName();
        COLUMN_NAME = insertFormDataConfiguration.columnName();
        FORM_NAME = insertFormDataConfiguration.formName();
      }
    public String getTABLE_NAME()
    {
        return TABLE_NAME;
    }
    public String getDATA_SOURCE_NAME()
    {
        return DATA_SOURCE_NAME;
    }
    public String getCOLUMN_NAME()
    {
        return COLUMN_NAME;
    }
    public String getFORM_NAME()
    {
        return FORM_NAME;
    }
}
```

## プロセス手順を実装するコード

```java
package com.aemforms.dbsamples.core.insertFormData;
import java.io.InputStream;
import java.io.StringWriter;
import java.nio.charset.StandardCharsets;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

import javax.jcr.Node;
import javax.jcr.Session;
import javax.sql.DataSource;

import org.apache.commons.io.IOUtils;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.day.commons.datasource.poolservice.DataSourcePool;

@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=Insert Form Data in Database",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Insert Form Data in Database"
})

public class InsertAfData implements WorkflowProcess {
  @Reference
  InsertFormDataConfigurationService insertFormDataConfig;
  @Reference
  DataSourcePool dataSourcePool;
  private final Logger log = LoggerFactory.getLogger(getClass());
  @Override
  public void execute(WorkItem workItem, WorkflowSession session, MetaDataMap metaDataMap) throws WorkflowException {

    String proccesArgsVals = (String) metaDataMap.get("PROCESS_ARGS", (Object)
      "string");
    String[] values = proccesArgsVals.split(",");
    String AdaptiveFormName = values[0];
    String formDataFile = values[1];
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    Session jcrSession = (Session) session.adaptTo((Class) Session.class);
    String dataFilePath = payloadPath + "/" + formDataFile + "/jcr:content";
    log.debug("The data file path is " + dataFilePath);
    PreparedStatement ps = null;
    Connection con = null;
    DataSource dbSource = null;

    try {
      dbSource = (DataSource) dataSourcePool.getDataSource(insertFormDataConfig.getDATA_SOURCE_NAME());
      log.debug("Got db source");
      con = dbSource.getConnection();

      Node xmlDataNode = jcrSession.getNode(dataFilePath);
      InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
      StringWriter writer = new StringWriter();
      String encoding = StandardCharsets.UTF_8.name();
      IOUtils.copy(xmlDataStream, writer, encoding);
      String queryStmt = "insert into " + insertFormDataConfig.TABLE_NAME + "(" + insertFormDataConfig.COLUMN_NAME + "," + insertFormDataConfig.FORM_NAME + ") values(?,?)";
      log.debug("The query Stmt is " + queryStmt);
      ps = con.prepareStatement(queryStmt);
      ps.setString(1, writer.toString());
      ps.setString(2, AdaptiveFormName);
      ps.executeUpdate();

    } catch (Exception e) {
      log.debug("The error message is " + e.getMessage());
    } finally {
      if (ps != null) {
        try {
          ps.close();
        } catch (SQLException sqlException) {
          log.debug(sqlException.getMessage());
        }
      }
      if (con != null) {
        try {
          con.close();
        } catch (SQLException sqlException) {
          log.error("Unable to close connection to database", sqlException);
        }
      }
    }
  }

}
```

## サンプルアセットのデプロイ

* JDBC 接続プールが設定されていることを確認します
* configMgr を使用してデータベースの詳細を指定します
* [zip ファイルをダウンロードし、その内容をハードドライブに抽出します](assets/article-assets.zip)

   * [AEM web コンソール](http://localhost:4502/system/console/bundles)を使用して jar ファイルをデプロイします。この jar ファイルには、データベースにフォームデータを保存するためのコードが含まれています。

   * [パッケージマネージャーを使用して、AEM に](http://localhost:4502/crx/packmgr/index.jsp) 2 つの zip ファイルを読み込みます。これで、フォーム送信時にワークフローをトリガーする[サンプルワークフロー](http://localhost:4502/editor.html/conf/global/settings/workflow/models/storeformdata.html)と[サンプルアダプティブフォーム](http://localhost:4502/editor.html/content/forms/af/addformdataindb.html)を作成しました。ワークフロー手順のプロセス引数に注意します。これらの引数は、フォーム名と、アダプティブフォームデータを格納するデータファイルの名前を示します。データファイルは、crx リポジトリのペイロードフォルダーに保存されます。送信時に AEM ワークフローをトリガーするための[アダプティブフォーム](http://localhost:4502/editor.html/content/forms/af/addformdataindb.html)の設定方法と、データファイルの設定（data.xml）を確認します。

   * プレビューしてフォームに入力し、送信します。データベースに作成された新しい行が表示されます

