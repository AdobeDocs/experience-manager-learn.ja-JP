---
title: AEMas a Cloud Service開発用のローカルAEMランタイムの設定
description: AEM as a Cloud Service SDK の Quickstart Jar を使用して、Local AEM Runtime を設定します。
feature: Developer Tools
version: Cloud Service
kt: 4678, 4677
thumbnail: 32551.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 19f72254-2087-450b-909d-2d90c9821486
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1800'
ht-degree: 4%

---

# ローカルAEM Runtime の設定 {#set-up-local-aem-runtime}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_aemruntime"
>title="Local AEM Runtime"
>abstract="Adobe Experience Manager(AEM) は、AEM as a Cloud Service SDK の Quickstart Jar を使用して、ローカルで実行できます。 これにより、開発者は、カスタムコード、設定およびコンテンツをソース管理にコミットする前に、カスタムコード、設定およびコンテンツをデプロイおよびテストし、AEMas a Cloud Service環境にデプロイできます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-as-a-cloud-service-sdk.html?lang=ja" text="AEM as a Cloud Service の SDK"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html" text="AEMas a Cloud ServiceSDK のダウンロード"

Adobe Experience Manager(AEM) は、AEM as a Cloud Service SDK の Quickstart Jar を使用して、ローカルで実行できます。 これにより、開発者は、カスタムコード、設定およびコンテンツをソース管理にコミットする前に、カスタムコード、設定およびコンテンツをデプロイおよびテストし、AEMas a Cloud Service環境にデプロイできます。

注意： `~` は、ユーザーのディレクトリの略記法として使用されます。 Windows の場合、これは `%HOMEPATH%`.

## Java のインストール

Experience Managerは Java アプリケーションなので、開発ツールをサポートするために Java SDK が必要です。

1. [最新の Java SDK 11 をダウンロードしてインストールする](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Autoling&amp;fulltext=Oracle%7E+JDK%7E+11%E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14)
1. 次のコマンドを実行して、Java 11 SDK がインストールされていることを確認します。
   + Windows:`java -version`
   + macOS/Linux: `java --version`

![Java](./assets/aem-runtime/java.png)

## AEMas a Cloud ServiceSDK のダウンロード

AEMas a Cloud ServiceSDK(AEM SDK) には、AEM オーサーとパブリッシュを開発用にローカルで実行するためのクイックスタート JAR、および互換性のあるバージョンの Dispatcher ツールが含まれています。

