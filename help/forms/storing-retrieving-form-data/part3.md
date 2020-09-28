---
title: MySQLデータベースからのフォームデータの格納と取得
description: フォームデータの格納と取得に関する手順について説明するマルチパートチュートリアル
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 1%

---


# データを取得するOSGIサービスの作成

次のコードは、保存されたアダプティブフォームデータを取得するために記述されています。 単純なクエリを使用して、特定のGUIDに関連付けられたアダプティブフォームデータを取得します。 取り込まれたデータは呼び出し元のアプリケーションに返される。 最初の手順で作成したのと同じデータソースが、このコードで参照されます。


```java
package com.techmarketing.core.impl;

import com.techmarketing.core.AemFormsAndDB;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

@Component(
        service = AemFormsAndDB.class
)
public class AemformWithDB implements AemFormsAndDB {
    private final Logger log = LoggerFactory.getLogger(getClass());
   
    @Reference(target = "(&(objectclass=javax.sql.DataSource)(datasource.name=aemformstutorial))")
    private DataSource dataSource;

    @Override
    public String getData(String guid) {
        log.debug("### inside my getData of AemformWithDB");
        Connection con = getConnection();
        try {
            Statement st = con.createStatement();
            String query = "SELECT afdata FROM aemformstutorial.formdata where guid = '" + guid + "'" + "";
            log.debug(" The query is " + query);
            ResultSet rs = st.executeQuery(query);
            while (rs.next()) {
                return rs.getString("afdata");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return null;
    }

    public Connection getConnection() {
        log.debug("Getting Connection ");
        Connection con = null;
        try {
            con = dataSource.getConnection();
            log.debug("got connection");
            return con;
        } catch (Exception e) {
            log.debug("not able to get connection ");
            e.printStackTrace();
        }
        return null;
    }
}
```

## インターフェイス

次に、使用されたインターフェイス宣言を示します

```java
package com.techmarketing.core;

public interface AemFormsAndDB {
   public String getData(String guid);
}
```
