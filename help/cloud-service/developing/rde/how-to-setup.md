---
title: 迅速な開発環境の設定方法
description: AEM as a Cloud Service の迅速な開発環境を設定する方法を説明します。
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11861
thumbnail: KT-11861.png
last-substantial-update: 2024-06-04T00:00:00Z
exl-id: ab9ee81a-176e-4807-ba39-1ea5bebddeb2
duration: 485
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '668'
ht-degree: 100%

---

# 迅速な開発環境の設定方法

AEM as a Cloud Service で迅速な開発環境（RDE）を&#x200B;**設定する方法**&#x200B;を説明します。

このビデオでは、次の内容を紹介します。

- Cloud Manager を使用した、プログラムへの RDE の追加
- Adobe IMS を使用した RDE ログインフロー（他の AEM as a Cloud Service 環境との類似）
- [Adobe I/O Runtime 拡張可能 CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/)（別名 `aio CLI`）の設定
- 非インタラクティブモードを使用した AEM RDE および Cloud Manager `aio CLI` プラグインのセットアップと設定。インタラクティブモードについて詳しくは、[設定手順](#setup-the-aem-rde-plugin)を参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/3415490?quality=12&learn=on)

## 前提条件

以下をローカルにインストールしておく必要があります。

- [Node.js](https://nodejs.org/ja/)（LTS - 長期サポート）
- [npm 8 以降](https://docs.npmjs.com/)

## ローカル設定

[WKND Sites プロジェクト](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project)のコードとコンテンツをローカルマシンから RDE 上にデプロイするには、次の手順を実行します。

### Adobe I/O Runtime 拡張可能 CLI

コマンドラインから次のコマンドを実行して、Adobe I/O Runtime 拡張可能 CLI（別名 `aio CLI`）をインストールします。

```shell
$ npm install -g @adobe/aio-cli
```

### aio CLI プラグインのインストールと設定

RDE を操作するには、aio CLI にプラグインをインストールし、組織、プログラムおよび RDE 環境 ID を設定する必要があります。設定は、よりシンプルなインタラクティブモードまたは非インタラクティブモードを使用して、aio CLI 経由で実行できます。

>[!BEGINTABS]

>[!TAB インタラクティブモード]

`aio cli` の `plugins:install` コマンドを使用して、AEM RDE プラグインをインストールおよび設定します。

1. `aio cli` の `plugins:install` コマンドを使用して、aio CLI の AEM RDE プラグインをインストールします。

   ```shell
   $ aio plugins:install @adobe/aio-cli-plugin-aem-rde    
   $ aio plugins:update
   ```

   AEM RDE プラグインによって、開発者はローカルマシンからコードとコンテンツをデプロイできるようになります。

2. 次のコマンドを実行して Adobe I/O Runtime 拡張可能 CLI にログインし、アクセストークンを取得します。Cloud Manager と同じ Adobe 組織にログインする必要があります。

   ```shell
   $ aio login
   ```

3. 次のコマンドを実行して、インタラクティブモードを使用して RDE を設定します。

   ```shell
   $ aio aem:rde:setup
   ```

4. CLI では、組織 ID、プログラム ID、環境 ID の入力が求められます。

   ```shell
   Setup the CLI configuration necessary to use the RDE commands.
   ? Do you want to store the information you enter in this setup procedure locally? (y/N)
   ```

   - 単一の RDE のみを使用し、RDE 設定をローカルマシンにグローバルに保存する場合は、「__No__」を選択します。

   - 複数の RDE を使用する場合や、各プロジェクトの現在のフォルダーの `.aio` ファイルに RDE 設定をローカルに保存する場合は、「__Yes__」を選択します。

5. 使用可能なオプションのリストから、組織 ID、プログラム ID、RDE 環境 ID を選択します。

6. 次のコマンドを実行して、正しい組織、プログラム、環境が設定されていることを確認します。

   ```shell
   $ aio aem rde setup --show
   ```

>[!TAB 非インタラクティブモード]

`aio cli` の `plugins:install` コマンドを使用して、Cloud Manager と AEM RDE のプラグインをインストールおよび設定します。

```shell
$ aio plugins:install @adobe/aio-cli-plugin-cloudmanager
$ aio plugins:install @adobe/aio-cli-plugin-aem-rde
$ aio plugins:update
```

Cloud Manager プラグインは、開発者がコマンドラインから Cloud Manager を操作できるようにします。

AEM RDE プラグインによって、開発者はローカルマシンからコードとコンテンツをデプロイできるようになります。

aio CLI プラグインは、RDE を操作できるように設定する必要があります。

1. まず、Cloud Manager を使用して、組織、プログラム、環境の ID の値をコピーします。

   - 組織 ID：**プロファイル画像／アカウント情報（内部）／モーダルウィンドウ／現在の組織 ID** から値をコピーします

   ![組織 ID](./assets/Org-ID.png)

   - プログラム ID：**プログラム概要／環境／{ProgramName}-rde／ブラウザー URI／`program/` と`/environment`** の間の数値から値をコピーします

   ![プログラム ID と環境 ID](./assets/Program-Environment-Id.png)

   - 環境 ID：**プログラム概要／環境／{ProgramName}-rde／ブラウザー URI／`environment/`** の後の数値から値をコピーします

   ![プログラム ID と環境 ID](./assets/Program-Environment-Id.png)

1. `aio cli` の `config:set` コマンドを使用し、次のコマンドを実行して、これらの値を設定します。

   ```shell
   $ aio config:set cloudmanager_orgid <ORGANIZATION ID>
   $ aio config:set cloudmanager_programid <PROGRAM ID>
   $ aio config:set cloudmanager_environmentid <ENVIRONMENT ID>
   ```

1. 次のコマンドを実行して、現在の設定値を確認します。

   ```shell
   $ aio config:list
   ```

1. 現在ログインしている組織の切り替えまたは確認を行います。

   ```shell
   $ aio where
   ```

>[!ENDTABS]

## RDE アクセスの検証

次のコマンドを実行して、AEM RDE プラグインのインストールと設定を確認します。

```shell
$ aio aem:rde:status
```

RDE ステータス情報として、環境ステータス、_AEM プロジェクト_&#x200B;バンドルのリスト、オーサー サービスとパブリッシュサービスの設定が表示されます。

## 次の手順

お気に入りの統合開発環境（IDE）からコードとコンテンツをデプロイして、開発サイクルを高速化する RDE の[使用方法](./how-to-use.md)を説明します。


## その他のリソース

[プログラムドキュメントでの RDE の有効化](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html?lang=ja#enabling-rde-in-a-program)

[Adobe I/O Runtime 拡張可能 CLI](https://developer.adobe.com/runtime/docs/guides/tools/cli_install/)（別名 `aio CLI`）の設定

[aio CLI の使用とコマンド](https://github.com/adobe/aio-cli#usage)

[AEM の迅速な開発環境とやり取りするための Adobe I/O Runtime CLI プラグイン](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[Cloud Manager aio CLI プラグイン](https://github.com/adobe/aio-cli-plugin-cloudmanager)
