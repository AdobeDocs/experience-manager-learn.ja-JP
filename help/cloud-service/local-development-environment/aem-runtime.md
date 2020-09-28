---
title: Cloud Service開発としてのAEM用のローカルAEMランタイムの設定
description: AEMをCloud ServiceSDKのQuickstart Jarとして使用して、Local AEM Runtimeをセットアップします。
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4678, 4677
thumbnail: 32551.jpg
translation-type: tm+mt
source-git-commit: ecbd4d21c5f41b2bc6db3b409767b767f00cc5d1
workflow-type: tm+mt
source-wordcount: '1518'
ht-degree: 1%

---


# ローカルAEM Runtimeのセットアップ

Adobe Experience Manager(AEM)は、AEMをCloud ServiceSDKのQuickstart Jarとして使用して、ローカルで実行できます。 これにより、開発者は、カスタムコード、設定、コンテンツをソース管理にコミットする前に、カスタムコード、設定、コンテンツをデプロイしてテストし、Cloud Service環境としてAEMにデプロイできます。

は、ユーザーのディレクトリの略記法 `~` として使用されます。 Windowsでは、これはと同じで `%HOMEPATH%`す。

>[!VIDEO](https://video.tv.adobe.com/v/32551/?quality=12&learn=on)

>[!NOTE]
>
> このビデオでは、AEM SDKのローカルクイックスタートを使用して、Adobe Experience Managerのローカルインスタンスを数分でインストールおよび実行する方法を示します。 このビデオでは、重複がquickstart JarファイルをクリックしてAEM SDKのローカルクイックスタートを開始する方法を示します。ただし、これは、Java 8がコンピューターにインストールされている場合は動作しません。 または、このページで `java -jar ...` 説明しているように、コマンドを使用して、AEM SDKのローカルクイックスタートをコマンドラインから起動することもで [きます](#set-up-local-aem-author-service)。

## Javaのインストール

Experience ManagerはJavaアプリケーションなので、開発ツールをサポートするJava SDKが必要です。

1. [最新のJava SDK 11をダウンロードしてインストールします](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Content%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Autoling&amp;fulltext=Oracle%7E+11%7E&amp;orderby=%40jcr%3Content%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=リスト&amp;p.offset=0&amp;p.limit=14)
1. 次のコマンドを実行して、Java 11 SDKがインストールされていることを確認します。
   + Windows：`java -version`
   + macOS/Linux: `java --version`

![Java](./assets/aem-runtime/java.png)

## Cloud ServiceSDKとしてAEMをダウンロードする

AEMは、Cloud ServiceSDK、またはAEM SDKとして、開発のためにAEM AuthorとPublishをローカルで実行するためのQuickstart Jarと、互換性のあるバージョンのディスパッチャーツールを含みます。

1. Log in to [https://experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads) with your Adobe ID
   + AEMをCloud ServiceSDKとしてダウンロードするには、Adobe組織 __がAEM用にCloud Serviceとしてプロビジョニングされている必要があります__ 。
1. 「 __AEM」タブにCloud Serviceとして移動します__ 。
1. __発行日順に並べ替え__ ( ____ 降順順)
1. 最新の __AEM SDK__ 結果行をクリックします
1. EULAを確認して同意し、「 __ダウンロード__ 」ボタンをタップします

## AEM SDKのzipからQuickstart JARを抽出します。

1. Unzip the downloaded `aem-sdk-XXX.zip` file
1. Experience Manager開発者用の __license.properties__ ファイルが使用可能であることを確認します。

同じQuickstart Jarファイルとlicense.propertiesファイルを使用して、 _AEM AuthorとPublish_ Servicesの両方を開始します。

## ローカルのAEM Authorサービスのセットアップ{#set-up-local-aem-author-service}

ローカルのAEM Authorサービスは、デジタルマーケターやコンテンツの作成者がコンテンツの作成と管理を共有する、ローカルエクスペリエンスのデジタルマーケターを開発者に提供します。  AEM Author Serviceは、オーサリングとプレビューの両方の環境として設計されており、これに対して機能開発のほとんどの検証を実行できるので、ローカル開発プロセスの重要な要素です。

1. フォルダーの作成 `~/aem-sdk/author`
1. Quickstart __JAR__ ファイルをにコピー `~/aem-sdk/author` し、 `aem-author-p4502.jar`
1. license.properties ____ ファイルを  `~/aem-sdk/author`
1. コマンドラインから次のコマンドを実行して、ローカルのAEM Author Serviceを開始します。
   + `java -jar aem-author-p4502.jar`
      + 管理者パスワードをに指定し `admin`ます。 任意の管理者パスワードを使用できますが、再設定の必要性を減らすために、ローカル開発でデフォルトを使用することをお勧めします。

   重複 *をクリックして、AEMをCloud ServiceQuickstart Jarとして* 開始することはできません [](#troubleshooting-double-click)。
1. Webブラウザーでhttp://localhost:4502 [からローカルのAEM Author Serviceにアクセスします](http://localhost:4502) 。

Windows：

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\author
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\author\aem-author-p4502.jar
$ copy ../license.properties c:\Users\<My User>\aem-sdk\author
$ cd c:\Users\<My User>\aem-sdk\author
$ java -jar aem-author-p4502.jar
```

macOS/Linux:

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cp ../license.properties ~/aem-sdk/author
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

## ローカルAEM発行サービスのセットアップ

ローカルのAEM発行サービスは、AEM上に保存されているWebサイトの参照など、AEMのエンドユーザーが持つローカルエクスペリエンスを開発者に提供します。 ローカルのAEM発行サービスは、AEM SDKの [ディスパッチャーツールと統合し](./dispatcher-tools.md) 、開発者がエンドユーザーの体験を煙でテストし、微調整できるようにするため、重要です。

1. フォルダーの作成 `~/aem-sdk/publish`
1. Quickstart __JAR__ ファイルをにコピー `~/aem-sdk/publish` し、 `aem-publish-p4503.jar`
1. license.properties ____ ファイルを  `~/aem-sdk/publish`
1. コマンドラインから次のコマンドを実行して、ローカルのAEM発行サービスを開始します。
   + `java -jar aem-publish-p4503.jar`
      + 管理者パスワードをに指定し `admin`ます。 任意の管理者パスワードを使用できますが、再設定の必要性を減らすために、ローカル開発でデフォルトを使用することをお勧めします。

   重複 *をクリックして、AEMをCloud ServiceQuickstart Jarとして* 開始することはできません [](#troubleshooting-double-click)。
1. Webブラウザーのhttp://localhost:4503 [で、ローカルのAEM Publish Serviceにアクセスします](http://localhost:4503) 。

Windows：

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\publish
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\publish\aem-publish-p4503.jar
$ copy ../license.properties c:\Users\<My User>\aem-sdk\publish
$ cd c:\Users\<My User>\aem-sdk\publish
$ java -jar aem-publish-p4503.jar
```

macOS/Linux:

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cp ../license.properties ~/aem-sdk/publish
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

## Quickstart JAR開始アップモード

Quickstart Jarの名前は、開始アップの方法を `aem-<tier>_<environment>-p<port number>.jar` 指定します。 AEMを特定の層、作成者、または発行で起動した後は、別の層に変更することはできません。 これを行うには、最初の実行時に生成された `crx-Quickstart` フォルダーを削除し、Quickstart Jarを再度実行する必要があります。 環境とポートは変更できますが、ローカルAEMインスタンスの停止/開始が必要です。

環境 `dev`、 `stage``prod`およびの変更は、環境固有の設定がAEMで正しく定義され、解決されることを確認する開発者にとって役立ちます。 ローカル開発は、主にデフォルトの `dev` 環境実行モードに対して行うことをお勧めします。

次の順に並べ替えます。

+ `aem-author-p4502.jar`
   + ポート4502でのDev実行モードでの作成者
+ `aem-author_dev-p4502.jar`
   + ポート4502でのDev実行モードの作成者(同じ `aem-author-p4502.jar`)
+ `aem-author_stage-p4502.jar`
   + ポート4502でのステージング実行モードでの作成者
+ `aem-author_prod-p4502.jar`
   + ポート4502での実稼働実行モードの作成者
+ `aem-publish-p4503.jar`
   + ポート4503でのDev実行モードでの作成者
+ `aem-publish_dev-p4503.jar`
   + ポート4503でのDev実行モードの作成者(同じ `aem-publish-p4503.jar`)
+ `aem-publish_stage-p4503.jar`
   + ポート4503でのステージング実行モードでの作成者
+ `aem-publish_prod-p4503.jar`
   + ポート4503での実稼働実行モードの作成者

ポート番号は、ローカル開発マシン上で使用可能な任意のポートにすることができますが、規則に従う必要があります。

+ ポート __4502__ は、 __ローカルのAEM Authorサービスに使用されます。__
+ ポート __4503__ は、 __ローカルAEM発行サービスで使用されます。__

これらの変更を行うと、AEM SDK設定の調整が必要になる場合があります

## ローカルAEMランタイムの停止

ローカルAEMランタイムを停止するには、AEM Authorまたは発行サービスのいずれかを停止し、AEMランタイムの開始に使用したコマンドラインウィンドウを開いてをタップし `Ctrl-C`ます。 AEMがシャットダウンするまで待ちます。 シャットダウン処理が完了すると、コマンドラインプロンプトが表示されます。

## Quickstart Jarを更新するタイミング

AEM SDKを毎月少なくとも毎月、または毎月の最終木曜日に更新します。これは、AEMのリリースカデンスで、Cloud Service「機能リリース」として更新します。

>[!WARNING]
>
> Quickstart Jarを新しいバージョンに更新するには、ローカル開発環境全体を置き換える必要があり、その結果、ローカルのAEMリポジトリ内のすべてのコード、設定、およびコンテンツが失われます。 破棄すべきでないコード、設定またはコンテンツが、AEMパッケージとしてローカルのAEMインスタンスから安全にGitにコミットされているか、またはエクスポートされていることを確認してください。

### AEM SDKのアップグレード時にコンテンツの損失を防ぐ方法

AEM SDKをアップグレードすると、新しいリポジトリを含む新しいAEMランタイムが効果的に作成されます。つまり、以前のAEM SDKのリポジトリに対して行われた変更はすべて失われます。 以下は、AEM SDKのアップグレード間でコンテンツを継続的に保持するための実行可能な戦略で、個別に使用することも、連携して使用することもできます。

1. 開発に役立つ「サンプル」コンテンツを含む専用のコンテンツパッケージを作成し、Gitで維持します。 AEM SDKのアップグレードを通じて保持する必要のあるコンテンツはすべて、このパッケージに格納され、AEM SDKのアップグレード後に再デプロイされます。
1. 以前のAEM SDKリポジトリから新しいAEM SDKリポジトリに [コンテンツをコピーするには、](https://jackrabbit.apache.org/oak/docs/migration.html) ディレクティブと共にoak-upgrade `includepaths` (oak-upgrade)を使用します。
1. AEM Package Managerを使用してコンテンツをバックアップし、以前のAEM SDKのコンテンツパッケージを新しいAEM SDKに再インストールします。

AEM SDKのアップグレード間でコードを維持するために上記の方法を使用すると、開発のアンチパターンを示すことに注意してください。 使い捨てでないコードは、開発IDEから派生し、デプロイメントを介してAEM SDKに流れ込む必要があります。

## トラブルシューティング

## 重複がQuickstart Jarファイルをクリックすると、エラーが発生する{#troubleshooting-double-click}

Quickstart Jarを重複クリックして開始すると、エラーモーダルが表示され、AEMがローカルで起動できなくなります。

![トラブルシューティング — Quickstart Jarファイルを重複クリック](./assets/aem-runtime/troubleshooting__double-click.png)

これは、AEM Quickstart JarとしてのCloud Serviceは、開始AEMに対するQuickstart Jarの重複クリックをローカルでサポートしていないためです。 代わりに、そのコマンドラインからJarファイルを実行する必要があります。

AEM Authorサービスを開始す `cd` るには、Quickstart Jarを含むディレクトリに移動し、次のコマンドを実行します。

`$ java -jar aem-author-p4502.jar`

または、AEM発行サービスに開始す `cd` るには、Quickstart Jarを含むディレクトリに移動し、次のコマンドを実行します。

`$ java -jar aem-author-p4503.jar`

## コマンドラインからQuickstart Jarを起動すると、すぐに中止されます{#troubleshooting-java-8}

コマンドラインからQuickstart Jarを開始すると、プロセスは直ちに中止され、AEMサービスは開始しません。次のエラーが発生します。

```shell
➜  ~/aem-sdk/author: java -jar aem-author-p4502.jar
Loading quickstart properties: default
Loading quickstart properties: instance
java.lang.Exception: Quickstart requires a Java Specification 11 VM, but your VM (Java HotSpot(TM) 64-Bit Server VM / Oracle Corporation) reports java.specification.version=1.8
  at com.adobe.granite.quickstart.base.impl.Main.checkEnvironment(Main.java:1046)
  at com.adobe.granite.quickstart.base.impl.Main.<init>(Main.java:646)
  at com.adobe.granite.quickstart.base.impl.Main.main(Main.java:981)
Quickstart: aborting
```

これは、AEMがCloud Serviceとして必要なJava SDK 11が異なるバージョンを実行している場合、おそらくJava 8が必要なためです。 この問題を解決するには、 [Oracle Java SDK 11をダウンロードしてインストールし](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Content%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Autoling&amp;fulltext=Oracle%7E+11%7E&amp;orderby=%40jcr%3Content%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=リスト&amp;p.offset=0&amp;p.limit=14)ます。
Java SDK 11がインストールされたら、コマンドラインから次のコマンドを実行して、アクティブバージョンであることを確認します。

Java 11 SDKがインストールされたら、コマンドラインから次のコマンドを実行して、アクティブなバージョンであることを確認します。

+ Windows：`java -version`
+ macOS/Linux: `java --version`

## その他のリソース

+ [AEM SDKのダウンロード](https://experience.adobe.com/#/downloads)
+ [Adobeクラウドマネージャー](https://my.cloudmanager.adobe.com/)
+ [Dockerのダウンロード](https://www.docker.com/)
+ [Experience Managerディスパッチャードキュメント](https://docs.adobe.com/content/help/ja-JP/experience-manager-dispatcher/using/dispatcher.html)
