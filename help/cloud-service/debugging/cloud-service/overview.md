---
title: デバッグAEM as a Cloud Service
description: セルフサービスで拡張性の高いクラウドインフラストラクチャを使用すると、AEMの開発者は、AEMas a Cloud Serviceの様々なファセットを理解し、デバッグする方法を理解し、ビルドとデプロイから、実行中のAEMアプリケーションの詳細の取得に至るまで理解する必要があります。
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
jira: KT-5346
topic: Development
role: Developer
level: Beginner, Intermediate
exl-id: 8092fbb4-234f-472e-a405-8a45734b7c65
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 26%

---

# デバッグAEM as a Cloud Service

AEM as a Cloud Serviceは、AEMアプリケーションのクラウドネイティブな利用方法です。 AEMas a Cloud Serviceは、セルフサービス、拡張性、クラウドインフラストラクチャ上で動作します。AEM開発者は、ビルドとデプロイから、実行中のAEMアプリケーションの詳細の取得に至るまで、AEMas a Cloud Serviceの様々なファセットを理解し、デバッグする方法を理解する必要があります。

## ログ

ログには、AEMでのアプリケーションのas a Cloud Service的な機能の詳細と、デプロイメントの問題に関するインサイトが記録されます。

[ログを使用した AEM as a Cloud Service のデバッグ](./logs.md)

## ビルドとデプロイメント

Adobe Cloud Manager のパイプラインは、AEM as a Cloud Serviceにデプロイした場合にコードの品質と実行可能性を判断するための一連の手順を通じてAEMアプリケーションをデプロイします。 各手順が失敗する可能性があるので、ビルドのデバッグ方法を理解して、の根本原因を特定し、エラーを解決する方法を理解することが重要です。

[AEMのas a Cloud Service的なビルドとデプロイメントのデバッグ](./build-and-deployment.md)

## Developer Console

開発者コンソールは、AEM as a Cloud Serviceでのアプリケーションの認識方法と機能を理解するのに役立つ、様々な情報とAEMas a Cloud Service環境の概要を提供します。

[Developer Console での AEM as a Cloud Service のデバッグ](./developer-console.md)

## リポジトリブラウザー

リポジトリブラウザーは、AEM の基盤となるデータストアを可視化する強力なツールで、AEM as a Cloud Service 環境のデバッグを容易にします。リポジトリブラウザーは、実稼動、ステージングおよび開発時の AEM のリソースとプロパティおよびオーサー、パブリッシュ、プレビューの各サービスの読み取り専用ビューをサポートしています。

[リポジトリブラウザーを使用した AEM as a Cloud Service のデバッグ](./repository-browser.md)
