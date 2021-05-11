---
title: サービスインターフェイスの作成
description: 公開するインターフェイスのメソッドを定義します
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: 開発
thumbnail: 331891.jpg
kt: 7192
translation-type: tm+mt
source-git-commit: dbc0a35ae96594fec1e10f411d57d2a3812c1cf2
workflow-type: tm+mt
source-wordcount: '24'
ht-degree: 8%

---

# インターフェイス

次の2つのメソッド定義を使用してインターフェイスを作成します。

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
