---
title: カスタム OSGi 設定の作成
description: OSGi 設定を使用して Azure Portal の設定を指定します。
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager 6.5
topic: Integrations
jira: KT-14794
exl-id: a90267b3-2eb2-4731-9cc9-e04c48979a1a
duration: 39
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '70'
ht-degree: 100%

---

# カスタム OSGi 設定の作成

Azure ストレージ URI と SAS トークン URI を指定するために、Azure Portal 設定と呼ばれるカスタム OSGi 設定が作成されました。これら 2 つの値は、Azure ストレージと通信するための REST API の構築に使用されます。

![azure-portal-configuration](assets/azure-portal-configuration.png)

設定サービスの完全なコードを以下に示します。

AzurePortalConfiguration

```java
package com.azure.portal.configuration;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;

@ObjectClassDefinition(name = "Azure Portal Configuration", description = "Settings for connecting to Azure Portal")

public @interface AzurePortalConfiguration {

    @AttributeDefinition(name = "SAS Token", description = "Enter the SAS Token")
    String getSASToken() default "";

    @AttributeDefinition(name = "Enter the Storage URI", description = "Enter the Storage URI")
    String getStorageURI() default"";

}
```

AzurePortalConfigurationService

```java
package com.azure.portal.configuration.service;

public interface AzurePortalConfigurationService {
    String getStorageURI();
    String getSASToken();

}
```

AzurePortalStorageConfigImpl

```java
package com.azure.portal.configuration.service.impl;

import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;
import com.azure.portal.configuration.service.AzurePortalConfigurationService;
import com.azure.portal.configuration.AzurePortalConfiguration;
@Component(
        service = AzurePortalConfigurationService.class,
        immediate = true,
        property = {
                Constants.SERVICE_ID+ "=Azure Portal Config Service",
                Constants.SERVICE_DESCRIPTION +"=This service reads values from Azure portal config"
        }
)
@Designate(ocd=AzurePortalConfiguration.class)
public class AzurePortalStorageConfigImpl implements AzurePortalConfigurationService {
    private AzurePortalConfiguration azurePortalConfiguration;
    @Activate
    protected void activate(AzurePortalConfiguration configuration) {
        System.out.println("##### in activate #####");
        this.azurePortalConfiguration = configuration;
    }

    @Override
    public String getStorageURI() {
        // TODO Auto-generated method stub
        return azurePortalConfiguration.getStorageURI();
    }

    @Override
    public String getSASToken() {
        // TODO Auto-generated method stub
        return azurePortalConfiguration.getSASToken();
    }

}
```

## 次の手順

[事前入力サービスの記述](./create-prefill-service.md)
