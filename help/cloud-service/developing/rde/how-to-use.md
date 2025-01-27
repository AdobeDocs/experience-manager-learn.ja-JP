---
title: 迅速な開発環境の使用方法
description: 迅速な開発環境を使用して、ローカルマシンからコードとコンテンツをデプロイする方法を説明します。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11862
thumbnail: KT-11862.png
last-substantial-update: 2023-02-15T00:00:00Z
exl-id: 1d1bcb18-06cd-46fc-be2a-7a3627c1e2b2
duration: 792
source-git-commit: d199ff3b9f4d995614c193f52dc90270f2283adf
workflow-type: tm+mt
source-wordcount: '792'
ht-degree: 80%

---

# 迅速な開発環境の使用方法

AEM as a Cloud Serviceの迅速な開発環境（RDE **について** 使用方法）を説明します。 お気に入りの統合開発環境（IDE）から RDE にコードとコンテンツをデプロイして、ほぼ最終的なコードを開発するサイクルを高速化します。

お気に入りの IDE から AEM-RDE の `install` コマンドを実行して様々な AEM アーティファクトを RDE にデプロイする方法を、[AEM WKND Sites プロジェクト](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) を使用して説明します。

- AEM のコードとコンテンツのパッケージ（すべて、ui.apps）のデプロイメント
- OSGi バンドルと設定ファイルのデプロイメント
- zip ファイルでの Apache およびDispatcher設定のデプロイメント
- HTL や `.content.xml`（ダイアログ XML）などの個々のファイルのデプロイメント
- `status, reset and delete` などの他の RDE コマンドの確認

