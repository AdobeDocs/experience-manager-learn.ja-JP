---
title: カスタム OSGi 設定の作成
description: Document Cloud 固有の詳細を取得するためのカスタム OSGi 設定
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
thumbnail: 7818.jpg
jira: KT-7818
exl-id: 1f34c356-6c0c-46ff-9cea-7baacfc4bb7f
duration: 22
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '63'
ht-degree: 100%

---

# はじめに

カスタム OSGi 設定を作成して、Document Cloud アカウントの資格情報を取得します。


カスタム OSGi 設定を作成するには、設定のフィールドを表すパブリックメソッドを持つインターフェイスをまず作成する必要があります。

![Document Cloud 設定](assets/doc-cloud-configuration.JPG)


DocumentCloudConfiguration という名前のインターフェイスを作成し、そこに次のコードをペーストします。

```java
package com.aemforms.doccloud.core;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;

@ObjectClassDefinition(name="Document Cloud Service Configuration", description = "Connect AEM Forms With Document Cloud")
public @interface DocumentCloudConfiguration {
	  @AttributeDefinition(name="Client ID", description="Client ID")
	  String getClientID() default "";
	  
	  @AttributeDefinition(name="Client Secret", description="Client Secret")
	  public String getClientSecret() default "26dc4de1-f7f0-46b6-9e8e-86270ad34f58";
	  
	  @AttributeDefinition(name="Technical Account ID", description="Technical Account ID")
	  public String getTechnicalAccount() default "42FF05A7606F71BB0A495FBE@techacct.adobe.com";

	  @AttributeDefinition(name="Organization ID", description="Organization ID")
	  String getOrganizationID() default "299805FF5FC9199D0A495EBC@AdobeOrg";
	  
	  @AttributeDefinition(name="Metascope", description="Metascope")
	  String getMetascope() default "ent_documentcloud_sdk";


}
```
