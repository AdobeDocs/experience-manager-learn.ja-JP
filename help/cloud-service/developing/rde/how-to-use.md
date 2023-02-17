---
title: 迅速な開発環境の使用方法
description: ラピッド開発環境を使用して、ローカルマシンからコードとコンテンツをデプロイする方法を説明します。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
jira: KT-11862
thumbnail: KT-11862.png
last-substantial-update: 2023-02-15T00:00:00Z
source-git-commit: 81e1e2bf0382f6a577c1037dcd0d58ebc73366cd
workflow-type: tm+mt
source-wordcount: '862'
ht-degree: 1%

---


# 迅速な開発環境の使用方法

学ぶ **使用方法** AEM as a Cloud Serviceの Rapid Development Environment(RDE)。 お気に入りの統合開発環境 (IDE) から、最終的に近いコードの開発サイクルを高速化するために、コードとコンテンツを RDE にデプロイします。

使用 [AEM WKND Sites Project](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) AEM-RDE の `install` コマンドを使用します。

- AEMコードおよびコンテンツパッケージ（すべて、ui.apps）のデプロイメント
- OSGi バンドルおよび設定ファイルのデプロイメント
- Apache および Dispatcher 設定の zip ファイルでのデプロイメント
- 個々のファイル（HTL など） `.content.xml` （ダイアログ XML）のデプロイメント
- 次のような他の RDE コマンドを確認します。 `status, reset and delete`