1. にログインします。 [https://experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads) Adobe ID
   + お使いのAdobe組織 __必須__ AEM as a Cloud Service SDK をダウンロードするためにAEM as a Cloud Serviceがプロビジョニングされている。
1. 次に移動： __AEMas a Cloud Service__ タブ
1. 並べ替え基準 __公開日__ in __降順__ 注文
1. 最新の __AEM SDK__ 結果行
1. EULA を確認して同意し、 __ダウンロード__ ボタン

## AEM SDK zip から Quickstart Jar を抽出します。

1. ダウンロードしたを解凍します。 `aem-sdk-XXX.zip` ファイル

## ローカルの AEM オーサーサービスを設定する{#set-up-local-aem-author-service}

ローカルの AEM オーサーサービスは、デベロッパーに、ローカルエクスペリエンスのデジタルマーケターやコンテンツ作成者が共有してコンテンツを作成および管理できるようにします。  AEM オーサーサービスは、オーサリングとプレビューの両方の環境として設計されており、それに対してほとんどの機能開発の検証を実行でき、ローカル開発プロセスの重要な要素となります。

1. フォルダーを作成 `~/aem-sdk/author`
1. を __クイックスタート JAR__ ～に提出する  `~/aem-sdk/author` に変更し、 `aem-author-p4502.jar`
1. コマンドラインから次のコマンドを実行して、ローカルの AEM オーサーサービスを開始します。
   + `java -jar aem-author-p4502.jar`
      + 管理者パスワードをとして指定します。 `admin`. admin パスワードはどれでも使用できますが、再設定の必要性を減らすために、ローカル開発のデフォルトを使用することをお勧めします。

   あなた *できません* AEMをCloud ServiceQuickstart Jar として起動 [ダブルクリックで](#troubleshooting-double-click).
1. ローカルの AEM オーサーサービス ( ) にアクセスします。 [http://localhost:4502](http://localhost:4502) Web ブラウザー内

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

## ローカル AEM パブリッシュサービスを設定する

ローカルの AEM パブリッシュサービスは、AEM上にホストされている Web サイトの参照など、AEMのエンドユーザーが持つローカルエクスペリエンスを開発者に提供します。 ローカルの AEM パブリッシュサービスは、AEM SDK の [Dispatcher ツール](./dispatcher-tools.md) また、開発者は、エンドユーザーに対する最終的なエクスペリエンスをスモークテストと微調整できます。

1. フォルダーを作成 `~/aem-sdk/publish`
1. を __クイックスタート JAR__ ～に提出する  `~/aem-sdk/publish` に変更し、 `aem-publish-p4503.jar`
1. コマンドラインから次のコマンドを実行して、ローカルの AEM パブリッシュサービスを開始します。
   + `java -jar aem-publish-p4503.jar`
      + 管理者パスワードをとして指定します。 `admin`. admin パスワードはどれでも使用できますが、再設定の必要性を減らすために、ローカル開発のデフォルトを使用することをお勧めします。

   あなた *できません* AEMをCloud ServiceQuickstart Jar として起動 [ダブルクリックで](#troubleshooting-double-click).
1. ローカルの AEM パブリッシュサービス ( ) にアクセスします。 [http://localhost:4503](http://localhost:4503) Web ブラウザー内

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

## プレリリースモードでローカルAEMサービスを設定する

ローカルのAEMランタイムは、 [プレリリースモード](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/release-notes/prerelease.html?lang=ja) AEM as a Cloud Serviceの次期リリースの機能に基づいて開発者がを構築できるようになりました。 プレリリースは、 `-r prerelease` ローカルAEMランタイムの最初の開始時の引数。 これは、ローカルの AEM オーサーサービスと AEM パブリッシュサービスの両方で使用できます。

```shell
# For AEM Author service in prerelease mode
$ java -jar aem-author-p4502.jar -r prerelease

# For AEM Publish service in prerelease mode
$ java -jar aem-publish-p4503.jar -r prerelease
```

## コンテンツ配布をシミュレート {#content-distribution}

真のCloud Service環境では、コンテンツは、 [Sling コンテンツ配布](https://sling.apache.org/documentation/bundles/content-distribution.html) とAdobeパイプライン この [Adobeパイプライン](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/core-concepts/architecture.html?lang=en#content-distribution) は、クラウド環境でのみ使用できる独立したマイクロサービスです。

開発時に、ローカルのオーサーサービスとパブリッシュサービスを使用して、コンテンツの配布をシミュレートする方が望ましい場合があります。 これは、従来のレプリケーションエージェントを有効にすることで実現できます。

>[!NOTE]
>
> レプリケーションエージェントは、ローカルのクイックスタート JAR でのみ使用でき、コンテンツ配布のシミュレーションのみ提供します。

1. にログインします。 **作成者** サービスにアクセスし、 [http://localhost:4502/etc/replication/agents.author.html](http://localhost:4502/etc/replication/agents.author.html).
1. クリック **デフォルトエージェント (publish)** をクリックして、デフォルトのレプリケーションエージェントを開きます。
1. クリック **編集** エージェントの設定を開きます。
1. 以下 **設定** 「 」タブで、次のフィールドを更新します。

   + **有効** - true をチェック
   + **エージェントユーザー ID**  — このフィールドは空のままにします

   ![レプリケーションエージェントの設定 — 設定](assets/aem-runtime/settings-config.png)

1. 以下 **輸送** 「 」タブで、次のフィールドを更新します。

   + **URI** - `http://localhost:4503/bin/receive?sling:authRequestLogin=1`
   + **ユーザー** - `admin`
   + **パスワード** - `admin`

   ![レプリケーションエージェントの設定 — トランスポート](assets/aem-runtime/transport-config.png)

1. クリック **Ok** 設定を保存し、 **デフォルト** レプリケーションエージェント。
1. これで、Author サービスのコンテンツに変更を加えて、それらを Publish サービスに公開できます。

![ページを公開](assets/aem-runtime/publish-page-changes.png)

## クイックスタート JAR の起動モード

クイックスタート JAR の名前 `aem-<tier>_<environment>-p<port number>.jar` 起動方法を指定します。 AEMを特定の層、オーサーまたはパブリッシュで開始した後は、別の層に変更できません。 これをおこなうには、 `crx-Quickstart` 最初の実行時に生成されたフォルダーを削除し、クイックスタート JAR を再度実行する必要があります。 環境とポートは変更できますが、ローカルのAEMインスタンスの停止/開始が必要です。

環境の変更 `dev`, `stage` および `prod`は、開発者がAEMで環境固有の設定を正しく定義して解決するのに役立ちます。 主にデフォルトのローカル開発に対しておこなうことをお勧めします `dev` 環境実行モード。

利用可能な順列は次のとおりです。

| クイックスタート JAR のファイル名 | モードの説明 |
|------------------------------|-----------------------------------------------------------------------------|
| `aem-author-p4502.jar` | ポート 4502 での開発実行モードのオーサーとして |
| `aem-author_dev-p4502.jar` | ポート 4502 での開発実行モードの作成者として ( 同じ `aem-author-p4502.jar`) |
| `aem-author_stage-p4502.jar` | ポート 4502 でのステージング実行モードのオーサーとして |
| `aem-author_prod-p4502.jar` | ポート 4502 の実稼動実行モードのオーサーとして |
| `aem-publish-p4503.jar` | ポート 4503 での Dev 実行モードでの公開 |
| `aem-publish_dev-p4503.jar` | ポート 4503 での Dev 実行モードでの公開 ( `aem-publish-p4503.jar`) |
| `aem-publish_stage-p4503.jar` | ポート 4503 でのステージング実行モードでの公開 |
| `aem-publish_prod-p4503.jar` | ポート 4503 での実稼動実行モードでの公開 |

ポート番号は、ローカル開発・マシン上の使用可能な任意のポートにすることができますが、次の規則に従います。

+ ポート __4502__ が __ローカル AEM オーサーサービス__
+ ポート __4503__ が __ローカル AEM パブリッシュサービス__

これらを変更するには、AEM SDK 設定の調整が必要になる場合があります

## ローカルAEMランタイムの停止

ローカルのAEMランタイム（AEM オーサーまたはパブリッシュサービス）を停止するには、AEM Runtime の開始に使用したコマンドラインウィンドウを開き、をタップします。 `Ctrl-C`. AEMがシャットダウンするまで待ちます。 シャットダウンプロセスが完了すると、コマンドラインプロンプトが表示されます。

## オプションのローカルAEMランタイム設定タスク

+ __OSGi 設定の環境変数とシークレット変数__ が [AEMローカルランタイム用に特別に設定](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html?lang=ja#local-development)aio CLI を使用して管理するのではなく、

## クイックスタート JAR を更新するタイミング

AEMas a Cloud Serviceの「機能リリース」のリリースケイデンスである、毎月または毎月の最後の木曜日以降にAEM SDK を更新します。

>[!WARNING]
>
> クイックスタート JAR を新しいバージョンに更新するには、ローカル開発環境全体を置き換える必要があります。その結果、ローカルAEMリポジトリ内のすべてのコード、設定およびコンテンツが失われます。 破棄するべきでないコード、設定またはコンテンツが、Git に安全にコミットされるか、AEMパッケージとしてローカルのAEMインスタンスから書き出されることを確認します。

### AEM SDK のアップグレード時にコンテンツが失われるのを防ぐ方法

AEM SDK をアップグレードすると、新しいリポジトリを含む新しいAEMランタイムが効果的に作成されます。つまり、以前のAEM SDK のリポジトリに対しておこなった変更は失われます。 次に、AEM SDK のアップグレード間でコンテンツを永続化するのに役立つ実行可能な戦略を示します。これらは、任意に、または協調して使用できます。

1. 開発に役立つ「サンプル」コンテンツを含む専用のコンテンツパッケージを作成し、Git で維持します。 AEM SDK のアップグレードを通じて保持する必要のあるコンテンツは、このパッケージに保持され、AEM SDK をアップグレードした後に再デプロイされます。
1. 用途 [oak-upgrade](https://jackrabbit.apache.org/oak/docs/migration.html) と `includepaths` ディレクティブを使用して、以前のAEM SDK リポジトリから新しいAEM SDK リポジトリにコンテンツをコピーします。
1. 以前のAEM SDK でAEM Package Manager とコンテンツパッケージを使用してコンテンツをバックアップし、新しいAEM SDK に再インストールします。

AEM SDK のアップグレードの間に上記の方法を使用してコードを維持する場合、開発のアンチパターンを示すことに注意してください。 使い捨て以外のコードは、開発 IDE から派生し、デプロイメントを介してAEM SDK に送られる必要があります。

## トラブルシューティング

### クイックスタート JAR ファイルをダブルクリックすると、エラーが発生する{#troubleshooting-double-click}

Quickstart Jar をダブルクリックして起動すると、エラーモーダルが表示され、AEMがローカルで起動できなくなります。

![トラブルシューティング — Quickstart Jar ファイルをダブルクリックします。](./assets/aem-runtime/troubleshooting__double-click.png)

これは、AEMas a Cloud Serviceのクイックスタート JAR は、Quickstart JAR をダブルクリックしてローカルにAEMを起動する機能をサポートしていないためです。 代わりに、そのコマンドラインから Jar ファイルを実行する必要があります。

AEM オーサーサービスを開始するには、次の手順を実行します。 `cd` クイックスタート JAR を含むディレクトリに移動し、次のコマンドを実行します。

`$ java -jar aem-author-p4502.jar`

または、AEM パブリッシュサービスを開始するには、 `cd` クイックスタート JAR を含むディレクトリに移動し、次のコマンドを実行します。

`$ java -jar aem-publish-p4503.jar`

### コマンドラインからのクイックスタート JAR の起動は直ちに中止されます{#troubleshooting-java-8}

コマンドラインからクイックスタート JAR を起動すると、プロセスが直ちに中止され、AEMサービスが起動しません。次のエラーが発生します。

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

これは、AEM as a Cloud Serviceには Java SDK 11 が必要で、異なるバージョンを実行している（おそらく Java 8）ためです。 この問題を解決するには、をダウンロードしてインストールします。 [OracleJava SDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Autoling&amp;fulltext=Oracle%7E+JDK%7E+11%E&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14).
Java SDK 11 をインストールしたら、コマンドラインから次のコマンドを実行して、その SDK がアクティブバージョンであることを確認します。

Java 11 SDK がインストールされたら、コマンドラインからコマンドを実行して、その SDK がアクティブなバージョンであることを確認します。

+ Windows：`java -version`
+ macOS/Linux: `java --version`

## その他のリソース

+ [AEM SDK をダウンロード](https://experience.adobe.com/#/downloads)
+ [AdobeCloud Manager](https://my.cloudmanager.adobe.com/)
+ [Docker のダウンロード](https://www.docker.com/)
+ [Experience ManagerDispatcher のドキュメント](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=ja)
