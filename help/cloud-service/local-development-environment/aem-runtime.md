---
title: AEM用のローカルAEMランタイムをCloud Service開発として設定
description: AEMをCloud ServiceSDKのQuickstart Jarとして使用して、Local AEM Runtimeを設定します。
feature: 開発者ツール
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4678, 4677
thumbnail: 32551.jpg
topic: 開発
role: Developer
level: Beginner
source-git-commit: d49ae402b332ba972a78cdbd8f5bf962b91c83b1
workflow-type: tm+mt
source-wordcount: '1734'
ht-degree: 3%

---


# ローカルAEM Runtimeの設定

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_aemruntime"
>title="Local AEM Runtime"
>abstract="Adobe Experience Manager(AEM)は、AEM as a Cloud Service SDKのQuickstart Jarとして使用して、ローカルで実行できます。 これにより、開発者は、カスタムコード、設定およびコンテンツをソース管理にコミットする前に、カスタムコード、設定およびコンテンツをデプロイおよびテストし、Cloud Service環境としてAEMにデプロイできます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html" text="AEM as a Cloud Service の SDK"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/ja/aemcloud.html" text="AEMをCloud ServiceSDKとしてダウンロード"

Adobe Experience Manager(AEM)は、AEM as a Cloud Service SDKのQuickstart Jarとして使用して、ローカルで実行できます。 これにより、開発者は、カスタムコード、設定およびコンテンツをソース管理にコミットする前に、カスタムコード、設定およびコンテンツをデプロイおよびテストし、Cloud Service環境としてAEMにデプロイできます。

`~`は、ユーザーのディレクトリの略記法として使用されます。 Windowsの場合、`%HOMEPATH%`と同じです。

## Javaのインストール

Experience ManagerはJavaアプリケーションなので、開発ツールをサポートするためにJava SDKが必要です。

