---
title: 開発者コンソール
description: AEM as a Cloud Serviceには、デバッグに役立つ、実行中のAEMサービスの様々な詳細を表示する各環境用の開発者コンソールが用意されています。
feature: Developer Tools
topics: development
version: Cloud Service
doc-type: tutorial
activity: develop
audience: developer
kt: 5433
thumbnail: kt-5433.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 0499ff9f-d452-459f-b1a2-2853a228efd1
source-git-commit: e8e5c67f6e9f057fd7472b76ee09d7f87b133c89
workflow-type: tm+mt
source-wordcount: '1414'
ht-degree: 2%

---

# 開発者コンソールでas a Cloud ServiceしたAEMのデバッグ

AEM as a Cloud Serviceには、デバッグに役立つ、実行中のAEMサービスの様々な詳細を表示する各環境用の開発者コンソールが用意されています。

各AEMas a Cloud Service環境には、独自の開発者コンソールがあります。

## 開発者コンソールへのアクセス

開発者コンソールにアクセスして使用するには、 [AdobeAdmin Console](https://adminconsole.adobe.com).

1. Cloud Manager およびAEMas a Cloud Service製品に影響を与えたAdobe組織が、Adobe組織切り替えボタンでアクティブになっていることを確認します。
1. 開発者は、 [Cloud Manager 製品の __開発者 —Cloud Service__ 製品プロファイル](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-cloud-manager.html#assign-developer).
   + このメンバーシップが存在しない場合、開発者は開発者コンソールにログインできません。
1. 開発者は、 [__AEM Users__ または __AEM Administrators__ AEM オーサーまたはパブリッシュの製品プロファイル](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/onboarding/journey/assign-profiles-aem.html#aem-product-profiles).
   + このメンバーシップが存在しない場合、 [ステータス](#status) ダンプが 401 Unauthorized エラーでタイムアウトします。

### 開発者コンソールへのアクセスのトラブルシューティング

#### 401 ステータスのダンピング時に未認証エラーが発生しました

![デベロッパーコンソール — 401 未認証](./assets/developer-console/troubleshooting__401-unauthorized.png)

ステータスのダンプに 401 Unauthorized エラーが報告される場合、AEM as a Cloud Serviceで必要な権限を持つユーザーがまだ存在していないか、使用するログイントークンが無効か、期限切れです。

401 Unauthorized の問題を解決するには：

1. ユーザーが、開発者コンソールに関連付けられているAEMas a Cloud Serviceの製品インスタンスに適したAdobe IMS製品プロファイル (AEM管理者またはAEMユーザー ) のメンバーであることを確認します。
   + 開発者コンソールは 2 つの製品インスタンスにアクセスするAdobe IMSに注意してください。AEMas a Cloud Serviceのオーサーインスタンスとパブリッシュ製品インスタンスの場合、開発者コンソールを介したアクセスが必要なサービス層に応じて、正しい製品プロファイルが使用されていることを確認します。
1. AEM as a Cloud Service（オーサーまたはパブリッシュ）にログインし、ユーザーとグループがAEMに正しく同期されていることを確認します。
   + 開発者コンソールでは、ユーザーレコードを対応するAEMサービス層に作成し、そのサービス層に対して認証できるようにする必要があります。
1. ブラウザーの Cookie とアプリケーションの状態（ローカルストレージ）を消去し、開発者コンソールに再ログインします。アクセストークン開発者コンソールが正しく、期限切れでないことを確認します。

## ポッド

AEMas a Cloud Serviceのオーサーサービスとパブリッシュサービスは、トラフィックの変動とローリング更新をダウンタイムなしに処理するために、それぞれ複数のインスタンスで構成されています。 これらのインスタンスは、ポッドと呼ばれます。 開発者コンソールでのポッドの選択は、他のコントロールを使用して公開されるデータの範囲を定義します。

![開発者コンソール — ポッド](./assets/developer-console/pod.png)

+ ポッドは、AEM Service（オーサーまたはパブリッシュ）の一部である個別のインスタンスです
+ ポッドは一時的なもので、AEMas a Cloud Serviceが必要に応じて作成および破棄します
+ 関連するAEMas a Cloud Service環境の一部であるポッドのみが、その環境の開発者コンソールのポッドスイッチャーに一覧表示されます。
+ ポッドスイッチャーの下部にある、便利なオプションで、サービスタイプ別にポッドを選択できます。
   + すべての発言者
   + すべての発行者
   + すべてのインスタンス

## ステータス

Status は、特定のAEMランタイム状態をテキスト出力または JSON 出力で出力するオプションを提供します。 開発者コンソールは、AEM SDK のローカルクイックスタートの OSGi Web コンソールと同様の情報を提供しますが、開発者コンソールは読み取り専用です。

![開発者コンソール — ステータス](./assets/developer-console/status.png)

### バンドル

バンドルには、AEM内のすべての OSGi バンドルが一覧表示されます。 この機能は、 [AEM SDK のローカルクイックスタートの OSGi バンドル](http://localhost:4502/system/console/bundles) 時刻 `/system/console/bundles`.

バンドルは次の方法でデバッグに役立ちます。

+ AEM as a Service にデプロイされたすべての OSGi バンドルのリスト
+ 各 OSGi バンドルの状態のリスト。アクティブかどうかを含む
+ OSGi バンドルがアクティブにならなくなる原因となる未解決の依存関係に詳細を指定する

### コンポーネント

コンポーネントは、AEM内のすべての OSGi コンポーネントをリストします。 この機能は、 [AEM SDK のローカルクイックスタートの OSGi コンポーネント](http://localhost:4502/system/console/components) 時刻 `/system/console/components`.

コンポーネントは、次の方法でデバッグに役立ちます。

+ AEM as a Cloud Serviceにデプロイされたすべての OSGi コンポーネントのリスト
+ 各 OSGi コンポーネントの状態の指定それらがアクティブか不満かを含む
+ 満たされていないサービス参照に詳細を指定すると、OSGi コンポーネントがアクティブにならなくなる場合があります
+ OSGi コンポーネントにバインドされた OSGi プロパティとその値のリスト。
   + これは、 [OSGi 環境設定変数](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values).

### 設定

設定には、すべての OSGi コンポーネントの設定（OSGi のプロパティと値）が一覧表示されます。 この機能は、 [AEM SDK のローカルクイックスタートの OSGi Configuration Manager](http://localhost:4502/system/console/configMgr) 時刻 `/system/console/configMgr`.

設定は、次の方法でデバッグに役立ちます。

+ OSGi コンポーネントによる OSGi プロパティとその値のリスト
   + で挿入された実際の値は表示されません。 [OSGi 環境設定変数](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html#environment-specific-configuration-values). 詳しくは、 [コンポーネント](#components) 上に、インジェクトされた値を示します。
+ 誤って設定されたプロパティの特定と識別

### Oak インデックス

Oak インデックスは、の下に定義されたノードのダンプを提供します。 `/oak:index`. これは、AEMインデックスが変更されたときに発生する、結合インデックスを表示しないことに注意してください。

Oak インデックスは次の方法でデバッグに役立ちます。

+ AEMでの検索クエリの実行方法に関するインサイトを提供する、すべての Oak インデックスの定義のリスト。 AEMインデックスに変更した内容は、ここには反映されません。 このビューは、AEMでのみ提供されるインデックス、またはカスタムコードでのみ提供されるインデックスに対してのみ役立ちます。

### OSGi サービス

コンポーネントは、すべての OSGi サービスをリストします。 この機能は、 [AEM SDK のローカルクイックスタートの OSGi サービス](http://localhost:4502/system/console/services) 時刻 `/system/console/services`.

OSGi サービスのデバッグヘルプ：

+ AEMでのすべての OSGi サービスのリスト。提供される OSGi バンドルと、それを使用するすべての OSGi バンドル。

### Sling ジョブ

Sling ジョブに、すべての Sling ジョブキューが表示されます。 この機能は、 [AEM SDK のローカルクイックスタートのジョブ](http://localhost:4502/system/console/slingevent) 時刻 `/system/console/slingevent`.

Sling ジョブは、次の方法でデバッグに役立ちます。

+ Sling ジョブキューとその設定のリスト
+ アクティブな、キューに登録され、処理された Sling ジョブの数に関するインサイトを提供する。これは、AEMの Sling ジョブで実行されるワークフロー、一時的なワークフロー、その他の作業に関する問題のデバッグに役立ちます。

## Java パッケージ

Java パッケージを使用すると、Java パッケージとバージョンがAEM as a Cloud Serviceで使用できるかどうかを確認できます。 この機能は、 [AEM SDK のローカルクイックスタートの依存関係ファインダー](http://localhost:4502/system/console/depfinder) 時刻 `/system/console/depfinder`.

![デベロッパーコンソール — Java パッケージ](./assets/developer-console/java-packages.png)

Java パッケージは、未解決のインポートまたはスクリプト（HTL、JSP など）の未解決のクラスが原因で、バンドルが開始されない問題を撮影するために使用されます。 Java パッケージレポートにバンドルがエクスポートされない場合（または OSGi バンドルによってインポートされたバージョンとバージョンが一致しない場合）:

+ プロジェクトのAEM API maven 依存関係のバージョンが、環境のAEMリリースバージョンと一致することを確認します（可能な場合は、すべてを最新に更新します）。
+ Maven プロジェクトで追加の Maven 依存関係が使用される場合
   + AEM SDK API の依存関係で提供される代替 API を代わりに使用できるかどうかを判断します。
   + 追加の依存関係が必要な場合は、プレーン JAR ではなく OSGi バンドルとして提供され、プロジェクトのコードパッケージ (`ui.apps`) と同様の機能を持ちます。 `ui.apps` パッケージ。

## サーブレット

サーブレットを使用して、AEMが URL を解決する Java サーブレットまたはスクリプト (HTL、JSP) への URL 解決方法に関するインサイトを提供し、最終的に要求を処理します。 この機能は、 [AEM SDK のローカルクイックスタートの Sling Servlet Resolver](http://localhost:4502/system/console/servletresolver) 時刻 `/system/console/servletresolver`.

![開発者コンソール — サーブレット](./assets/developer-console/servlets.png)

サーブレットは、デバッグに役立ち、次の情報を確認できます。

+ URL がそのアドレス可能な部分（リソース、セレクター、拡張子）に分解される方法。
+ URL が解決されるサーブレットまたはスクリプト。正しくない形式の URL や誤って登録されたサーブレット/スクリプトを識別するのに役立ちます。

## クエリ

クエリは、AEMでの検索クエリの実行内容と方法に関するインサイトを提供します。 この機能は、  [AEM SDK のローカルクイックスタートのツール/クエリパフォーマンス ](http://localhost:4502/libs/granite/operations/content/diagnosistools/queryPerformance.html) コンソール。

クエリは、特定のポッドが選択された場合にのみ機能し、そのポッドのクエリパフォーマンス Web コンソールが開きます。開発者は、AEMサービスにログインするためのアクセス権を持つ必要があります。

![開発者コンソール — クエリ — クエリの説明](./assets/developer-console/queries__explain-query.png)

クエリは、次の方法でデバッグに役立ちます。

+ Oak によるクエリの解釈、分析、実行方法を説明する。 これは、クエリの処理速度が遅い理由と速度を把握する際に非常に重要です。
+ AEMで実行されている最も一般的なクエリと、そのクエリの説明を表示する機能を提供する。
+ AEMで実行中の最も遅いクエリを「説明」機能と共に表示する。