>[!VIDEO](https://video.tv.adobe.com/v/3415491?quality=12&learn=on)

## 前提条件

[WKND Sites](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) プロジェクトのクローンを作成してお気に入りの IDE で開き、AEM アーティファクトを RDE にデプロイします。

```shell
$ git clone git@github.com:adobe/aem-guides-wknd.git
```

次に、以下の maven コマンドを実行してビルドし、ローカルの AEM-SDK にデプロイします。

```
$ cd aem-guides-wknd/
$ mvn clean package
```

## AEM-RDE プラグインを使用した AEM アーティファクトのデプロイ

まず、[最新の `aio` CLI モジュールがインストールされている](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/development-tools#aio-cli)ことを確認します。

次に、`aio aem:rde:install` コマンドを使用して、様々な AEM アーティファクトをデプロイします。以下を行う必要があります。

### `all` パッケージと `dispatcher` パッケージのデプロイ

一般には、まず次のコマンドを実行して `all` パッケージと `dispatcher` パッケージをデプロイすることから始まります。

```shell
# Install the 'all' content package (zip file)
$ aio aem:rde:install all/target/aem-guides-wknd.all-2.1.3-SNAPSHOT.zip

# Install the 'dispatcher' deployment artifact (zip file)
$ aio aem:rde:install dispatcher/target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
```

デプロイに成功したら、オーサーサービスとパブリッシュサービスの両方で WKND サイトを検証します。WKND サイトページのコンテンツを追加および編集し公開できるはずです。

### コンポーネントの機能強化とデプロイ

`Hello World Component` を機能強化して RDE にデプロイします。

1. `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/` フォルダーからダイアログ XML（`.content.xml`）ファイルを開きます。
1. 既存の `Text` ダイアログフィールドの後に `Description` テキストフィールドを追加します。

   ```xml
   ...
   <description
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
       fieldLabel="Description"
       name="./description"/>       
   ...
   ```

1. `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld` フォルダーから `helloworld.html` ファイルを開きます。
1. `Text` プロパティの既存の `<div>` 要素の後に `Description` プロパティをレンダリングします。

   ```html
   ...
   <div class="cmp-helloworld__item" data-sly-test="${properties.description}">
       <p class="cmp-helloworld__item-label">Description property:</p>
       <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.description}</pre>
   </div>              
   ...
   ```

1. Maven ビルドを実行するか個々のファイルを同期して、ローカルのAEM SDKで変更を検証します。

1. `ui.apps` パッケージを介して、または個々のダイアログファイルと HTL ファイルをデプロイして、変更を RDE にデプロイします。

   ```shell
   # Using 'ui.apps' package
   
   $ cd ui.apps
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.ui.apps-2.1.3-SNAPSHOT.zip
   
   # Or by deploying the individual HTL and Dialog XML
   
   # HTL file
   $ aio aem:rde:install ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/helloworld.html -t content-file -p /apps/wknd/components/helloworld/helloworld.html
   
   # Dialog XML
   $ aio aem:rde:install ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/.content.xml -t content-xml -p /apps/wknd/components/helloworld/_cq_dialog/.content.xml
   ```

1. WKND サイト ページで `Hello World Component` を追加または編集して、RDE での変更を検証します。

### `install` コマンドオプションの確認

上記の個々のファイルデプロイメントコマンドの例では、`-t` フラグと `-p` フラグを使用して、それぞれ JCR パスのタイプと宛先を示しています。次のコマンドを実行して、使用可能な `install` コマンドオプションを確認してみましょう。

```shell
$ aio aem:rde:install --help
```

フラグは自明であり、`-s` フラグは、オーサーサービスまたはパブリッシュサービスのみをデプロイメントの対象とする場合に便利です。**content-file または content-xml** ファイルをデプロイするときに `-t` フラグを `-p` フラグとともに使用して、AEM RDE 環境での宛先 JCR パスを指定します。

### OSGi バンドルのデプロイ

OSGi バンドルのデプロイ方法を説明するために、`HelloWorldModel` Java™ クラスを機能強化して RDE にデプロイします。

1. `core/src/main/java/com/adobe/aem/guides/wknd/core/models` フォルダー内の `HelloWorldModel.java` ファイルを開きます。
1. `init()` メソッドを次のように更新します。

   ```java
   ...
   message = "Hello World!\n"
       + "Resource type is: " + resourceType + "\n"
       + "Current page is:  " + currentPagePath + "\n"
       + "Changes deployed via RDE, lets try faster dev cycles";
   ...
   ```

1. Maven コマンドを使用して `core` バンドルをデプロイすることで、ローカル AEM-SDK の変更を検証します。
1. 次のコマンドを実行して、RDE に変更をデプロイします。

   ```shell
   $ cd core
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.core-2.1.3-SNAPSHOT.jar
   ```

1. WKND サイトページで `Hello World Component` を追加または編集して、RDE での変更を検証します。

### OSGi 設定のデプロイ

個々の設定ファイルまたは設定パッケージ全体をデプロイできます。次に例を示します。

```shell
# Deploy individual config file
$ aio aem:rde:install ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config/org.apache.sling.commons.log.LogManager.factory.config~wknd.cfg.json

# Or deploy the complete config package
$ cd ui.config
$ mvn clean package
$ aio aem:rde:install target/aem-guides-wknd.ui.config-2.1.3-SNAPSHOT.zip
```

>[!TIP]
>
>OSGi 設定をオーサーインスタンスまたはパブリッシュインスタンスにのみインストールするには、`-s` フラグを使用します。


### Apache または Dispatcher 設定のデプロイ

Apache または Dispatcher 設定ファイルを&#x200B;**個別にデプロイできるのではなく**、Dispatcher フォルダー構造全体を ZIP ファイルの形式でデプロイする必要があります。

1. `dispatcher` モジュールの設定ファイルに必要な変更を加えます。デモのために、`html` ファイルを 60 秒間だけキャッシュするように `dispatcher/src/conf.d/available_vhosts/wknd.vhost` を更新します。

   ```
   ...
   <LocationMatch "^/content/.*\.html$">
       Header unset Cache-Control
       Header always set Cache-Control "max-age=60,stale-while-revalidate=60" "expr=%{REQUEST_STATUS} < 400"
       Header always set Surrogate-Control "stale-while-revalidate=43200,stale-if-error=43200" "expr=%{REQUEST_STATUS} < 400"
       Header set Age 0
   </LocationMatch>
   ...
   ```

1. ローカルで変更を検証します。詳しくは、[Dispatcher のローカルでの実行](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools)を参照してください。
1. 次のコマンドを実行して、変更を RDE にデプロイします。

   ```shell
   $ cd dispatcher
   $ mvn clean install
   $ aio aem:rde:install target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
   ```

1. RDE での変更を検証します。

### 設定（YAML）ファイルのデプロイ

CDN、メンテナンスタスク、ログ転送およびAEM API 認証設定ファイルは、`install` コマンドを使用して RDE にデプロイできます。 これらの設定は、AEM プロジェクトの `config` フォルダーで YAML ファイルとして管理されます。詳しくは、[ サポートされる設定 ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/operations/config-pipeline#configurations) を参照してください。

設定ファイルのデプロイ方法を説明するには、`cdn` 設定ファイルを機能強化して RDE にデプロイします。

1. `config` フォルダーの `cdn.yaml` ファイルを開きます。
1. 目的の設定を更新します。例えば、レート制限を 1 秒あたり 200 リクエストに更新します

   ```yaml
   kind: "CDN"
   version: "1"
   metadata:
     envTypes: ["dev", "stage", "prod"]
   data:
     trafficFilters:
       rules:
       #  Block client for 5m when it exceeds an average of 100 req/sec to origin on a time window of 10sec
       - name: limit-origin-requests-client-ip
         when:
           reqProperty: tier
           equals: 'publish'
         rateLimit:
           limit: 200 # updated rate limit
           window: 10
           count: fetches
           penalty: 300
           groupBy:
             - reqProperty: clientIp
         action: log
   ...
   ```

1. 次のコマンドを実行して、RDE に変更をデプロイします。

   ```shell
   $ aio aem:rde:install -t env-config ./config/cdn.yaml
   ```

1. RDE での変更の検証


## 追加の AEM RDE プラグインコマンド

ローカルマシンから RDE を管理および操作するための追加の AEM RDE プラグインコマンドを確認します。

```shell
$ aio aem:rde --help
Interact with RapidDev Environments.

USAGE
$ aio aem rde COMMAND

COMMANDS
aem rde delete   Delete bundles and configs from the current rde.
aem rde history  Get a list of the updates done to the current rde.
aem rde install  Install/update bundles, configs, and content-packages.
aem rde reset    Reset the RDE
aem rde restart  Restart the author and publish of an RDE
aem rde status   Get a list of the bundles and configs deployed to the current rde.
```

上記のコマンドを使用すると、お気に入りの IDE から RDE を管理して、開発/デプロイメントのライフサイクルを迅速化できます。

## 次の手順

機能を迅速に提供できるように、[RDE を使用した開発／デプロイメントライフサイクル](./development-life-cycle.md)について説明します。


## その他のリソース

[RDE コマンドのドキュメント](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments)

[AEM の迅速な開発環境とやり取りするための Adobe I/O Runtime CLI プラグイン](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[AEM プロジェクトのセットアップ](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/project-setup)
