---
title: AdobeCloud Managerについて
description: AdobeCloud Managerは、AEM環境の管理、内省、セルフサービスを容易におこなえる、シンプルで堅牢なソリューションです。
sub-product: cloud-manager、foundation
topics: best-practices, cicd, development, operations, governance
doc-type: feature video
activity: understand
audience: developer, implementer, administrator, architect
topic: アーキテクチャ
role: Architect
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '532'
ht-degree: 23%

---


# AdobeCloud Managerについて

AdobeCloud Managerは、AEM環境の管理、内省、セルフサービスを容易におこなえる、シンプルで堅牢なソリューションです。

## Cloud Manager の概要

このビデオシリーズでは、AEM向けCloud Managerの主な機能を紹介します。

* [プログラム](#programs)
* [環境](#environments)
* [レポート](#reports)
* [CI/CD実稼動パイプライン](#cicd-production-pipeline)
* [CI/CD非実稼動パイプライン](#cicd-non-production-pipeline)
* [アクティビティ](#activity)

概要については、[Cloud Manager ユーザーガイド](https://docs.adobe.com/content/help/ja/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html)を参照してください。

## プログラム {#programs}

[Cloud Managerプログラム](https://docs.adobe.com/content/help/ja-JP/experience-manager-cloud-manager/using/getting-started/setting-up-program.html) は、通常、購入したサービスレベル契約(SLA)に対応する、ビジネスイニシアチブの論理的なセットをサポートするAEM環境のセットを表します。例えば、あるプログラムは、グローバルなパブリックWebサイトをサポートするAEMリソースを表し、別のプログラムは、内部のCentral DAMを表します。

>[!VIDEO](https://video.tv.adobe.com/v/26313/?quality=12&learn=on)

## 環境 {#environments}

[Cloud Manager環境](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/manage-your-environment.html) は、AEMオーサー、AEMパブリッシュ、Dispatcherの各インスタンスで構成されます。環境が異なれば、役割がサポートされ、様々なCI/CDパイプラインを使用してエンゲージできます（以下で説明）。 Cloud Manager環境には通常、実稼動環境とステージング環境が1つあります。

>[!VIDEO](https://video.tv.adobe.com/v/26318/?quality=12&learn=on)

## レポート {#reports}

[Cloud Manager のレポートでは、各 AEM インスタンスの様々な指標をレポートおよび追跡する一連のグラフを通じて、プログラムの環境と AEM インスタンスを確認できます。](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/monitor-your-environments.html)

>[!VIDEO](https://video.tv.adobe.com/v/26315/?quality=12&learn=on)

## CI/CD実稼動パイプライン{#cicd-production-pipeline}

*[Use CI/CD Pipeline in Production Cloud Managervideoシリーズは、実稼動パイプラインの実行に関する深い掘り下げを提供しま](./use-the-cicd-pipeline-in-cloud-manager-for-aem.md) す。失敗したデプロイメントや成功したデプロイメントの調査なども含まれます。*

>[!NOTE]
>
> これらのビデオ全体で、ビルド、テスト、デプロイメントに要する時間が長くなり、ビデオの時間が短縮されました。 パイプラインの完全な実行には、通常、プロジェクトのサイズ、AEMインスタンスの数、UATプロセスに応じて、45分以上かかります（必須の30分のパフォーマンステストを含む）。

### 設定

[CI/CD実稼動パイプライン](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html)設定は、パイプラインを開始するトリガー、実稼動デプロイメントを制御するパラメーター、パフォーマンステストパラメーターを定義します。

>[!VIDEO](https://video.tv.adobe.com/v/26314/?quality=12&learn=on)

### パイプラインの実行

[CI/CD実稼動パイプライン](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/deploying-code.html)は、ステージングを通じてコードを構築し実稼動環境にデプロイするために使用され、価値創出までの時間を短縮します。

>[!VIDEO](https://video.tv.adobe.com/v/26317/?quality=12&learn=on)

## CI/CD非実稼動パイプライン{#cicd-non-production-pipeline}

[CI／CD 非実稼動パイプラインは、コード品質パイプラインとデプロイパイプラインの 2 つのカテゴリに分類されます。](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/configuring-pipeline.html#non-production--code-quality-only-pipelines)コード品質は、Git ブランチのすべてのコードをパイプライン化し、Cloud Manager のコード品質スキャンに対して構築および評価されます。デプロイメントパイプラインは、Gitリポジトリから非実稼動環境(ステージング環境または実稼動環境以外のプロビジョニング済みAEM環境)へのコードの自動デプロイメントをサポートします。

>[!VIDEO](https://video.tv.adobe.com/v/26316/?quality=12&learn=on)

## アクティビティ {#activity}

Cloud Managerは、すべてのCI/CDパイプライン実行をリストするプログラムのアクティビティを統合表示し、過去と現在のアクティビティと、アクティビティの詳細を確認できます。

また、Cloud Managerは、ユーザー単位のレベルで[Adobe Experience Cloud通知](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/notifications.html)と統合され、関心のあるイベントやアクションに対する全体像を提供します。

>[!VIDEO](https://video.tv.adobe.com/v/26319/?quality=12&learn=on)
