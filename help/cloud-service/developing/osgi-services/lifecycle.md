---
title: OSGi コンポーネントのライフサイクル
description: OSGi サービスをライフサイクルイベント（アクティブ化、変更、非アクティブ化）にバインドする方法など、OSGi コンポーネントのライフサイクルについて説明します。
role: Developer
level: Beginner
topic: Development
feature: OSGI
jira: KT-8228
thumbnail: 335475.jpeg
exl-id: 5a65dbcd-649b-464c-9c78-d31c2b6c49c3
duration: 377
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '58'
ht-degree: 100%

---

# OSGi コンポーネントのライフサイクル

OSGi サービスを以下にバインドする方法など、OSGi コンポーネントのライフサイクルについて説明します。

+ アクティブ化
+ 変更
+ 非アクティブ化

の各ライフサイクルイベント

>[!VIDEO](https://video.tv.adobe.com/v/3417893?quality=12&learn=on&captions=jpn)

## リソース

+ [@Activate JavaDocs](https://javadoc.io/static/com.adobe.aem/aem-sdk-api/2021.7.5658.20210723T140305Z-210600/org/osgi/service/component/annotations/Activate.html)
+ [@Modified JavaDocs](https://javadoc.io/static/com.adobe.aem/aem-sdk-api/2021.7.5658.20210723T140305Z-210600/org/osgi/service/component/annotations/Modified.html)
+ [@Deactivate JavaDocs](https://javadoc.io/static/com.adobe.aem/aem-sdk-api/2021.7.5658.20210723T140305Z-210600/org/osgi/service/component/annotations/Deactivate.html)

## コード

### ActivitiesImpl.java

`/core/src/main/java/com/adobe/aem/wknd/examples/core/adventures/impl/ActivitiesImpl.java`

```java
package com.adobe.aem.wknd.examples.core.adventures.impl;

import java.util.Random;

import com.adobe.aem.wknd.examples.core.adventures.Activities;

import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Deactivate;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(
    service = { Activities.class }
)
public class ActivitiesImpl implements Activities {
    private static final Logger log = LoggerFactory.getLogger(ActivitiesImpl.class);

    private String[] activities;

    private final Random random = new Random();

    /**
     * @return the name of a random WKND adventure activity
     */
    public String getRandomActivity() {
        int randomIndex = random.nextInt(activities.length);
        return activities[randomIndex];
    }    

    @Activate
    protected void activate() {
        this.activities = new String[] { 
            "Running", "Cycling",  "Skateboarding"
        };

        log.info("Activated ActivitiesImpl with activities [ {} ]", String.join(", ", this.activities));
    }

    @Deactivate
    protected void deactivate() {
        log.info("ActivitiesImpl has been deactivated!");
    }
}
```
