---
title: OSGi Webコンソールを使用したAEM SDKのデバッグ
description: AEM SDKのローカルクイックスタートには、OSGi Webコンソールがあり、ローカルAEMランタイムに関する様々な情報とイントロスペクションを提供します。これは、AEMでのアプリケーションの認識方法と、内の機能を理解するのに役立ちます。
feature: 開発者ツール
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5265, 5366, 5267
topic: 開発
role: Developer
level: Beginner, Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '396'
ht-degree: 5%

---


# OSGi Webコンソールを使用したAEM SDKのデバッグ

AEM SDKのローカルクイックスタートには、OSGi Webコンソールがあり、ローカルAEMランタイムに関する様々な情報とイントロスペクションを提供します。これは、AEMでのアプリケーションの認識方法と、内の機能を理解するのに役立ちます。

AEMは多くのOSGiコンソールを提供し、それぞれがAEMの様々な側面に関する重要なインサイトを提供します。ただし、通常、アプリケーションのデバッグで最も役立つのは次のとおりです。

## バンドル

>[!VIDEO](https://video.tv.adobe.com/v/34335/?quality=12&learn=on)

バンドルコンソールは、OSGiバンドルとその詳細をAEMにデプロイしたカタログです。また、バンドルを開始および停止するアドホック機能も備えています。

バンドルコンソールは、次の場所にあります。

+ ツール/操作/ Webコンソール/OSGi/バンドル
+ または、次の場所に直接アクセスします。[http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

各バンドルをクリックすると、アプリケーションのデバッグに役立つ詳細が表示されます。

+ OSGiバンドルが存在することを確認しています
+ OSGiバンドルがアクティブかどうかの検証
+ OSGiバンドルが未満の読み込みを行ったかどうかを判断し、読み込みを開始できない

## コンポーネント

>[!VIDEO](https://video.tv.adobe.com/v/34336/?quality=12&learn=on)

コンポーネントコンソールは、AEMにデプロイされたすべてのOSGiコンポーネントのカタログで、定義済みのOSGiコンポーネントのライフサイクルから参照可能なOSGiサービスに至るまで、それらに関するすべての情報を提供します

コンポーネントコンソールは、次の場所にあります。

+ ツール/操作/ Webコンソール/OSGi/コンポーネント
+ または、次の場所に直接アクセスします。[http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

デバッグアクティビティに役立つ主な側面：

+ OSGiバンドルが存在することを確認しています
+ OSGiバンドルがアクティブかどうかの検証
+ OSGiバンドルが未満の読み込みを行ったかどうかを判断し、読み込みを開始できない
+ コンポーネントのPIDを取得し、GitでOSGi設定を作成する
+ アクティブなOSGi設定にバインドされたOSGiプロパティ値の識別

## Sling Model

>[!VIDEO](https://video.tv.adobe.com/v/34337/?quality=12&learn=on)

Sling Modelsコンソールは次の場所にあります。

+ ツール/操作/ Webコンソール/ステータス/ Slingモデル
+ または、次の場所に直接アクセスします。[http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

デバッグアクティビティに役立つ主な側面：

+ Slingモデルが適切なリソースタイプに登録されていることを確認する
+ Slingモデルの検証は、正しいオブジェクト（ResourceまたはSlingHttpRequestServlet）から適応可能です。
+ Sling Model Exporterが正しく登録されていることを確認します
