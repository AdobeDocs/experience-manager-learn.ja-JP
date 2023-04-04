---
title: 迅速な開発環境の設定方法
description: AEM as a Cloud Service用の迅速な開発環境を設定する方法を説明します。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2023-02-15T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '492'
ht-degree: 7%

---


# 迅速な開発環境の設定方法

学ぶ **設定方法** AEM as a Cloud Serviceの Rapid Development Environment(RDE)。

このビデオでは、次の内容を紹介します。

- Cloud Manager を使用したプログラムへの RDE の追加
- Adobe IMSを使用した RDE ログインフロー ( 他のAEMas a Cloud Service環境とどのように類似しているか )
- の設定 [Adobe I/O Runtime Extensible CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) 別名 `aio CLI`
- AEM RDE および Cloud Manager のセットアップと設定 `aio CLI` プラグイン

>[!VIDEO](https://video.tv.adobe.com/v/3415490?quality=12&learn=on)

## 前提条件

以下をローカルにインストールしておく必要があります。

- [Node.js](https://nodejs.org/ja/) （LTS — 長期サポート）
- [npm 8 以降](https://docs.npmjs.com/)

## ローカル設定

をデプロイするには、以下を実行します。 [WKND Sites プロジェクトの](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) ローカルマシンから RDE 上にコードとコンテンツを取り込むには、次の手順を実行します。

### Adobe I/O Runtime Extensible CLI

Adobe I/O Runtime Extensible CLI( `aio CLI` 次のコマンドをコマンドラインから実行します。

```shell
$ npm install -g @adobe/aio-cli
```

### AEM plugins

を使用して、Cloud Manager およびAEM RDE プラグインをインストールします。 `aio cli`&#39;s `plugins:install` コマンドを使用します。

```shell
$ aio plugins:install @adobe/aio-cli-plugin-cloudmanager

$ aio plugins:install @adobe/aio-cli-plugin-aem-rde
```

Cloud Manager プラグインを使用すると、開発者はコマンドラインから Cloud Manager を操作できます。

AEM RDE プラグインを使用すると、開発者はローカルマシンからコードとコンテンツをデプロイできます。

また、プラグインを更新するには、 `aio plugins:update` コマンドを使用します。

## AEMプラグインの設定

AEMプラグインは、RDE とやり取りするように設定する必要があります。 まず、Cloud Manager UI を使用して、組織 ID、プログラム ID、環境 ID の値をコピーします。

1. 組織 ID :値のコピー元 **プロファイルの画像/アカウント情報（内部）/モーダルウィンドウ/現在の組織 ID**

   ![組織 ID](./assets/Org-ID.png)

1. プログラム ID:値のコピー元 **プログラムの概要/環境/ {ProgramName}-rde /ブラウザー URI /次の間の数値 `program/` および`/environment`**

1. 環境 ID :値のコピー元 **プログラムの概要/環境/ {ProgramName}-rde /ブラウザー URI /次の後の数値`environment/`**

   ![プログラムおよび環境 ID](./assets/Program-Environment-Id.png)

1. 次に、 `aio cli`&#39;s `config:set` コマンドを実行して、これらの値を設定します。

   ```shell
   $ aio config:set cloudmanager_orgid <org-id>
   
   $ aio config:set cloudmanager_programid <program-id>
   
   $ aio config:set cloudmanager_environmentid <env-id>
   ```

次のコマンドを実行して、現在の設定値を確認できます。

```shell
$ aio config:list
```

また、現在ログインしている組織を切り替えたり、把握したりするには、次のコマンドを使用します。

```shell
$ aio where
```

## RDE アクセスの検証

次のコマンドを実行して、AEM RDE プラグインのインストールと設定を確認します。

```shell
$ aio aem:rde:status
```

RDE のステータス情報は、環境ステータスや _AEMプロジェクト_ オーサーサービスとパブリッシュサービスのバンドルと設定。

## 次のステップ

学ぶ [使用方法](./how-to-use.md) お気に入りの統合開発環境 (IDE) からコードとコンテンツをデプロイして、開発サイクルを高速化する RDE です。


## その他のリソース

[プログラムドキュメントでの RDE の有効化](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#enabling-rde-in-a-program)

の設定 [Adobe I/O Runtime Extensible CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/) 別名 `aio CLI`

[AIO CLI の使用とコマンド](https://github.com/adobe/aio-cli#usage)

[Adobe I/O Runtime CLI プラグイン (AEM Rapid Development Environments とのやり取り用 )](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Cloud Manager AIO CLI プラグイン](https://github.com/adobe/aio-cli-plugin-cloudmanager)
