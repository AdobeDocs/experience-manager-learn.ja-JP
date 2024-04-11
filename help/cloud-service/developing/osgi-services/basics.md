---
title: OSGi サービスの開発の基本
description: OSGi サービスの開発の基本について説明します
role: Developer
level: Beginner
topic: Development
feature: OSGI
jira: KT-8227
thumbnail: 335476.jpeg
last-substantial-update: 2022-09-16T00:00:00Z
exl-id: a3a9bf59-e9a2-4322-ac93-9c12c70b9a75
duration: 492
source-git-commit: a18bf2c8b57eaaac3686a26fa1fb39e6fc075af5
workflow-type: tm+mt
source-wordcount: '122'
ht-degree: 46%

---

# OSGi サービス

次のような OSGi サービスの開発の基本について説明します。

+ Java POJO を OSGi サービスに変換する方法
+ OSGi サービスを Java インターフェイスに連結する方法

>[!VIDEO](https://video.tv.adobe.com/v/335476?quality=12&learn=on)

## リソース

+ [@Component JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/org/osgi/service/component/annotations/Component.htmll?lang=ja)
+ [@ProviderType JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/org/osgi/annotation/versioning/ProviderType.html)
+ [@Version JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/org/osgi/annotation/versioning/Version.html)

## コード

### Activities.java

`/core/src/main/java/com/adobe/aem/wknd/examples/core/adventures/Activities.java`

```java
package com.adobe.aem.wknd.examples.core.adventures;

import org.osgi.annotation.versioning.ProviderType;

@ProviderType
public interface Activities {    
    String getRandomActivity();
}
```

### ActivitiesImpl.java

`/core/src/main/java/com/adobe/aem/wknd/examples/core/adventures/impl/ActivitiesImpl.java`

```java
package com.adobe.aem.wknd.examples.core.adventures.impl;

import java.util.Random;

import com.adobe.aem.wknd.examples.core.adventures.Activities;

import org.osgi.service.component.annotations.Component;

@Component(
    service = { Activities.class }
)
public class ActivitiesImpl implements Activities {

    private static final String[] ACTIVITIES = new String[] { 
        "Camping", "Skiing",  "Skateboarding"
    };

    //private final int randomIndex = new Random().nextInt(ACTIVITIES.length);
    private final Random random = new Random();

    /**
     * @return the name of a random WKND adventure activity
     */
    public String getRandomActivity() {
        int randomIndex = random.nextInt(ACTIVITIES.length);
        return ACTIVITIES[randomIndex];
    }    
}
```

### package-info.java

`/core/src/main/java/com/adobe/aem/wknd/examples/core/adventures/package-info.java`

```java
@Version("1.0")
package com.adobe.aem.wknd.examples.core.adventures;

import org.osgi.annotation.versioning.Version;
```

の追加 `package-info.java` は、AEMの他の OSGi バンドルが OSGi サービスインターフェイス（または任意の Java クラス）を解決できるようにするために必要です。 次の場合 `package-info.java` が見つからない場合、Java パッケージとその Java インターフェイスまたはクラスは書き出されません。 これらの Java インターフェイスまたはクラスをこの Java パッケージから読み込もうとする他の OSGi バンドルは、メッセージでエラーが発生します。 __解決できません__ （AEM OSGi バンドルコンソール）。
