---
title: AEMをCloud Serviceとしてデバッグ
description: をセルフサービスで拡張可能なクラウドインフラストラクチャに基づいて構築し、デプロイから実行中のAEMアプリケーションの詳細の取得に至るまで、AEM開発者は、Cloud ServiceとしてのAEMの様々なファセットを理解し、デバッグする方法を理解する必要があります。
feature: 開発者ツール
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5346
topic: 開発
role: Developer
level: Beginner, Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '311'
ht-degree: 2%

---


# AEMをCloud Serviceとしてデバッグ

AEM as a Cloud Serviceは、AEMアプリケーションを利用するクラウドネイティブな方法です。 AEM as aCloud Serviceは、セルフサービスで拡張性の高いクラウドインフラストラクチャ上で動作します。AEM開発者は、ビルドとデプロイから実行中のAEMアプリケーションの詳細の取得に至るまで、Cloud ServiceとしてのAEMの様々なファセットを理解し、デバッグする方法を理解する必要があります。

## ログ

ログには、AEM as aCloud Serviceでのアプリケーションの機能に関する詳細と、デプロイメントの問題に関するインサイトが記録されます。

[ログを使用したCloud ServiceとしてのAEMのデバッグ](./logs.md)

## ビルドとデプロイメント

Cloud Managerのパイプラインは、AEMにCloud Serviceとしてデプロイした場合のコード品質と実行可能性を判断するための一連の手順を通じてAEMアプリケーションをデプロイします。 各手順が失敗する可能性があるので、ビルドのデバッグ方法を理解して、の根本原因を特定し、エラーを解決する方法を理解することが重要です。

[AEM as aCloud Serviceビルドとデプロイメントのデバッグ](./build-and-deployment.md)

## デベロッパーコンソール

開発者コンソールは、AEMに対する様々な情報とイントロスペクションをCloud Service環境として提供します。この環境は、AEMでのアプリケーションの認識方法と、内でのCloud Serviceとしての機能を理解するのに役立ちます。

[開発者コンソールを使用したCloud ServiceとしてのAEMのデバッグ](./developer-console.md)

## CRXDE Lite

CRXDE Liteは、AEMをCloud Service開発環境としてデバッグするための、従来の強力なツールです。 CRXDE Liteは、すべてのリソースとプロパティの検査、JCRの可変部分の操作、権限の調査、クエリの評価を行うデバッグを支援する一連の機能を提供します。

[CRXDE Liteを使用したCloud ServiceとしてのAEMのデバッグ](./crxde-lite.md)
