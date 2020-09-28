---
title: Cloud ServiceとしてのAEMのデバッグ
description: セルフサービスで拡張性の高いクラウドインフラストラクチャに基づき、AEM開発者は、AEMの様々なファセットをCloud Serviceとして理解し、デバッグする方法を理解し、構築とデプロイから、実行中のAEMアプリケーションの詳細を入手する必要があります。
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5346
translation-type: tm+mt
source-git-commit: debb65a218ae0b9e5105c3f63018358902330b34
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 1%

---


# Cloud ServiceとしてのAEMのデバッグ

AEM asCloud Serviceは、AEMアプリケーションをクラウドでネイティブに活用する方法です。 AEMは、セルフサービスで拡張性の高いクラウドインフラストラクチャ上で動作します。これには、AEM開発者が、AEMの様々なファセットをCloud Serviceとして理解し、デバッグする方法を理解し、構築と展開から、AEMアプリケーションの詳細を入手する必要があります。

## ログ

ログには、アプリケーションがAEMでCloud Serviceとして機能している方法の詳細や、デプロイメントに関する問題に関するインサイトが記録されます。

[ログを使用したCloud ServiceとしてのAEMのデバッグ](./logs.md)

## 構築と導入

AdobeCloud Managerのパイプラインにより、AEMにCloud Serviceとしてデプロイする場合のコードの品質と実行可能性を決定する一連の手順に従ってAEMアプリケーションがデプロイされます。 各手順は失敗を引き起こす可能性があり、ビルドのデバッグ方法を理解して、の根本原因を特定し、失敗を解決する方法を理解することが重要です。

[Cloud Serviceの構築と展開としてのAEMのデバッグ](./build-and-deployment.md)

## 開発者コンソール

開発者コンソールは、AEMに対して様々な情報やイントロスペクションを提供します。これらの情報は、Cloud Service環境がアプリケーションを認識する方法を理解するのに役立ち、AEM内でCloud Serviceとして機能します。

[Developer ConsoleでのCloud ServiceとしてのAEMのデバッグ](./developer-console.md)

## CRXDE Lite

CRXDE Liteは、AEM開発環境としてCloud Serviceをデバッグするための従来の強力なツールです。 CRXDE Liteは、すべてのリソースとプロパティの検査、JCRの可変部分の操作、権限の調査、クエリの評価からデバッグを支援する一連の機能を提供します。

[CRXDE Liteを使用したCloud ServiceとしてのAEMのデバッグ](./crxde-lite.md)
