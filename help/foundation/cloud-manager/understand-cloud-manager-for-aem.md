---
title: Adobe Cloud Manager について
description: Adobe Cloud Manager は、AEM 環境の管理、内観、セルフサービスを容易に行える、シンプルで堅牢なソリューションを提供します。
sub-product: Experience Manager Cloud Manager, Experience Manager
doc-type: Feature Video
topic: Architecture
feature: Cloud Manager
role: Developer
level: Beginner
exl-id: 53279cbb-70c8-4319-b5bb-9a7d350a7f72
last-substantial-update: 2022-05-10T00:00:00Z
thumbnail: understand-cloud-manager.jpg
duration: 1011
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '464'
ht-degree: 100%

---

# Adobe Cloud Manager について

Adobe Cloud Manager は、AEM 環境の管理、内観、セルフサービスを容易に行える、シンプルで堅牢なソリューションを提供します。

## Cloud Manager の概要

このビデオシリーズでは、AEM 向け Cloud Manager の主な機能を紹介します。

* [プログラム](#programs)
* [環境](#environments)
* [レポート](#reports)
* [CI/CD 実稼動パイプライン](#cicd-production-pipeline)
* [CI/CD 実稼動以外のパイプライン](#cicd-non-production-pipeline)
* [アクティビティ](#activity)

概要については、[Cloud Manager ユーザーガイド](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html?lang=ja)を参照してください。

## プログラム {#programs}

[Cloud Manager プログラム](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/getting-started/program-setup.html?lang=ja)は、ビジネスイニシアチブの論理セットをサポートする AEM 環境セットであり、通常、購入したサービスレベル契約（SLA）に対応しています。例えば、あるプログラムが、グローバルなパブリック web サイトをサポートする AEM リソースを表す一方で、別のプログラムは社内の中核的 DAM を表します。

>[!VIDEO](https://video.tv.adobe.com/v/26313?quality=12&learn=on)

## 環境 {#environments}

[Cloud Manager 環境](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/managing-environments.html?lang=ja)は、AEM オーサー、AEM パブリッシュおよび Dispatcher インスタンスで構成されます。様々な環境がどのように役割をサポートし、様々な CI/CD Pipeline を使用して関与できるかについて説明します。Cloud Manager 環境には通常、本番環境とステージング環境がそれぞれ 1 つあります。

>[!VIDEO](https://video.tv.adobe.com/v/26318?quality=12&learn=on)

## レポート {#reports}

[Cloud Manager レポート](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/monitoring-environments.html?lang=ja)では、各 AEM インスタンスの様々な指標をレポートおよび追跡する一連のグラフを通じて、プログラムの環境と AEM インスタンスを確認できます。

>[!VIDEO](https://video.tv.adobe.com/v/26315?quality=12&learn=on)

## CI/CD 実稼動パイプライン {#cicd-production-pipeline}

*[Adobe Cloud Manager の CI/CD Pipeline ](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) のビデオシリーズでは、本番稼動パイプライン実行について詳しく説明します。失敗したデプロイや成功したデプロイの説明も含まれています。*

>[!NOTE]
>
> これらの動画では、ビルド、テスト、デプロイに要する時間をスピードアップし、視聴時間を短縮しました。完全なパイプライン実行には通常 45 分以上かかります（必須の 30 分のパフォーマンステストを含む）。プロジェクトのサイズ、AEM インスタンスの数、UAT プロセスの数に応じて異なります。

### 設定

[CI/CD 実稼動パイプライン](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html?lang=ja)の設定では、パイプラインを開始するトリガー、実稼動環境のデプロイを制御するパラメーター、およびテストパラメーターのパフォーマンスを定義します。

>[!VIDEO](https://video.tv.adobe.com/v/26314?quality=12&learn=on)

### パイプライン実行

[CI/CD 本番パイプライン](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-deployment.html?lang=ja)は、ステージングを通じてコードを構築して本番環境にデプロイするために使用され、価値創出までの時間を短縮します。

>[!VIDEO](https://video.tv.adobe.com/v/26317?quality=12&learn=on)

## CI/CD 実稼動以外のパイプライン {#cicd-non-production-pipeline}

[CI/CD 実稼動以外のパイプラインは、コード品質パイプラインとデプロイパイプラインの 2 つのカテゴリに分類されます。](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/pipelines/production-pipelines.html?lang=ja)コード品質は、Git ブランチのすべてのコードをパイプライン化し、Cloud Manager のコード品質スキャンに対して構築および評価されます。デプロイメントパイプラインは、Git リポジトリから非本番環境へのコードの自動デプロイメントをサポートしています。つまり、ステージング環境や本番環境以外のプロビジョニング済み AEM 環境がサポートされます。

>[!VIDEO](https://video.tv.adobe.com/v/26316?quality=12&learn=on)

## アクティビティ {#activity}

Cloud Manager ではプログラムのアクティビティをまとめて表示することができ、本番稼動と本番稼動以外の両方の CI/CD パイプライン実行をすべて一覧表示し、過去と現在のアクティビティを可視化するとともに、任意のアクティビティの詳細を確認できます。

また、Cloud Manager は、ユーザーごとのレベルで [Adobe Experience Cloud 通知](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/notifications.html?lang=ja)と統合し、関心のあるイベントやアクションをどこにでも表示できるようにします。

>[!VIDEO](https://video.tv.adobe.com/v/26319?quality=12&learn=on)
