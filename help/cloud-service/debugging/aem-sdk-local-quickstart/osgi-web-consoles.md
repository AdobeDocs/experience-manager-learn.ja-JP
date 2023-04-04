---
title: OSGi Web コンソールを使用したAEM SDK のデバッグ
description: AEM SDK のローカルクイックスタートには、OSGi Web コンソールがあり、ローカルAEMランタイムに関する様々な情報とイントロスペクションを提供します。これは、AEMでのアプリケーションの認識方法と関数を理解するのに役立ちます。
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5265, 5366, 5267
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 0929bc1a-376c-4e16-a540-a276fd5af164
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 4%

---

# OSGi Web コンソールを使用したAEM SDK のデバッグ

AEM SDK のローカルクイックスタートには、OSGi Web コンソールがあり、ローカルAEMランタイムに関する様々な情報とイントロスペクションを提供します。これは、AEMでのアプリケーションの認識方法と関数を理解するのに役立ちます。

AEMは多くの OSGi コンソールを提供し、それぞれがAEMの様々な側面に関する重要なインサイトを提供します。ただし、通常、アプリケーションのデバッグに最も役立つのは次のとおりです。

## バンドル

>[!VIDEO](https://video.tv.adobe.com/v/34335?quality=12&learn=on)

バンドルコンソールは、AEMにデプロイされた OSGi バンドルとその詳細のカタログです。また、バンドルを開始および停止するアドホック機能も備えています。

バンドルコンソールは次の場所にあります。

+ ツール/操作/ Web コンソール/ OSGi /バンドル
+ または直接アクセス： [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)

各バンドルをクリックすると、アプリケーションのデバッグに役立つ詳細が表示されます。

+ OSGi バンドルが存在することを検証しています
+ OSGi バンドルがアクティブかどうかを検証しています
+ OSGi バンドルが満たされていないインポートを開始できないかどうかの判断

## コンポーネント

>[!VIDEO](https://video.tv.adobe.com/v/34336?quality=12&learn=on)

コンポーネントコンソールは、AEMにデプロイされたすべての OSGi コンポーネントのカタログで、定義された OSGi コンポーネントのライフサイクルから参照可能な OSGi サービスに至るまで、それらに関するすべての情報を提供します

コンポーネントコンソールは次の場所にあります。

+ ツール/操作/ Web コンソール/ OSGi /コンポーネント
+ または直接アクセス： [http://localhost:4502/system/console/components](http://localhost:4502/system/console/components)

デバッグアクティビティに役立つ主な要素：

+ OSGi バンドルが存在することを検証しています
+ OSGi バンドルがアクティブかどうかを検証しています
+ OSGi バンドルが満たされていないインポートを開始できないかどうかの判断
+ コンポーネントの PID を取得して、Git でコンポーネントの OSGi 設定を作成します。
+ アクティブな OSGi 設定にバインドされた OSGi プロパティ値の識別

## Sling Model

>[!VIDEO](https://video.tv.adobe.com/v/34337?quality=12&learn=on)

Sling Models コンソールは次の場所にあります。

+ ツール/操作/ Web コンソール/ステータス/ Sling モデル
+ または直接アクセス： [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels)

デバッグアクティビティに役立つ主な要素：

+ Sling モデルが適切なリソースタイプに登録されていることを確認します
+ Sling モデルの検証は、正しいオブジェクト（Resource または SlingHttpRequestServlet）から適応可能です
+ Sling Model Exporter が正しく登録されていることを検証しています