>[!VIDEO](https://video.tv.adobe.com/v/3415491/?quality=12&learn=on)

## 前提条件

のクローン [WKND サイト](https://github.com/adobe/aem-guides-wknd#aem-wknd-sites-project) プロジェクトを開き、お気に入りの IDE で開いて、AEMアーティファクトを RDE にデプロイします。

    &quot;&#39;シェル
    $ git clone git@github.com:adobe/aem-guides-wknd.git
    &quot;&#39;

次に、次の maven コマンドを実行して、ビルドし、ローカルのAEM-SDK にデプロイします。

    &quot;&#39;
    $ cd aem-guides-wknd/
    $ mvn clean install -PautoInstallSinglePackage
    &quot;&#39;

## AEM-RDE プラグインを使用したAEMアーティファクトのデプロイ

の使用 `aem:rde:install` 」コマンドを使用して、様々なAEMアーティファクトをデプロイします。

### デプロイ `all` および `dispatcher` パッケージ

一般的な出発点は、最初に `all` および `dispatcher` 次のコマンドを実行してパッケージを作成します。

    &quot;&#39;シェル
    # &#39;all&#39;パッケージをインストール
    $ aem:rde:all/target/aem-guides-wknd.all-2.1.3-SNAPSHOT.zip をインストールします。
    
    # &#39;dispatcher&#39; zip をインストールします
    $ aem:rde:dispatcher/target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip をインストールします。
    &quot;&#39;

デプロイメントが成功したら、オーサーサービスとパブリッシュサービスの両方で WKND サイトを検証します。 WKND サイトページ上のコンテンツを追加、編集して公開できるはずです。

### コンポーネントの拡張とデプロイ

次に、 `Hello World Component` RDE にデプロイします。

1. ダイアログ XML (`.content.xml`) `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld/_cq_dialog/` フォルダー
1. を `Description` 既存の `Text` ダイアログフィールド

   ```xml
   ...
   <description
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/textfield"
       fieldLabel="Description"
       name="./description"/>       
   ...
   ```

1. を開きます。 `helloworld.html` ファイルから `ui.apps/src/main/content/jcr_root/apps/wknd/components/helloworld` フォルダー
1. をレンダリング `Description` 既存の `<div>` 要素 `Text` プロパティ。

   ```html
   ...
   <div class="cmp-helloworld__item" data-sly-test="${properties.description}">
       <p class="cmp-helloworld__item-label">Description property:</p>
       <pre class="cmp-helloworld__item-output" data-cmp-hook-helloworld="property">${properties.description}</pre>
   </div>              
   ...
   ```

1. Maven のビルドを実行するか、個々のファイルを同期して、ローカルのAEM-SDK で変更を検証します。

1. を介して RDE に変更をデプロイします。 `ui.apps` パッケージを作成するか、個々のダイアログおよび HTL ファイルをデプロイします。

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

1. RDE で変更を検証するには、 `Hello World Component` を WKND サイトページに貼り付けます。

### 以下を確認します。 `install` コマンドオプション

上記の個々のファイルデプロイメントコマンドの例では、 `-t` および `-p` フラグは、それぞれ JCR パスのタイプと宛先を示すために使用されます。 利用可能な `install` コマンドオプションを使用するには、次のコマンドを実行します。

    &quot;&#39;シェル
    $ aem:rde:install —help
    &quot;&#39;

フラグは自明で、 `-s` フラグは、オーサーサービスまたはパブリッシュサービスだけにデプロイメントのターゲットを設定する場合に役立ちます。 以下を使用： `-t` デプロイ時のフラグ **content-file または content-xml** ファイルを `-p` フラグを設定して、AEM RDE 環境での宛先 JCR パスを指定します。

### OSGi バンドルをデプロイ

OSGi バンドルのデプロイ方法について詳しくは、 `HelloWorldModel` Java™クラスを使用して RDE にデプロイします。

1. を開きます。 `HelloWorldModel.java` ファイルから `core/src/main/java/com/adobe/aem/guides/wknd/core/models` フォルダー
1. を更新します。 `init()` メソッドを次に示します。

   ```java
   ...
   message = "Hello World!\n"
       + "Resource type is: " + resourceType + "\n"
       + "Current page is:  " + currentPagePath + "\n"
       + "Changes deployed via RDE, lets try faster dev cycles";
   ...
   ```

1. ローカルのAEM-SDK で変更を検証するには、 `core` maven コマンドを介したバンドル
1. 次のコマンドを実行して、RDE に変更をデプロイします。

   ```shell
   $ cd core
   $ mvn clean package
   $ aio aem:rde:install target/aem-guides-wknd.core-2.1.3-SNAPSHOT.jar
   ```

1. RDE で変更を検証するには、 `Hello World Component` を WKND サイトページに貼り付けます。

### OSGi 設定のデプロイ

個々の設定ファイルをデプロイするか、設定パッケージ全体をデプロイできます。次に例を示します。

    &quot;&#39;シェル
    #個々の設定ファイルをデプロイ
    $ aem:rde:ui.config/src/main/content/jcr_root/apps/wknd/osgiconfig/config/org.apache.sling.commons.log.LogManager.factory.config～wknd.cfg.json をインストールします。
    
    #または設定パッケージ全体をデプロイします
    $ cd ui.config
    $ mvn クリーンパッケージ
    $ aem:rde:target/aem-guides-wknd.ui.config-2.1.3-SNAPSHOT.zip をインストールします。
    &quot;&#39;

>[!TIP]
>
>OSGi 設定をオーサーインスタンスまたはパブリッシュインスタンスにのみインストールするには、 `-s` フラグ。


### Apache または Dispatcher 設定のデプロイ

Apache または Dispatcher の設定ファイル **個別にデプロイすることはできません**&#x200B;と呼ばれる場合でも、Dispatcher のフォルダー構造全体を ZIP ファイルの形式でデプロイする必要があります。

1. 必要に応じて、 `dispatcher` モジュール ( デモ用に、 `dispatcher/src/conf.d/available_vhosts/wknd.vhost` キャッシュする `html` は 60 秒間のみファイルを保存します。

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

1. ローカルで変更を検証する場合は、 [Dispatcher をローカルで実行する](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html#run-dispatcher-locally) を参照してください。
1. 次のコマンドを実行して、変更を RDE にデプロイします。

   ```shell
   $ cd dispatcher
   $ mvn clean install
   $ aio aem:rde:install target/aem-guides-wknd.dispatcher.cloud-2.1.3-SNAPSHOT.zip
   ```

1. RDE での変更の検証

## 追加のAEM RDE プラグインコマンド

ローカルマシンから RDE を管理および操作する、追加のAEM RDE プラグインコマンドを確認します。

    &quot;&#39;シェル
    $ aio aem:rde —help
    RapidDev 環境とやり取りする。
    
    使用方法
    $ aio aem rde コマンド
    
    コマンド
    aem rde 現在の rde からバンドルと設定を削除します。
    aem rde history 現在の rde に対しておこなわれた更新のリストを取得します。
    aem rde インストール/アップデートバンドル、設定および content-packages をインストールします。
    aem rde reset RDE をリセット
    aem rde restart RDE のオーサーとパブリッシュを再起動します。
    aem rde ステータス現在の rde にデプロイされたバンドルと設定のリストを取得します。
    &quot;&#39;

上記のコマンドを使用すると、お気に入りの IDE から RDE を管理して、開発/デプロイメントのライフサイクルを迅速に実行できます。

## 次のステップ

詳しくは、 [RDE を使用した開発/デプロイメントのライフサイクル](./development-life-cycle.md) 機能を高速で提供する。


## その他のリソース

[RDE コマンドドキュメント](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/rapid-development-environments.html#rde-cli-commands)

[Adobe I/O Runtime CLI プラグイン (AEM Rapid Development Environments とのやり取り用 )](https://github.com/adobe/aio-cli-plugin-aem-rde#aio-cli-plugin-aem-rde)

[AEM Project setup](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/project-archetype/project-setup.html?lang=ja)
