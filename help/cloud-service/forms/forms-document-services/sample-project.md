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
exl-id: f1fcc4bb-cc31-45e8-b7bb-688ef6a236bb
source-git-commit: 1a76256677d06aaffd142c46dc9167a669ac6455
workflow-type: ht
source-wordcount: '94'
ht-degree: 100%

---

# ローカル環境でのテスト

* プロジェクトの読み込み

   * [サンプルプロジェクト](./assets/formsdocumentservices.zip)のダウンロードと抽出
   * 任意の **Java 開発環境**（IntelliJ IDEA、Eclipse または VS Code）を開き、プロジェクトを Maven プロジェクトとして読み込みます。
* 資格情報の設定

   * `resources/credentials/server_credentials.json` ファイルを見つけます。
   * これを開き、環境に固有の&#x200B;**資格情報を更新**&#x200B;します。
   * 次の有効な値が含まれていることを確認します。
     `clientId`、`clientSecret`、`adobeIMSV3TokenEndpointURL`。
     `scopes`

* メインクラスの実行

   * `src/main/java/Main.java` に移動し、メインメソッドを実行します。

* 実行の確認
   * ターミナルウィンドウで出力を確認します。
