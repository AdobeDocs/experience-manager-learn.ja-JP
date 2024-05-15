---
title: JDBC DataSourcePool を使用した SQL 接続
description: AEM JDBC DataSourcePool とエグレスポートを使用して AEM as a Cloud Serviceから SQL データベースに接続する方法を説明します。
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9355
thumbnail: KT-9355.jpeg
exl-id: c1a26dcb-b2ae-4015-b865-2ce32f4fa869
duration: 117
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '329'
ht-degree: 100%

---

# JDBC DataSourcePool を使用した SQL 接続

SQL データベース（およびその他の非 HTTP／HTTPS サービス）への接続は、AEM からプロキシ化する必要があります。これには、AEM DataSourcePool OSGi サービスを使用して接続を管理する接続も含まれます。

## 高度なネットワーク機能のサポート

次のコードサンプルは、以下の高度なネットワーク機能オプションでサポートされています。

このチュートリアルを開始する前に、高度なネットワーク機能の設定が[適切](../advanced-networking.md#advanced-networking)にセットアップされていることを確認してください。

| 高度なネットワーク機能なし | [柔軟なポートエグレス](../flexible-port-egress.md) | [専用エグレス IP アドレス](../dedicated-egress-ip-address.md) | [仮想プライベートネットワーク](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## OSGi 設定

OSGi 設定の接続文字列には、次の値が使用されます。

+ [OSGi 設定環境変数](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=ja#environment-specific-configuration-values) を介した `AEM_PROXY_HOST` 値（`$[env:AEM_PROXY_HOST;default=proxy.tunnel]` を接続のホストとして使用）
+ `30001`（Cloud Manager のポートフォワードマッピング `30001` → `mysql.example.com:3306` の `portOrig` 値）

シークレットはコードに保存できないので、SQL 接続のユーザー名とパスワードは、OSGi 設定変数、AIO CLI または Cloud Manager API を使用して設定することで最適に提供されます。

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/config/com.day.commons.datasource.jdbcpool.JdbcPoolService~wknd-examples-mysql.cfg.json`

```json
{
  "datasource.name": "wknd-examples-mysql",
  "jdbc.driver.class": "com.mysql.jdbc.Driver",
  "jdbc.connection.uri": "jdbc:mysql://$[env:AEM_PROXY_HOST;default=proxy.tunnel]:30001/wknd-examples",
  "jdbc.username": "$[env:MYSQL_USERNAME;default=mysql-user]",
  "jdbc.password": "$[secret:MYSQL_PASSWORD]"
}
```

以下の `aio CLI` コマンドを使用して、環境ごとに OSGi シークレットを設定できます。

```shell
$ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret MYSQL_USERNAME "mysql-user" --secret MYSQL_PASSWORD "password123"
```

## コードサンプル

この Java™コードの例は、AEM DataSourcePool OSGi サービスを介して外部の MySQL データベースに接続する OSGi サービスです。
次に、DataSourcePool OSGi ファクトリ構成は、[enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration?lang=ja) 操作の `portForwards` ルールによって外部ホストとポート `mysql.example.com:3306` にマッピングされるポート（`30001`）を指定します。

```json
...
"portForwards": [{
    "name": "mysql.example.com",
    "portDest": 3306,
    "portOrig": 30001
}]
...
```

+ `core/src/com/adobe/aem/wknd/examples/connections/impl/JdbcExternalServiceImpl.java`

```java
package com.adobe.aem.wknd.examples.core.connections.impl;

import com.adobe.aem.wknd.examples.core.connections.ExternalService;
import com.day.commons.datasource.poolservice.DataSourceNotFoundException;
import com.day.commons.datasource.poolservice.DataSourcePool;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.SQLException;

@Component
public class JdbcExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(JdbcExternalServiceImpl.class);

    @Reference
    private DataSourcePool dataSourcePool;

    // The datasource.name value of the OSGi configuration containing the connection this OSGi component will use.
    private static final String DATA_SOURCE_NAME = "wknd-examples-mysql";

    @Override
    public boolean isAccessible() {

        try {
            // Get the JDBC data source based on the named OSGi configuration
            DataSource dataSource = (DataSource) dataSourcePool.getDataSource(DATA_SOURCE_NAME);

            // Establish a connection with the external JDBC service
            // Per the OSGi configuration, this will use the injected $[env:AEM_PROXY_HOST] value as the host
            // and the port (30001) mapped via Cloud Manager API call
            try (Connection connection = dataSource.getConnection()) {

                // Validate the connection
                connection.isValid(1000);

                // Close the connection, since this is just a simple connectivity check
                connection.close();

                // Return true if AEM could reach the external JDBC service
                return true;
            } catch (SQLException e) {
                log.error("Unable to validate SQL connection for [ {} ]", DATA_SOURCE_NAME, e);
            }
        } catch (DataSourceNotFoundException e) {
            log.error("Unable to establish an connection with the JDBC data source [ {} ]", DATA_SOURCE_NAME, e);
        }

        return false;
    }
}
```

## MySQL ドライバーの依存関係

AEM as a Cloud Service では、多くの場合、接続をサポートするために Java™ データベースドライバーを提供する必要があります。 ドライバーの提供は、通常、これらのドライバーを含む OSGi バンドルアーティファクトを `all` パッケージ経由で AEM プロジェクトに埋め込むことで達成します。

### pom.xml のリファクタリング

データベースドライバの依存関係をリアクター `pom.xml` に含め、`all` サブプロジェクトで参照します。

+ `pom.xml`

```xml
...
<dependencies>
    ...
    <!-- MySQL Driver dependencies -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>[8.0.27,)</version>
        <scope>provided</scope>
    </dependency>
    ...
</dependencies>
...
```

## すべての pom.xml

データベースドライバーの依存関係アーティファクトを `all` パッケージをAEM as a Cloud Service上にデプロイして使用できるようにします。これらのアーティファクトは&#x200B;__必須__&#x200B;データベースドライバー Java™ クラスを書き出す OSGi バンドルである必要があります。

+ `all/pom.xml`

```xml
...
<embededs>
    ...
    <!-- Include the MySQL Driver OSGi bundles for deployment to the project -->
    <embedded>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <target>/apps/wknd-examples-vendor-packages/application/install</target>
    </embedded>
    ...
</embededs>

...

<dependencies>
    ...
    <!-- Add MySQL OSGi bundle artifacts so the <embeddeds> can add them to the project -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <scope>provided</scope>
    </dependency>
    ...
</dependencies>
...
```
