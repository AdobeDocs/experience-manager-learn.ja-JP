---
title: AEM as a Cloud Serviceでの秘密鍵の管理
description: AEMが提供するツールや手法を使用して機密情報を保護し、アプリケーションのセキュリティと機密性を確保し、AEM as a Cloud Service内で秘密鍵を管理するためのベストプラクティスについて説明します。
version: Cloud Service
topic: Development, Security
feature: OSGI, Cloud Manager
role: Developer
jira: KT-15880
level: Intermediate, Experienced
exl-id: 856b7da4-9ee4-44db-b245-4fdd220e8a4e
source-git-commit: ba744f95f8d1f0b982cd5430860f0cb0945a4cda
workflow-type: tm+mt
source-wordcount: '702'
ht-degree: 0%

---

# AEM as a Cloud Serviceでの秘密鍵の管理

API キーやパスワードなどの秘密鍵の管理は、アプリケーションのセキュリティを維持するために重要です。 Adobe Experience Manager（AEM）as a Cloud Serviceでは、秘密鍵を安全に処理するための堅牢なツールを提供しています。

このチュートリアルでは、AEM内で秘密鍵を管理するためのベストプラクティスについて説明します。 AEMが提供する機密情報を保護するためのツールとテクニックを取り上げ、お客様のアプリケーションの安全性と機密性を確保します。

このチュートリアルでは、AEM Java 開発、OSGi サービス、Sling モデルおよびAdobeCloud Managerに関する十分な知識があることを前提としています。

## シークレットマネージャー OSGi サービス

AEM as a Cloud Serviceでは、OSGi サービスを通じて秘密鍵を管理することで、拡張性の高い安全なアプローチが提供されます。 OSGi サービスは、API キーやパスワードなどの機密性の高い情報（OSGi 設定を通じて定義された情報や、Cloud Managerを介して設定された情報）を処理するように設定できます。

### OSGi サービスの実装

