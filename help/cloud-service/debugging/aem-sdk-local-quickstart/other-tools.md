---
title: AEM SDKのデバッグ用のその他のツール
description: その他の様々なツールは、AEM SDKのローカルクイックスタートのデバッグに役立ちます。
feature: 開発者ツール
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
topic: 開発
role: Developer
level: Beginner, Intermediate
translation-type: tm+mt
source-git-commit: 5fcc7eec120debf1a8ac08716154599467e66759
workflow-type: tm+mt
source-wordcount: '222'
ht-degree: 13%

---


# AEM SDKのデバッグ用のその他のツール

AEM SDKのローカルクイックスタートでのアプリケーションのデバッグには、様々なツールが役立ちます。

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

CRXDE Liteは、JCR、AEMデータリポジトリと対話用のWebベースのインターフェイスです。 CRXDE Liteは、ノード、プロパティ、プロパティ値、権限など、JCRを完全に視認できます。

CRXDE Liteの場所：

+ ツール/一般/CRXDE Lite
+ または[http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)に直接

## クエリの説明を実行

![クエリの説明を実行](./assets/other-tools/explain-query.png)

AEM SDKのローカルクイックスタートで、クエリのWebベースのツールを説明します。AEMがクエリを解釈し実行する方法に関する重要な洞察を提供し、AEMがクエリをパフォーマンスの高い方法で実行するための非常に貴重なツールです。

説明のクエリは次の場所にあります。

+ ツール/診断/クエリのパフォーマンス/「クエリの実行」タブ
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) /「Explainクエリ」タブ

## QueryBuilderデバッガー

![QueryBuilderデバッガー](./assets/other-tools/query-debugger.png)

QueryBuilderデバッガーは、AEM [QueryBuilder](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/query-builder/querybuilder-api.html)構文を使用した検索クエリのデバッグと理解に役立つWebベースのツールです。

QueryBuilderデバッガーは次の場所にあります。

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)

