---
title: OSGi Webコンソールを使用したAEM SDKのデバッグ
description: AEM SDKのローカルクイックスタートにはOSGi Webコンソールがあり、このコンソールは、アプリケーションがどのように認識され、AEM内で機能するかを理解するのに役立つ、様々な情報と、ローカルAEMランタイムの内省を提供します。
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5265, 5366, 5267
translation-type: tm+mt
source-git-commit: a3d3612713decefb5c7e1cf5b2d4d21afff0a2f5
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 4%

---


# OSGi Webコンソールを使用したAEM SDKのデバッグ

AEM SDKのローカルクイックスタートにはOSGi Webコンソールがあり、このコンソールは、アプリケーションがどのように認識され、AEM内で機能するかを理解するのに役立つ、様々な情報と、ローカルAEMランタイムの内省を提供します。

AEMは多くのOSGiコンソールを提供し、それぞれがAEMのさまざまな側面に対する主要なインサイトを提供します。ただし、通常、以下はアプリケーションのデバッグに最も役立ちます。

## バンドル

>[!VIDEO](https://video.tv.adobe.com/v/34335/?quality=12&learn=on)

バンドルコンソールは、OSGiバンドルとその詳細をAEMにデプロイしたカタログです。また、バンドルの開始と停止のアドホック機能も備えています。

Bundlesコンソールは次の場所にあります。

+ ツール/操作/Webコンソール/OSGi/バンドル
+ Or directly at: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

各バンドルをクリックすると、アプリケーションのデバッグに役立つ詳細が表示されます。

+ OSGiバンドルが存在することを確認しています
+ OSGiバンドルがアクティブかどうかの検証
+ OSGiバンドルが未満のインポートを満たしているかどうかを確認中、インポートを開始できませんでした

## コンポーネント

>[!VIDEO](https://video.tv.adobe.com/v/34336/?quality=12&learn=on)

コンポーネントコンソールは、AEMにデプロイされたすべてのOSGiコンポーネントのカタログで、定義済みのOSGiコンポーネントのライフサイクルから、参照するOSGiサービスに関するすべての情報を提供します

コンポーネントコンソールは次の場所にあります。

+ ツール/操作/Webコンソール/OSGi/コンポーネント
+ Or directly at: [http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

デバッグアクティビティに役立つ主要な側面：

+ OSGiバンドルが存在することを確認しています
+ OSGiバンドルがアクティブかどうかの検証
+ OSGiバンドルが未満のインポートを満たしているかどうかを確認中、インポートを開始できませんでした
+ コンポーネントのPIDの取得（GitでコンポーネントのOSGi設定を作成する場合）
+ アクティブなOSGi構成にバインドされたOSGiプロパティ値の識別

## Sling Model

>[!VIDEO](https://video.tv.adobe.com/v/34337/?quality=12&learn=on)

Slingモデルコンソールは、次の場所にあります。

+ ツール/操作/Webコンソール/ステータス/Slingモデル
+ Or directly at: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

デバッグアクティビティに役立つ主要な側面：

+ Slingモデルが正しいリソースの種類に登録されていることを確認しています
+ Slingモデルの検証は、適切なオブジェクト（ResourceまたはSlingHttpRequestServlet）から適応可能です
+ Slingモデルエクスポーターが正しく登録されていることを確認しています
