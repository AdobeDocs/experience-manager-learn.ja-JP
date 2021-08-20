---
title: サービスインターフェイスの作成
description: インターフェイスで、公開するメソッドを定義します
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: 開発
thumbnail: 7825.jpg
kt: 7825
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '24'
ht-degree: 8%

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


}
```
