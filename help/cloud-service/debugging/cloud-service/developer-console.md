---
title: 開発者コンソール
description: AEM as a Cloud Serviceは、デバッグに役立つ、実行中のAEMサービスの様々な詳細を表示する、各環境の開発者コンソールを提供します。
feature: 開発者ツール
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5433
thumbnail: kt-5433.jpg
topic: 開発
role: Developer
level: Beginner
source-git-commit: 048a37a9813e7b61ff069c4606b8d23cc6b6844f
workflow-type: tm+mt
source-wordcount: '1351'
ht-degree: 2%

---


# 開発者コンソールを使用したCloud ServiceとしてのAEMのデバッグ

AEM as a Cloud Serviceは、デバッグに役立つ、実行中のAEMサービスの様々な詳細を表示する、各環境の開発者コンソールを提供します。

各AEM as a Cloud Service環境には、独自の開発者コンソールがあります。

## 開発者コンソールへのアクセス

開発者コンソールにアクセスして使用するには、[AdobeのAdmin Console](https://adminconsole.adobe.com)を介して、開発者のAdobe IDに次の権限を付与する必要があります。

1. Cloud ManagerおよびAEMをCloud Service製品として有効にしたAdobe組織が、Adobe組織切り替えボタンでアクティブになっていることを確認します。
1. 開発者は、Cloud Manager製品の&#x200B;__開発者 —Cloud Service__&#x200B;製品プロファイルのメンバーである必要があります。
   + このメンバーシップが存在しない場合、開発者は開発者コンソールにログインできません。
1. 開発者は、AEMオーサーまたはパブリッシュ環境の&#x200B;__AEM Users__&#x200B;または&#x200B;__AEM Administrators__&#x200B;製品プロファイルのメンバーである必要があります。
   + このメンバーシップが存在しない場合、[status](#status)ダンプがタイムアウトし、401 Unauthorizedエラーが発生します。

### 開発者コンソールへのアクセスのトラブルシューティング

#### 401ダンピング状態時の未承認エラー

![開発者コンソール — 401未承認](./assets/developer-console/troubleshooting__401-unauthorized.png)

ステータスをダンプすると401 Unauthorizedというエラーが報告される場合は、Cloud ServiceとしてAEMに必要な権限がまだ存在しないか、使用するログイントークンが無効か期限切れであることを意味します。

401 Unauthorizedの問題を解決するには：

1. ユーザーが、開発者コンソールに関連付けられたAEM as aCloud Service製品インスタンス用の適切なAdobeIMS製品プロファイル(AEM管理者またはAEMユーザー)のメンバーであることを確認します。
   + 開発者コンソールは、2つのAdobeIMS製品インスタンスにアクセスします。AEM as aCloud Serviceオーサーおよびパブリッシュ製品インスタンスを使用するので、開発者コンソールを介したアクセスが必要なサービス層に応じて、正しい製品プロファイルが使用されていることを確認します。
1. AEMにCloud Service（オーサーまたはパブリッシュ）としてログインし、ユーザーとグループがAEMに正しく同期されていることを確認します。
   + 開発者コンソールでは、ユーザーレコードを対応するAEMサービス層に作成し、そのサービス層に対して認証する必要があります。
1. ブラウザーのCookieとアプリケーションの状態（ローカルストレージ）を消去し、開発者コンソールに再ログインして、デベロッパーコンソールで使用されているアクセストークンが正しく、期限切れでないことを確認します。

## ポッド

AEM as a Traffic AuthorサービスとPublishサービスは、ダウンタイムなしにトラフィックの変動とローリング更新を処理するために、それぞれ複数のインスタンスで構成されます。 これらのインスタンスは、ポッドと呼ばれます。 開発者コンソールでのポッド選択は、他のコントロールを使用して公開されるデータの範囲を定義します。

![開発者コンソール — ポッド](./assets/developer-console/pod.png)

+ ポッドは、AEMサービス（オーサーまたはパブリッシュ）の一部である個別のインスタンスです
+ ポッドは一時的で、必要に応じてAEM as a Cloud Serviceが作成および破棄します
+ 関連するAEM as aCloud Service環境の一部であるポッドのみが、その環境の開発者コンソールのポッドスイッチャーに一覧表示されます。
+ ポッド切り替えボタンの下部に、サービスタイプ別にポッドを選択できる便利なオプションがあります。
   + すべての発言者
   + すべての発行者
   + すべてのインスタンス

## ステータス

「ステータス」には、特定のAEMランタイム状態をテキスト出力またはJSON出力で出力するオプションが表示されます。 開発者コンソールは、AEM SDKのローカルクイックスタートのOSGi Webコンソールと同様の情報を提供しますが、開発者コンソールは読み取り専用です。

![開発者コンソール — ステータス](./assets/developer-console/status.png)

### バンドル

バンドルには、AEM内のすべてのOSGiバンドルが一覧表示されます。 この機能は、[AEM SDKのローカルクイックスタートのOSGi Bundles](http://localhost:4502/system/console/bundles)(`/system/console/bundles`)に似ています。

バンドルは次の方法でデバッグに役立ちます。

+ AEM as a ServiceにデプロイされたすべてのOSGiバンドルのリスト
+ 各OSGiバンドルの状態のリストアクティブかどうかを含む
+ OSGiバンドルがアクティブにならない原因となる未解決の依存関係への詳細の提供

### コンポーネント

コンポーネントは、AEM内のすべてのOSGiコンポーネントをリストします。 この機能は、[AEM SDKのローカルクイックスタートのOSGiコンポーネント](http://localhost:4502/system/console/components)(`/system/console/components`)と似ています。

コンポーネントは次の方法でデバッグに役立ちます。

+ AEM as a Cloud ServiceとしてデプロイされたすべてのOSGiコンポーネントのリスト
+ 各OSGiコンポーネントの状態の提供それらがアクティブであるか不満であるかを含む
+ 未満のサービス参照の詳細を指定すると、OSGiコンポーネントがアクティブにならなくなる場合があります
+ OSGiコンポーネントにバインドされたOSGiプロパティとその値のリスト

### 設定

設定には、すべてのOSGiコンポーネントの設定（OSGiのプロパティと値）が一覧表示されます。 この機能は、[AEM SDKのローカルクイックスタートのOSGi Configuration Manager](http://localhost:4502/system/console/configMgr)(`/system/console/configMgr`)と似ています。

設定は次の方法でデバッグに役立ちます。

+ OSGiコンポーネントによるOSGiプロパティとその値のリスト
+ 誤った設定のプロパティの特定と識別

### Oak インデックス

Oakインデックスは、`/oak:index`の下に定義されたノードのダンプを提供します。 これは、AEMインデックスが変更されたときに発生する、結合されたインデックスを表示しないことに注意してください。

Oakインデックスは次の方法でデバッグに役立ちます。

+ AEMでの検索クエリの実行方法に関するインサイトを提供する、すべてのOakインデックス定義のリスト。 AEMインデックスに変更した内容は、ここには反映されません。 この表示は、AEMでのみ提供されるインデックス、またはカスタムコードでのみ提供されるインデックスに対してのみ役立ちます。

### OSGiサービス

コンポーネントは、すべてのOSGiサービスをリストします。 この機能は、[AEM SDKのローカルクイックスタートのOSGi Services](http://localhost:4502/system/console/services)(`/system/console/services`)に似ています。

OSGiサービスは、次の方法でデバッグする際に役立ちます。

+ AEM内のすべてのOSGiサービスと、その提供OSGiバンドル、およびそれを使用するすべてのOSGiバンドルのリスト

### Sling ジョブ

Slingジョブに、すべてのSlingジョブキューがリストされます。 この機能は、[AEM SDKのローカルクイックスタートのジョブ](http://localhost:4502/system/console/slingevent)(`/system/console/slingevent`)と似ています。

Slingジョブは、次の方法でデバッグに役立ちます。

+ Slingジョブキューとその設定のリスト
+ アクティブで、キューに登録され、処理されたSlingジョブの数に関するインサイトを提供する。これは、AEMのSlingジョブで実行されるワークフロー、一時的なワークフロー、その他の作業に関する問題のデバッグに役立ちます。

## Javaパッケージ

Javaパッケージを使用すると、JavaパッケージとバージョンがAEM as aCloud Serviceで使用可能かどうかを確認できます。 この機能は、[AEM SDKのローカルクイックスタートの依存関係ファインダー](http://localhost:4502/system/console/depfinder)(`/system/console/depfinder`)と同じです。

![開発者コンソール — Javaパッケージ](./assets/developer-console/java-packages.png)

Javaパッケージは、未解決のインポートまたはスクリプト（HTL、JSPなど）の未解決のクラスが原因で、バンドルが開始されない撮影の問題に使用されます。 Javaパッケージがバンドルをエクスポートしない場合（またはOSGiバンドルによって読み込まれたバージョンとバージョンが一致しない場合）:

+ プロジェクトのAEM API maven依存関係のバージョンが、環境のAEMリリースバージョンと一致することを確認します（可能な場合は、すべてを最新に更新します）。
+ Mavenプロジェクトで追加のMaven依存関係が使用される場合
   + AEM SDK APIの依存関係で提供される代替APIを代わりに使用できるかどうかを確認します。
   + 追加の依存関係が必要な場合は、コアOSGiバンドルを`ui.apps`パッケージに埋め込むのと同様に、OSGiバンドルとして（プレーンJARではなく）提供し、プロジェクトのコードパッケージ(`ui.apps`)に埋め込まれていることを確認します。

## サーブレット

サーブレットを使用して、AEMがURLを解決するJavaサーブレットまたはスクリプト(HTL、JSP)が最終的に要求を処理する方法に関するインサイトを提供します。 この機能は、[AEM SDKのローカルクイックスタートのSling Servlet Resolver](http://localhost:4502/system/console/servletresolver)(`/system/console/servletresolver`)と同じです。

![開発者コンソール — サーブレット](./assets/developer-console/servlets.png)

サーブレットは、デバッグに役立ち、次の情報を確認できます。

+ URLをアドレス可能な部分（リソース、セレクター、拡張子）に分解する方法。
+ URLが解決されるサーブレットまたはスクリプト。形式が正しくないURLや、誤って登録されたサーブレット/スクリプトを識別するのに役立ちます。

## クエリ

クエリは、AEMでの検索クエリの内容と実行方法に関するインサイトを提供します。 この機能は、AEM SDKのローカルクイックスタートのツール/クエリパフォーマンス](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html)コンソールと同じです。[

クエリは、特定のポッドが選択されている場合にのみ機能します。ポッドのクエリパフォーマンスWebコンソールが開き、開発者はAEMサービスにログインするためのアクセス権を持つ必要があります。

![開発者コンソール — クエリ — クエリの説明](./assets/developer-console/queries__explain-query.png)

クエリは、次の方法でデバッグに役立ちます。

+ Oakによるクエリの解釈、分析、実行方法の説明 これは、クエリが遅い理由と速度を追跡する際に非常に重要です。
+ AEMで実行されている最も一般的なクエリと、そのクエリの説明を表示する機能。
+ AEMで実行中の最も遅いクエリを表示し、それらのクエリを説明する機能を備えている。
