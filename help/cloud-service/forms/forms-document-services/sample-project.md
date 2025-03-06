---
title: サンプルプロジェクト
description: 環境に読み込んで実行できるサンプルプロジェクト
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Document Services
topic: Development
jira: KT-17479
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
source-git-commit: a72f533b36940ce735d5c01d1625c6f477ef4850
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 8%

---


# ローカル環境でのテスト

* プロジェクトの読み込み

   * [ サンプルプロジェクト ](./assets/formsdocumentservices.zip) をダウンロードして抽出します。
   * 好みの **Java 開発環境** IntelliJ IDEA、Eclipse または VS Code）を開き、プロジェクトを Maven プロジェクトとして読み込みます
* 資格情報の設定

   * ファイル `resources/credentials/server_credentials.json` を見つけます。
   * これを開き、環境に固有の **資格情報を更新** します。
   * 次の有効な値が含まれていることを確認します。
     `clientId`、`clientSecret`、`adobeIMSV3TokenEndpointURL` および
     `scopes`

* Main クラスの実行

   * `src/main/java/Main.java` に移動し、main メソッドを実行します。

* 実行の検証
   * ターミナルウィンドウで出力を確認します