1. [最新のJava SDK 11をダウンロードしてインストールする](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Autoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40cr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. 次のコマンドを実行して、Java 11 SDKがインストールされていることを確認します。
   + Windows：`java -version`
   + macOS/Linux:`java --version`

![Java](./assets/aem-runtime/java.png)

## AEM as aCloud ServiceSDKのダウンロード

AEM as aCloud ServiceSDK(AEM SDK)には、AEMオーサーとパブリッシュを開発用にローカルで実行するためのクイックスタートJARと、互換性のあるバージョンのDispatcherツールが含まれています。

1. Adobe IDで[https://experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads)にログインします。
   + AEMをCloud ServiceSDKとしてダウンロードするには、Adobe組織&#x200B;__をCloud ServiceとしてAEM用にプロビジョニングする必要があります。__
1. 「__AEM as aCloud Service__」タブに移動します。
1. __公開日__&#x200B;で並べ替え（__降順__）
1. 最新の&#x200B;__AEM SDK__&#x200B;結果行をクリックします。
1. 使用許諾契約書を確認して同意し、「__ダウンロード__」ボタンをタップします。

## AEM SDK zipからクイックスタートJARを抽出します。

1. ダウンロードした`aem-sdk-XXX.zip`ファイルを解凍します。

## ローカルのAEMオーサーサービス{#set-up-local-aem-author-service}を設定します。

ローカルのAEMオーサーサービスは、開発者にローカルエクスペリエンスのデジタルマーケターやコンテンツの作成者が共有し、コンテンツを作成および管理することを提供します。  AEMオーサーサービスは、オーサリングとプレビューの両方の環境として設計されており、機能開発のほとんどの検証をその環境に対して実行でき、ローカル開発プロセスの重要な要素となります。

1. フォルダー`~/aem-sdk/author`を作成します。
1. __クイックスタートJAR__&#x200B;ファイルを`~/aem-sdk/author`にコピーし、`aem-author-p4502.jar`に名前を変更します。
1. コマンドラインから次のコマンドを実行して、ローカルのAEMオーサーサービスを開始します。
   + `java -jar aem-author-p4502.jar`
      + 管理パスワードに`admin`を指定します。 adminパスワードはどれでも問題ありませんが、再設定の必要性を減らすために、ローカル開発のデフォルトを使用することをお勧めします。

   **&#x200B;は、](#troubleshooting-double-click)をダブルクリックして、AEMをCloud ServiceクイックスタートJAR [として起動することはできません。
1. Webブラウザーの[http://localhost:4502](http://localhost:4502)にあるローカルのAEMオーサーサービスにアクセスします。

Windows：

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\author
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\author\aem-author-p4502.jar
$ cd c:\Users\<My User>\aem-sdk\author
$ java -jar aem-author-p4502.jar
```

macOS/Linux:

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

## ローカルAEMパブリッシュサービスの設定

ローカルのAEMパブリッシュサービスは、AEM上にホストするWebサイトの参照など、AEMのエンドユーザーのローカルエクスペリエンスを開発者に提供します。 ローカルのAEMパブリッシュサービスは、AEM SDKの[Dispatcherツール](./dispatcher-tools.md)と統合され、開発者が最終的なエンドユーザー対応エクスペリエンスをスモークテストおよび微調整できるので、重要です。

1. フォルダー`~/aem-sdk/publish`を作成します。
1. __クイックスタートJAR__&#x200B;ファイルを`~/aem-sdk/publish`にコピーし、`aem-publish-p4503.jar`に名前を変更します。
1. コマンドラインから次のコマンドを実行して、ローカルのAEMパブリッシュサービスを開始します。
   + `java -jar aem-publish-p4503.jar`
      + 管理パスワードに`admin`を指定します。 adminパスワードはどれでも問題ありませんが、再設定の必要性を減らすために、ローカル開発のデフォルトを使用することをお勧めします。

   **&#x200B;は、](#troubleshooting-double-click)をダブルクリックして、AEMをCloud ServiceクイックスタートJAR [として起動することはできません。
1. Webブラウザーの[http://localhost:4503](http://localhost:4503)にあるローカルのAEMパブリッシュサービスにアクセスします。

Windows：

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\publish
$ copy aem-sdk-Quickstart-XXX.jar c:\Users\<My User>\aem-sdk\publish\aem-publish-p4503.jar
$ cd c:\Users\<My User>\aem-sdk\publish
$ java -jar aem-publish-p4503.jar
```

macOS/Linux:

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-Quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

## コンテンツ配布をシミュレート{#content-distribution}

真のCloud Service環境では、[Slingコンテンツ配布](https://sling.apache.org/documentation/bundles/content-distribution.html)とAdobeパイプラインを使用して、コンテンツをオーサーサービスからパブリッシュサービスに配布します。 [Adobeパイプライン](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/core-concepts/architecture.html?lang=en#content-distribution)は、クラウド環境でのみ使用可能な独立したマイクロサービスです。

開発時に、ローカルのオーサーサービスとパブリッシュサービスを使用してコンテンツの配布をシミュレートする方が望ましい場合があります。 これは、レガシーレプリケーションエージェントを有効にすることで実現できます。

>[!NOTE]
>
> レプリケーションエージェントは、ローカルのクイックスタートJARでのみ使用でき、コンテンツ配布のシミュレーションのみ提供します。

1. **Author**&#x200B;サービスにログインし、[http://localhost:4502/etc/replication/agents.author.html](http://localhost:4502/etc/replication/agents.author.html)に移動します。
1. 「**デフォルトエージェント（パブリッシュ）**」をクリックして、デフォルトのレプリケーションエージェントを開きます。
1. **Edit**&#x200B;をクリックして、エージェントの設定を開きます。
1. 「**設定**」タブで、次のフィールドを更新します。

   + **有効**  - trueを確認
   + **エージェントユーザーID**  — このフィールドは空のままにします。

   ![レプリケーションエージェントの設定 — 設定](assets/aem-runtime/settings-config.png)

1. 「**Transport**」タブで、次のフィールドを更新します。

   + **URI** - `http://localhost:4503/bin/receive?sling:authRequestLogin=1`
   + **ユーザー** - `admin`
   + **パスワード** - `admin`

   ![レプリケーションエージェントの設定 — トランスポート](assets/aem-runtime/transport-config.png)

1. **OK**&#x200B;をクリックして設定を保存し、**デフォルト**&#x200B;レプリケーションエージェントを有効にします。
1. これで、オーサーサービスのコンテンツに変更を加えて、パブリッシュサービスにパブリッシュできます。

![ページを公開](assets/aem-runtime/publish-page-changes.png)

## クイックスタートJAR起動モード

クイックスタートJARの名前`aem-<tier>_<environment>-p<port number>.jar`は、その起動方法を指定します。 AEMを特定の層、オーサーまたはパブリッシュで起動した後は、別の層に変更できません。 これをおこなうには、最初の実行時に生成された`crx-Quickstart`フォルダーを削除し、クイックスタートJARを再度実行する必要があります。 環境とポートは変更できますが、ローカルのAEMインスタンスの停止/開始が必要です。

環境`dev`、`stage`および`prod`の変更は、AEMで環境固有の設定が正しく定義され、解決されるように開発に役立ちます。 ローカル開発は主にデフォルトの`dev`環境実行モードに対しておこなうことをお勧めします。

利用可能な順列は次のとおりです。

+ `aem-author-p4502.jar`
   + ポート4502の開発実行モードの作成者
+ `aem-author_dev-p4502.jar`
   + ポート4502の開発実行モードの作成者として（`aem-author-p4502.jar`と同じ）
+ `aem-author_stage-p4502.jar`
   + ポート4502のステージング実行モードの作成者
+ `aem-author_prod-p4502.jar`
   + ポート4502の実稼動実行モードの作成者
+ `aem-publish-p4503.jar`
   + ポート4503の開発実行モードの作成者
+ `aem-publish_dev-p4503.jar`
   + ポート4503の開発実行モードの作成者として（`aem-publish-p4503.jar`と同じ）
+ `aem-publish_stage-p4503.jar`
   + ポート4503のステージング実行モードの作成者
+ `aem-publish_prod-p4503.jar`
   + ポート4503の実稼動実行モードの作成者

通常は、ポート番号はローカル開発・マシン上の使用可能な任意のポートにすることができます。

+ ポート&#x200B;__4502__&#x200B;は、__ローカルのAEMオーサーサービス__&#x200B;に使用されます。
+ ポート&#x200B;__4503__&#x200B;は、__ローカルのAEMパブリッシュサービス__&#x200B;に使用されます。

これらの設定を変更するには、AEM SDKの設定の調整が必要になる場合があります

## ローカルAEMランタイムの停止

ローカルのAEMランタイム（AEMオーサーサービスまたはパブリッシュサービス）を停止するには、AEM Runtimeの開始に使用したコマンドラインウィンドウを開き、`Ctrl-C`をタップします。 AEMがシャットダウンするまで待ちます。 シャットダウンプロセスが完了すると、コマンドラインプロンプトが表示されます。

## オプションのローカルAEMランタイム設定タスク

+ __OSGi設定環境変数とシークレ__ ット変数 [は、aio CLIを使用して管理するのではなく、AEMローカルランタイム用に特別に設定されます](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=ja#local-development)。

## クイックスタートJARを更新するタイミング

AEM SDKを、少なくとも毎月またはその直後(Cloud Serviceの「機能リリース」としてのAEMのリリースサイクル)に更新します。

>[!WARNING]
>
> クイックスタートJARを新しいバージョンに更新するには、ローカル開発環境全体を置き換える必要があり、その結果、ローカルのAEMリポジトリ内のすべてのコード、設定およびコンテンツが失われます。 破棄すべきでないコード、設定またはコンテンツが、Gitに安全にコミットされるか、AEMパッケージとしてローカルのAEMインスタンスからエクスポートされることを確認します。

### AEM SDKをアップグレードする際のコンテンツの損失を回避する方法

AEM SDKをアップグレードすると、新しいリポジトリを含む新しいAEMランタイムが効果的に作成されます。つまり、以前のAEM SDKのリポジトリに対しておこなわれた変更は失われます。 次に、AEM SDKのアップグレード間でコンテンツを永続化するための有効な戦略を示します。これらは、離散的に、または協調的に使用できます。

1. 開発に役立つ「サンプル」コンテンツを含む専用のコンテンツパッケージを作成し、Gitで維持します。 AEM SDKのアップグレードを通じて保持する必要のあるコンテンツは、このパッケージに保持され、AEM SDKをアップグレードした後に再デプロイされます。
1. [oak-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html)を`includepaths`ディレクティブと共に使用して、以前のAEM SDKリポジトリから新しいAEM SDKリポジトリにコンテンツをコピーします。
1. 以前のAEM SDKのAEM Package Managerとコンテンツパッケージを使用してコンテンツをバックアップし、新しいAEM SDKに再インストールします。

上記の方法を使用してAEM SDKのアップグレード間のコードを維持する場合、開発アンチパターンを示すことを忘れないでください。 使い捨て以外のコードは、開発IDEで作成し、デプロイメントを介してAEM SDKに送信する必要があります。

## トラブルシューティング

## クイックスタートJARファイルをダブルクリックすると、エラー{#troubleshooting-double-click}が発生します。

クイックスタートJARをダブルクリックして起動すると、エラーモーダルが表示され、AEMがローカルで起動されない。

![トラブルシューティング — クイックスタートJARファイルをダブルクリック](./assets/aem-runtime/troubleshooting__double-click.png)

これは、AEM as a AEM Quickstart Jarは、Quickstart JarをダブルクリックしてローカルでCloud Serviceを起動できないためです。 代わりに、そのコマンドラインからJarファイルを実行する必要があります。

AEMオーサーサービスを起動するには、Quickstart Jarを含むディレクトリに`cd`を入れ、次のコマンドを実行します。

`$ java -jar aem-author-p4502.jar`

または、AEMパブリッシュサービスを起動するには、Quickstart Jarを含むディレクトリに`cd`を入れ、次のコマンドを実行します。

`$ java -jar aem-publish-p4503.jar`

## コマンドラインからのクイックスタートJARの起動は、直ちに{#troubleshooting-java-8}を中止します。

コマンドラインからQuickstart Jarを起動すると、プロセスが直ちに中断され、AEMサービスが起動しません。次のエラーが発生します。

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

これは、AEM as aCloud ServiceにはJava SDK 11が必要で、異なるバージョン（おそらくJava 8）を実行しているためです。 この問題を解決するには、[OracleJava SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Autoling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40cr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)をダウンロードしてインストールします。
Java SDK 11がインストールされたら、コマンドラインから次を実行して、それがアクティブなバージョンであることを確認します。

Java 11 SDKがインストールされたら、コマンドラインからコマンドを実行して、SDKがアクティブなバージョンであることを確認します。

+ Windows：`java -version`
+ macOS/Linux:`java --version`

## その他のリソース

+ [AEM SDKのダウンロード](https://experience.adobe.com/#/downloads)
+ [AdobeCloud Manager](https://my.cloudmanager.adobe.com/)
+ [Dockerのダウンロード](https://www.docker.com/)
+ [Experience ManagerDispatcherのドキュメント](https://docs.adobe.com/content/help/ja-JP/experience-manager-dispatcher/using/dispatcher.html)
