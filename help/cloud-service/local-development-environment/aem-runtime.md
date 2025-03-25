---
title: AEM as a Cloud Service 開発用のローカル AEM SDK の設定
description: AEM as a Cloud Service SDK のクイックスタート jar を使用して、ローカル AEM SDK ランタイムを設定します。
feature: Developer Tools
version: Experience Manager as a Cloud Service
kt: 4678, 4677
thumbnail: 32551.jpg
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-02T00:00:00Z
exl-id: 19f72254-2087-450b-909d-2d90c9821486
duration: 411
source-git-commit: 99e3cadc71ca4e26f9e4034085788dfc5407d1bb
workflow-type: tm+mt
source-wordcount: '1696'
ht-degree: 99%

---

# ローカル AEM SDK の設定 {#set-up-local-aem-sdk}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_aemruntime"
>title="ローカル AEM ランタイム"
>abstract="Adobe Experience Manager（AEM）は、AEM as a Cloud Service SDK のクイックスタート jar を使用してローカルで実行できます。これにより、開発者は、ソース管理にコミットして AEM as a Cloud Service 環境にデプロイする前に、カスタムコード、設定およびコンテンツをデプロイしてテストできます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=ja" text="AEM as a Cloud Service の SDK"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/jp/aemcloud.html" text="AEM as a Cloud Service SDK のダウンロード"

Adobe Experience Manager（AEM）は、AEM as a Cloud Service SDK のクイックスタート jar を使用してローカルで実行できます。これにより、開発者は、ソース管理にコミットして AEM as a Cloud Service 環境にデプロイする前に、カスタムコード、設定およびコンテンツをデプロイしてテストできます。

`~` は、ユーザーのディレクトリの略記法として使用されます。Windows では、これは `%HOMEPATH%` に相当します。

## Java™ のインストール

Experience Manager は Java™ アプリケーションなので、開発ツールをサポートするには Oracle Java™ SDK が必要です。

