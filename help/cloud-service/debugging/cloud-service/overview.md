---
title: AEM as a Cloud Service のデバッグ
description: 拡張性の高いセルフサービスのクラウドインフラストラクチャ上で動作するので、AEM 開発者はビルドとデプロイから、実行中の AEM アプリケーションの詳細の取得に至るまで、AEM as a Cloud Service の様々な側面を把握しデバッグする方法を理解する必要があります。
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5346
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 8092fbb4-234f-472e-a405-8a45734b7c65
duration: 60
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '314'
ht-degree: 100%

---

# AEM as a Cloud Service のデバッグ

AEM as a Cloud Service によって、AEM アプリケーションをクラウドネイティブな方法で利用できます。AEM as a Cloud Service は、拡張性の高いセルフサービスのクラウドインフラストラクチャ上で動作します。AEM 開発者は、ビルドとデプロイから、実行中の AEM アプリケーションの詳細の取得に至るまで、AEM as a Cloud Service の様々な側面を把握しデバッグする方法を理解する必要があります。

## ログ

ログには、AEM as a Cloud Service でアプリケーションがどのように機能しているかについての詳細と、デプロイメントの問題に関するインサイトが記録されます。

[ログを使用した AEM as a Cloud Service のデバッグ](./logs.md)

## ビルドとデプロイメント

Adobe Cloud Manager パイプラインは、一連のステップを通じて AEM アプリケーションをデプロイし、AEM as a Cloud Service にデプロイする際のコードの品質と実行可能性を判断します。各ステップは失敗する可能性があるので、エラーの根本原因と解決方法を特定するためにビルドのデバッグ方法を理解することが重要です。

[AEM as a Cloud Service のビルドとデプロイメントのデバッグ](./build-and-deployment.md)

## Developer Console

Developer Console は、AEM as a Cloud Service でアプリケーションがどのように認識され機能するかを理解するのに役立つ、AEM as a Cloud Service 環境に関する様々な情報とイントロスペクションを提供します。

[Developer Console での AEM as a Cloud Service のデバッグ](./developer-console.md)

## リポジトリブラウザー

リポジトリブラウザーは、AEM の基盤となるデータストアを可視化する強力なツールで、AEM as a Cloud Service 環境のデバッグを容易にします。リポジトリブラウザーは、実稼動、ステージングおよび開発時の AEM のリソースとプロパティおよびオーサー、パブリッシュ、プレビューの各サービスの読み取り専用ビューをサポートしています。

[リポジトリブラウザーを使用した AEM as a Cloud Service のデバッグ](./repository-browser.md)
