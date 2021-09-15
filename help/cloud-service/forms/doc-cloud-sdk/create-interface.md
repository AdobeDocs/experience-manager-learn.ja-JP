---
title: サービスインターフェイスの作成
description: インターフェイスで、公開するメソッドを定義します
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
thumbnail: 7825.jpg
kt: 7825
exl-id: f262013b-aaf1-43d1-84b8-6173942c3415
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '23'
ht-degree: 4%

---


# インターフェイス

次の2つのメソッド定義を持つインターフェイスを作成します。

```java
package com.aemforms.doccloud.core;

import java.io.InputStream;

import com.adobe.aemfd.docmanager.Document;

public interface DocumentCloudSDKService {	
	public Document getPDF(String location,String accessToken,String fileName);
	
    public Document createPDFFromInputStream(InputStream is,String fileName);
}
```