1. [最新の Java™ SDK 11 をダウンロードしてインストール](https://experience.adobe.com/#/downloads/content/software-distribution/jp/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. 次のコマンドを実行して、Oracle Java™ 11 SDK がインストールされていることを確認します。

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java --version
```

>[!TAB Windows]

```shell
$ java -version
```

>[!TAB Linux®]

```shell
$ java --version
```

>[!ENDTABS]

![Java](./assets/aem-runtime/java.png)

## AEM as a Cloud Service SDK のダウンロード

AEM as a Cloud Service SDK または AEM SDK には、AEM オーサーおよびパブリッシュを開発用にローカルで実行するために使用されるクイックスタート jar と、Dispatcher ツールの互換バージョンが含まれています。

1. Adobe IDを使用して ](https://experience.adobe.com/#/downloads/content/software-distribution/jp/aemcloud.html)0}https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html} にログインします[
   + AEM as a Cloud Service SDK をダウンロードするには、アドビ組織が AEM as a Cloud Service に対してプロビジョニングされている&#x200B;__必要があります__。
1. 「__AEM as a Cloud Service__」タブに移動します
1. __公開日__&#x200B;を&#x200B;__降__&#x200B;順で並べ替えます
1. 最新の __AEM SDK__ 結果行をクリックします
1. EULA を確認して同意し、「__ダウンロード__」ボタンをタップします

## AEM SDK zip からのクイックスタート jar の抽出

1. ダウンロードした `aem-sdk-XXX.zip` ファイルを解凍します

## ローカル AEM オーサーサービスの設定{#set-up-local-aem-author-service}

ローカル AEM オーサーサービスは、デジタルマーケターやコンテンツ作成者がコンテンツの作成と管理のために共有するローカルエクスペリエンスを開発者に提供します。AEM オーサーサービスは、オーサリングおよびプレビュー環境の両方として設計されており、それに対して機能開発のほとんどの検証を実行できるので、ローカル開発プロセスの重要な要素となっています。

1. `~/aem-sdk/author` フォルダーを作成します
1. __クイックスタート jar__ ファイルを `~/aem-sdk/author` にコピーし、名前を `aem-author-p4502.jar` に変更します
1. コマンドラインから次のコマンドを実行して、ローカル AEM オーサーサービスを開始します。
   + `java -jar aem-author-p4502.jar`
      + 管理者パスワードを `admin` として指定します。任意の管理者パスワードを使用できますが、再設定の必要性を減らすために、ローカル開発にはデフォルトを使用することをお勧めします。

   [ダブルクリックして](#troubleshooting-double-click)、AEM as Cloud Service クイックスタート jar を起動することは&#x200B;*できません*。
1. Web ブラウザーで [http://localhost:4502](http://localhost:4502) のローカル AEM オーサーサービスにアクセスします。

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

>[!TAB Windows]

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\author
$ copy aem-sdk-quickstart-XXX.jar c:\Users\<My User>\aem-sdk\author\aem-author-p4502.jar
$ cd c:\Users\<My User>\aem-sdk\author
$ java -jar aem-author-p4502.jar
```

>[!TAB Linux®]

```shell
$ mkdir -p ~/aem-sdk/author
$ cp aem-sdk-quickstart-XXX.jar ~/aem-sdk/author/aem-author-p4502.jar
$ cd ~/aem-sdk/author
$ java -jar aem-author-p4502.jar
```

>[!ENDTABS]


## ローカル AEM パブリッシュサービスの設定

ローカル AEM パブリッシュサービスは、AEM でホストされている web サイトの参照など、AEM のローカルエンドユーザーが持つローカルエクスペリエンスを開発者に提供します。ローカル AEM パブリッシュサービスは、AEM SDK の [Dispatcher ツール](./dispatcher-tools.md)と統合され、開発者がスモークテストを行い、最終的なエンドユーザー向けエクスペリエンスを微調整できるので重要です。

1. `~/aem-sdk/publish` フォルダーを作成します
1. __クイックスタート jar__ ファイルを `~/aem-sdk/publish` にコピーし、名前を `aem-publish-p4503.jar` に変更します
1. コマンドラインから次のコマンドを実行して、ローカル AEM パブリッシュサービスを開始します。
   + `java -jar aem-publish-p4503.jar`
      + 管理者パスワードを `admin` として指定します。任意の管理者パスワードを使用できますが、再設定の必要性を減らすために、ローカル開発にはデフォルトを使用することをお勧めします。

   AEM as Cloud Service クイックスタート Jar を[ダブルクリック](#troubleshooting-double-click)しても、起動&#x200B;*できません*。
1. Web ブラウザーで [http://localhost:4503](http://localhost:4503) のローカル AEM パブリッシュサービスにアクセスします。

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

>[!TAB Windows]

```shell
$ mkdir -p c:\Users\<My User>\aem-sdk\publish
$ copy aem-sdk-quickstart-XXX.jar c:\Users\<My User>\aem-sdk\publish\aem-publish-p4503.jar
$ cd c:\Users\<My User>\aem-sdk\publish
$ java -jar aem-publish-p4503.jar
```

>[!TAB Linux®]

```shell
$ mkdir -p ~/aem-sdk/publish
$ cp aem-sdk-quickstart-XXX.jar ~/aem-sdk/publish/aem-publish-p4503.jar
$ cd ~/aem-sdk/publish
$ java -jar aem-publish-p4503.jar
```

>[!ENDTABS]


## プレリリースモードでのローカル AEM サービスの設定

ローカルの AEM ランタイムは、[プレリリース モード](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/release-notes/prerelease.html?lang=ja)で開始できるため、開発者は AEM as a Cloud Service の次のリリースの機能に対してビルドすることができます。プレリリースは、ローカル AEM ランタイムの最初の起動時に `-r prerelease` 引数を渡すことにより有効になります。これは、ローカルの AEM オーサーサービスと AEM パブリッシュサービスの両方で使用できます。


>[!BEGINTABS]

>[!TAB macOS]

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

>[!TAB Windows]

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

>[!TAB Linux®]

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

>[!ENDTABS]

## コンテンツ配布のシミュレーション {#content-distribution}

真の Cloud Service 環境では、コンテンツは、[Sling Content Distribution](https://sling.apache.org/documentation/bundles/content-distribution.html) と Adobe パイプラインを使用して、オーサーサービスからパブリッシュサービスに配信されます。この [Adobe パイプライン](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/core-concepts/architecture.html?lang=ja#content-distribution) は、クラウド内環境でのみ使用できる独立したマイクロサービスです。

開発中は、ローカルのオーサーサービスとパブリッシュサービスを使用して、コンテンツの配布をシミュレートする方が望ましい場合があります。 これは、従来のレプリケーションエージェントを有効にすることで実現できます。

>[!NOTE]
>
> レプリケーションエージェントは、ローカルのクイックスタート JAR でのみ使用することができ、コンテンツ配布のシミュレーションのみを提供します。

1. **オーサー**&#x200B;サービスににログインし、 [http://localhost:4502/etc/replication/agents.author.html](http://localhost:4502/etc/replication/agents.author.html) に移動します。
1. **デフォルトエージェント（公開）**&#x200B;をクリックして、デフォルトのレプリケーションエージェントを開きます。
1. 「**編集**」をクリックしてエージェントの設定を開きます。
1. 「**設定**」タブで、次のフィールドを更新します。

   + **有効** - true をチェック
   + **エージェントユーザー ID** - このフィールドは空のままにします

   ![レプリケーションエージェントの設定 - 設定](assets/aem-runtime/settings-config.png)

1. 「**トランスポート**」タブで、次のフィールドを更新します。

   + **URI** - `http://localhost:4503/bin/receive?sling:authRequestLogin=1`
   + **ユーザー** - `admin`
   + **パスワード** - `admin`

   ![レプリケーションエージェントの設定 - トランスポート](assets/aem-runtime/transport-config.png)

1. 「**OK**」をクリックして設定を保存し、**デフォルト** レプリケーションエージェントを有効にします。
1. これで、オーサーサービスのコンテンツに変更を加えて、変更内容をパブリッシュサービスに公開することができます。

![ページを公開](assets/aem-runtime/publish-page-changes.png)

## クイックスタート JAR の起動モード

クイックスタート JAR の名前 `aem-<tier>_<environment>-p<port number>.jar` が、起動方法を指定します。AEM を特定の層、オーサーやパブリッシュで開始した後は、別の層に変更することができません。 変更を行うには、最初の実行時に生成された `crx-Quickstart` フォルダーを削除し、クイックスタート JAR を再度実行する必要があります。環境とポートは変更できますが、ローカルの AEM インスタンスの停止／開始が必要です。

環境、`dev`、`stage` および `prod` の変更は、環境固有の設定が AEM によって正しく定義および解決されていることを、開発者が確認するのに役立ちます。ローカル開発は、主にデフォルトの `dev` 環境実行モードに対して行うことをお勧めします。

使用できる配列は次のとおりです。

| クイックスタート JAR のファイル名 | モードの説明 |
|------------------------------|-----------------------------------------------------------------------------|
| `aem-author-p4502.jar` | ポート 4502 での開発実行モードのオーサーとして |
| `aem-author_dev-p4502.jar` | ポート 4502 での開発実行モードのオーサーとして（`aem-author-p4502.jar` と同じ） |
| `aem-author_stage-p4502.jar` | ポート 4502 でのステージング実行モードのオーサーとして |
| `aem-author_prod-p4502.jar` | ポート 4502 での実稼動実行モードのオーサーとして |
| `aem-publish-p4503.jar` | ポート 4503 での開発実行モードのパブリッシュとして |
| `aem-publish_dev-p4503.jar` | ポート 4503 での開発実行モードのパブリッシュとして（`aem-publish-p4503.jar` と同じ） |
| `aem-publish_stage-p4503.jar` | ポート 4503 でのステージング実行モードのパブリッシュとして |
| `aem-publish_prod-p4503.jar` | ポート 4503 での実稼動実行モードのパブリッシュとして |

ポート番号は、ローカル開発マシンで使用可能な任意のポートにすることができますが、慣例により、次のようになります。

+ ポート __4502__ は&#x200B;__ローカル AEM オーサーサービス__&#x200B;用に使用されます
+ ポート __4503__ は&#x200B;__ローカル AEM パブリッシュサービス__ 用に使用されます

ポートの対応を変更するには、AEM SDK 設定の調整が必要になる場合があります。

## ローカル AEM ランタイムの停止

ローカルの AEM ランタイム（AEM オーサーサービスまたはパブリッシュサービス）を停止するには、AEM ランタイムの開始に使用したコマンドラインウィンドウを開き、`Ctrl-C` をタップします。AEM がシャットダウンするまで待ちます。シャットダウンプロセスが完了すると、コマンドラインプロンプトが表示されます。

## オプションのローカル AEM ランタイム設定タスク

+ __OSGi の設定環境変数と秘密変数__&#x200B;は、aio CLI では管理せず、[AEM ローカルランタイム](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=ja#local-development)用に特別に設定されています。

## クイックスタート Jar を更新するタイミング

AEM as a Cloud Service「機能リリース」のリリース周期、毎月最終木曜日以降または直後にAEM SDKを更新します。

>[!WARNING]
>
> クイックスタート Jar を新しいバージョンにアップグレードするには、ローカルの開発環境全体を置き換える必要があります。その結果、ローカル AEM リポジトリ内のコード、設定およびコンテンツはすべて失われます。破棄してはいけないコード、設定またはコンテンツを Git に安全にコミットするか、ローカルの AEM インスタンスから AEM パッケージとして書き出すようにします。

### AEM SDK のアップグレード時にコンテンツの損失を防ぐ方法

AEM SDK をアップグレードすると、新しいリポジトリを含む新しい AEM ランタイムが効果的に作成されます。つまり、以前の AEM SDK のリポジトリに対して実行された変更は失われます。AEM SDK のアップグレード間にコンテンツを持続させるための戦略を次に示します。個別に、または組み合わせて使用できます。

1. 開発に役立つ「サンプル」コンテンツを含む専用のコンテンツパッケージを作成し、Git で維持します。 AEM SDK のアップグレードを通じて保持する必要のあるコンテンツは、このパッケージに保持され、AEM SDK をアップグレードした後に再デプロイされます。
1. [oak-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html) と `includepaths` ディレクティブを使用して、以前の AEM SDK リポジトリから新しい AEM SDK リポジトリにコンテンツをコピーします。
1. 以前の AEM SDK で AEM パッケージマネージャーとコンテンツパッケージを使用してコンテンツをバックアップし、新しい AEM SDK に再インストールします。

AEM SDK のアップグレードの間に上記の方法を使用してコードを維持する場合、開発のアンチパターンを示すことに注意してください。使い捨て以外のコードは、開発 IDE から派生し、デプロイメントを介して AEM SDK に送られる必要があります。

## トラブルシューティング

### クイックスタート Jar ファイルをダブルクリックすると、エラーが発生する{#troubleshooting-double-click}

クイックスタート Jar をダブルクリックして起動すると、エラーモーダルが表示され、AEM がローカルで起動できなくなります。

![トラブルシューティング - クイックスタート Jar ファイルをダブルクリックする](./assets/aem-runtime/troubleshooting__double-click.png)

これは、AEM as a Cloud Service のクイックスタート Jar が、クイックスタート Jar をダブルクリックして AEM をローカルに起動することをサポートしていないためです。代わりに、コマンドラインから Jar ファイルを実行する必要があります。

AEM オーサーサービスを起動するには、`cd` を使用してクイックスタート Jar のあるディレクトリへと移動し、コマンドを実行します。

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java -jar aem-author-p4502.jar
```

>[!TAB Windows]

```shell
$ java -jar aem-author-p4502.jar
```

>[!TAB Linux®]

```shell
$ java -jar aem-author-p4502.jar
```

>[!ENDTABS]

または、AEM パブリッシュサービスを起動するには、`cd` でクイックスタート Jar が格納されているディレクトリに移動して、コマンドを実行します。

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java -jar aem-publish-p4503.jar
```

>[!TAB Windows]

```shell
$ java -jar aem-publish-p4503.jar
```

>[!TAB Linux®]

```shell
$ java -jar aem-publish-p4503.jar
```

>[!ENDTABS]

### コマンドラインからのクイックスタート Jar の起動がすぐに中止される{#troubleshooting-java-8}

クイックスタート Jar をコマンドラインから起動すると、処理がすぐに中断し、AEM サービスが起動せず、次のエラーが発生します。

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

これは、AEM as a Cloud Service には Java™ SDK 11 が必要で、異なるバージョン（おそらく Java™ 8）を実行しているためです。この問題を解決するには、[Oracle Java™ SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/jp/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;fulltext=Oracle%7E+JDK%7E+11%7E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) をダウンロードしてインストールします。

Oracle Java™ 11 SDK がインストールされたら、コマンドラインからコマンドを実行して、その SDK がアクティブなバージョンであることを確認します。

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ java --version
```

>[!TAB Windows]

```shell
$ java -version
```

>[!TAB Linux®]

```shell
$ java --version
```

>[!ENDTABS]

## その他のリソース

+ [AEM SDK をダウンロード](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Docker のダウンロード](https://www.docker.com/)
+ [Experience Manager Dispatcher のドキュメント](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=ja)
