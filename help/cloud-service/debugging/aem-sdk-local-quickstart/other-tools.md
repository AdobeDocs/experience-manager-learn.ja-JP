---
title: AEM SDKのデバッグ用のその他のツール
description: その他の様々なツールは、AEM SDKのローカルクイックスタートのデバッグに役立ちます。
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
translation-type: tm+mt
source-git-commit: a3d3612713decefb5c7e1cf5b2d4d21afff0a2f5
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 12%

---


# AEM SDKのデバッグ用のその他のツール

AEM SDKのローカルクイックスタートでのアプリケーションのデバッグには、様々なツールが役立ちます。

## CRXDE Lite

![CRXDE Lite](./assets/other-tools/crxde-lite.png)

CRXDE Liteは、JCR、AEMデータリポジトリと対話用のWebベースのインターフェイスです。 CRXDE Liteは、ノード、プロパティ、プロパティ値、権限など、JCRを完全に視認できます。

CRXDE Liteの場所：

+ ツール/一般/CRXDE Lite
+ または [http://localhost:4502/crx/de/index.jspに直接](http://localhost:4502/crx/de/index.jsp)

## クエリの説明を実行

![クエリの説明を実行](./assets/other-tools/explain-query.png)

AEM SDKのローカルクイックスタートで、クエリのWebベースのツールを説明します。AEMがクエリを解釈し実行する方法に関する重要な洞察を提供し、AEMがクエリをパフォーマンスの高い方法で実行するための非常に貴重なツールです。

説明のクエリは次の場所にあります。

+ ツール/診断/クエリのパフォーマンス/「クエリの実行」タブ
+ [http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) /「Explainクエリ」タブ

## QueryBuilderデバッガー

![QueryBuilderデバッガー](./assets/other-tools/query-debugger.png)

QueryBuilderデバッガーは、AEM QueryBuilderの構文を使用して検索クエリをデバッグし、理解するのに役立つWebベースのツールで [す](https://docs.adobe.com/content/help/en/experience-manager-65/developing/platform/query-builder/querybuilder-api.html) 。

QueryBuilderデバッガーは次の場所にあります。

+ [http://localhost:4502/libs/cq/search/content/querydebug.html](http://localhost:4502/libs/cq/search/content/querydebug.html)

## Sling Log TracerおよびAEM Chromeプラグイン

![Sling Log TracerおよびAEM Chromeプラグイン](./assets/other-tools/log-tracer.png)

[AEM SDKのローカルクイックスタートに付属するSling Log Tracer](https://sling.apache.org/documentation/bundles/log-tracers.html)(Sling Log Tracer)は、HTTP要求を詳細にトレースし、要求ごとの詳細なデバッグ情報を公開することを可能にします。 この機能を有効にするには、 [ログトレーサーOSGiの設定を構成する必要があります](https://sling.apache.org/documentation/bundles/log-tracers.html#configuration-1) 。

[Google Chrome Webブラウザー用のオープンソースの](https://chrome.google.com/webstore/detail/aem-chrome-plug-in/ejdcnikffjleeffpigekhccpepplaode?hl=ja-JP) AEM Chromeプラグイン [](https://www.google.com/chrome/)。ログトレーサーと統合されており、Chromeの開発ツールでデバッグ情報が直接公開されます。

_AEM Chromeプラグインはオープンソースのツールで、Adobeはこの機能をサポートしていません。_