[OSGi 設定からシークレットを公開する ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi#secret-configuration-values) カスタム OSGi サービスの開発について順を追って説明します。

実装では、`@Activate` メソッドを使用して OSGi 設定からシークレットを読み取り、`getSecret(String secretName)` メソッドを使用して公開します。 または、秘密鍵ごとに `getApiKey()` のような個別のメソッドを作成できますが、このアプローチでは秘密鍵が追加または削除されるので、より多くのメンテナンスが必要です。

```java
package com.example.core.util.impl;

import com.example.core.util.SecretsManager;
import org.osgi.service.component.annotations.*;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.apache.sling.api.resource.ValueMap;
import org.apache.sling.api.resource.ValueMapDecorator;
import java.util.Map;

@Component(
    service = { SecretsManager.class }
)
public class SecretsManagerImpl implements SecretsManager {
    private static final Logger log = LoggerFactory.getLogger(SecretsManagerImpl.class);
 
    private ValueMap secrets;

    @Override
    public String getSecret(String secretName) {
        return secrets.get(secretName, String.class);
    }

    @Activate
    @Modified
    protected void activate(Map<String, Object> properties) {
        secrets = new ValueMapDecorator(properties);
    }
}
```

OSGi サービスとしては、Java インターフェイスを介して登録して使用することをお勧めします。 以下は、消費者が OSGi プロパティ名で秘密鍵を取得できるようにするシンプルなインターフェイスです。

```java
package com.example.core.util;

import org.osgi.annotation.versioning.ConsumerType;

@ConsumerType
public interface SecretsManager {
    String getSecret(String secretName);
}
```

## OSGi 設定への秘密鍵のマッピング

OSGi サービスでシークレット値を公開するには、[OSGi シークレット設定値 ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi#secret-configuration-values) を使用して、それらを OSGi 設定にマッピングします。 `SecretsManager.getSecret()` メソッドからシークレット値を取得するためのキーとして、OSGi プロパティ名を定義します。

AEM Maven プロジェクトの OSGi 設定ファイル `/apps/example/osgiconfig/config/com.example.core.util.impl.SecretsManagerImpl.cfg.json`OSGi 設定ファイルでシークレットを定義します。 各プロパティは、AEMで公開された秘密鍵を表し、値はCloud Managerを使用して設定されます。 キーは OSGi プロパティ名で、`SecretsManager` サービスからシークレット値を取得するために使用します。

```json
{
    "api.key": "$[secret:api_key]",
    "service.password": "$[secret:service_password]"
}
```

また、Shared Secrets Manager OSGi サービスを使用する代わりに、シークレットを使用する特定のサービスの OSGi 設定にシークレットを直接含めることもできます。 このアプローチは、シークレットが 1 つの OSGi サービスでのみ必要であり、複数のサービスで共有されない場合に役立ちます。 この場合、シークレットの値は、特定のサービスの OSGi 設定ファイルで定義され、`@Activate` メソッドを介してサービスの Java コードでアクセスできます。

## 秘密鍵の使用

秘密鍵は、Sling モデルや別の OSGi サービスなど、様々な方法で OSGi サービスから使用できます。 以下は、両方から秘密鍵を使用する方法の例です。

### Sling モデルから

Sling モデルは、多くの場合、AEM サイトコンポーネントのビジネスロジックを提供します。 `SecretsManager` OSGi サービスは、`@OsgiService` 注釈を介して使用でき、Sling モデル内で使用してシークレット値を取得できます。

```java
import com.example.core.util.SecretsManager;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.servlets.SlingHttpServletRequest;
import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.OsgiService;

@Model(
    adaptables = {SlingHttpServletRequest.class, Resource.class},
    adapters = {ExampleDatabaseModel.class}
)
public class ExampleDatabaseModelImpl implements ExampleDatabaseModel {

    @OsgiService
    SecretsManager secretsManager;

    @Override 
    public String doWork() {
        final String secret = secretsManager.getSecret("api.key");
        // Do work with secret
    }
}
```

### OSGi サービスから

OSGi サービスは、Sling モデル、ワークフローなどのAEM サービス、その他のカスタム OSGi サービスで使用される、再利用可能なビジネスロジックをAEM内で公開することがよくあります。 `SecretsManager` OSGi サービスは、`@Reference` 注釈を介して使用でき、OSGi サービス内でシークレット値を取得するために使用できます。

```java
import com.example.core.util.SecretsManager;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;

@Component
public class ExampleSecretConsumerImpl implements ExampleSecretConsumer {

    @Reference
    SecretsManager secretsManager;

    public void doWork() {
        final String secret = secretsManager.getSecret("service.password");
        // Do work with the secret
    }
}
```

## Cloud Managerでの秘密鍵の設定

OSGi サービスと設定が整ったら、最後にCloud Managerでシークレットの値を設定します。

シークレットの値は、[Cloud Manager API](https://developer.adobe.com/experience-cloud/cloud-manager/reference/api/#tag/Variables) または一般的には [Cloud Manager UI](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables#overview) を使用して設定できます。 Cloud Manager UI を使用してシークレット変数を適用するには：

![Cloud Manager シークレットの設定 ](./assets/secrets/cloudmanager-configuration.png)

1. [Cloud ManagerのAdobe](https://my.cloudmanager.adobe.com) にログインします。
1. シークレットを設定するAEM プログラムと環境を選択します。
1. 環境の詳細表示で、「**設定**」タブを選択します。
1. 「**追加**」を選択します。
1. 環境設定ダイアログで、次の手順を実行します。
   - OSGi 設定で参照されるシークレットの変数名（`api_key` など）を入力します。
   - シークレットの値を入力します。
   - 秘密鍵を適用するAEM サービスを選択します。
   - タイプとして **秘密鍵** を選択します。
1. 「**追加**」を選択して、秘密鍵を保持します。
1. 必要な数の秘密鍵を追加します。 完了したら、「**保存**」を選択して、変更内容を直ちにAEM環境に適用します。

Cloud Manager設定を秘密鍵に使用すると、AEM アプリケーションを再展開しなくても、異なる環境やサービスに異なる値を適用し、秘密鍵をローテーションできるという利点があります。
