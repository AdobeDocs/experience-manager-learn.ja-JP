---
title: OSGi web コンソールを使用した AEM SDK のデバッグ
description: AEM SDK のローカルクイックスタートには OSGi web コンソールがあり、AEM でアプリケーションがどう認識され AEM 内でどう機能するかを理解するのに役立つ様々な情報やイントロスペクションをローカルの AEM ランタイムに提供します。
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
kt: 5265, 5366, 5267
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 0929bc1a-376c-4e16-a540-a276fd5af164
duration: 486
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '369'
ht-degree: 100%

---

# OSGi web コンソールを使用した AEM SDK のデバッグ

AEM SDK のローカルクイックスタートには OSGi web コンソールがあり、AEM でアプリケーションがどう認識され AEM 内でどう機能するかを理解するのに役立つ様々な情報やイントロスペクションをローカルの AEM ランタイムに提供します。

AEM には多くの OSGi コンソールが用意されており、それぞれが AEM の様々な側面について重要な洞察を与えてくれますが、アプリケーションのデバッグに最も役に立つのは一般に次のものです。

## バンドル

>[!VIDEO](https://video.tv.adobe.com/v/38128?quality=12&learn=on&captions=jpn)

Bundles コンソールは、AEM にデプロイされた OSGi バンドルとその詳細のカタログです。バンドルを開始および停止するアドホック機能も備えています。

Bundles コンソールは次の場所にあります。

+ ツール／操作／Web コンソール／OSGi／バンドル
+ または [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles) に直接アクセスします。

各バンドルをクリックすると、アプリケーションのデバッグに役立つ詳細情報が表示されます。

+ OSGi バンドルが存在することを検証します
+ OSGi バンドルがアクティブかどうかを検証します
+ OSGi バンドルの起動を妨げる不十分な読み込みがないか確認します

## コンポーネント

>[!VIDEO](https://video.tv.adobe.com/v/38127?quality=12&learn=on&captions=jpn)

Components コンソールは、AEM にデプロイされたすべての OSGi コンポーネントのカタログであり、OSGi コンポーネントのライフサイクル定義から、参照先となる OSGi サービスに至るまで、コンポーネントに関するすべての情報を提供します。

Components コンソールは次の場所にあります。

+ ツール／操作／Web コンソール／OSGi／コンポーネント
+ または [http://localhost:4502/system/console/components](http://localhost:4502/system/console/components) に直接アクセスします。

デバッグ活動に役立つ重要な側面：

+ OSGi バンドルが存在することを検証します
+ OSGi バンドルがアクティブかどうかを検証します
+ OSGi バンドルの起動を妨げる不十分な読み込みがないか確認します
+ Git でコンポーネントの OSGi 設定を作成するために、コンポーネントの PID を取得します
+ アクティブな OSGi 設定にバインドされた OSGi プロパティ値を特定します

## Sling モデル

>[!VIDEO](https://video.tv.adobe.com/v/38126?quality=12&learn=on&captions=jpn)

Sling モデルコンソールは次の場所にあります。

+ ツール／操作／Web コンソール／ステータス／Sling モデル
+ または [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels) に直接アクセスします

デバッグ活動に役立つ重要な側面：

+ Sling モデルが適切なリソースタイプに登録されていることを検証します
+ Sling モデルが正しいオブジェクト（リソースまたは SlingHttpRequestServlet）から適応可能であることを検証します
+ Sling Model Exporter が適切に登録されていることを検証します
