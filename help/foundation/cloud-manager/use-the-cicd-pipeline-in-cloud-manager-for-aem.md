---
title: Adobe Cloud Manager での CI/CD Pipeline の使用
description: Adobe Cloud Manager は、AEM プロジェクトチームが AMS でホストされるすべての AEM 環境に、迅速、安全かつ一貫してコードをデプロイするための、シンプルで柔軟なセルフサービス CI／CD パイプラインを提供します。 このビデオシリーズでは、Cloud Manager の CI/CD Pipeline の設定と実行を、失敗シナリオと成功シナリオの両方で説明します。
sub-product: Experience Manager Cloud Manager, Experience Manager
topics: cicd, performance, best-practices, development, governance
doc-type: feature video
activity: understand
audience: all
topic: Architecture
feature: Cloud Manager
role: Developer
level: Beginner
exl-id: d5d59ef5-9343-4ac2-9053-a010decdb9b6
last-substantial-update: 2022-08-15T00:00:00Z
thumbnail: cm-pipeline.jpg
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: ht
source-wordcount: '312'
ht-degree: 100%

---

# Adobe Cloud Manager での CI/CD Pipeline の使用

Adobe Cloud Manager は、AEM プロジェクトチームが AMS でホストされるすべての AEM 環境に、迅速、安全かつ一貫してコードをデプロイするための、シンプルで柔軟なセルフサービス CI／CD パイプラインを提供します。 このビデオシリーズでは、Cloud Manager の CI/CD Pipeline の設定と実行を、失敗シナリオと成功シナリオの両方で説明します。

## はじめに

Cloud Manager および Cloud Manager プログラムの概要です。

>[!NOTE]
>
>これらの動画では、ビルド、テスト、デプロイに要する時間をスピードアップし、視聴時間を短縮しました。完全なパイプラインの実行には通常 45 分以上かかります（必須の 30 分のパフォーマンステストを含む）。プロジェクトのサイズ、AEM インスタンスの数、UAT プロセスの数に応じて異なります。

>[!VIDEO](https://video.tv.adobe.com/v/23082?quality=12&learn=on)

## CI／CD パイプラインの設定

このビデオでは、Cloud Manager でのプログラムのパイプラインの設定について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/23083?quality=12&learn=on)

## 失敗のパイプライン実行

このビデオでは、Cloud Manager で必要とされる品質チェックに失敗するコードを使用する CI/CD Pipeline の実行を、 **[!DNL yellow]** リポジトリブランチで行う方法を見ていきます。

>[!VIDEO](https://video.tv.adobe.com/v/23084?quality=12&learn=on)

## 成功のパイプライン実行

このビデオでは、Cloud Manager で必要とされる品質チェックに成功するコードを使用する CI/CD Pipeline の実行を、 **[!DNL master]** リポジトリブランチで行う方法を見ていきます。

このビデオでは、Cloud Manager の[!UICONTROL アクティビティ]コンソールについても見てきます。このコンソールでは、アクティブな実行に再度入ることや、完了した実行または失敗した実行を確認することができます。

>[!VIDEO](https://video.tv.adobe.com/v/23085?quality=12&learn=on)

## サポート資料

* [Cloud Manager ユーザーガイド](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/introduction.html?lang=ja)
* [コードスキャン [!DNL SonarQube] ルール](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/content/using/code-quality-testing.html)をダウンロード
   * *リンクされたセクションの下部にある XLSX*
* [[!DNL SonarQube] Java™ ルールのインデックス](https://rules.sonarsource.com/java/)
