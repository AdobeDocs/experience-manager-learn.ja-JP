---
title: 構築と導入
description: AdobeCloud Managerは、コードの構築とAEMへのCloud Serviceとしてのデプロイを容易にします。 エラーは、ビルドプロセスのステップ中に発生し、解決のために操作が必要になる場合があります。 このガイドでは、展開時に発生する一般的なエラーと、その最適なアプローチ方法について詳しく説明します。
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5434
thumbnail: kt-5424.jpg
translation-type: tm+mt
source-git-commit: a405cf14d3f71bf51e32e50c828c3216d29aa253
workflow-type: tm+mt
source-wordcount: '2517'
ht-degree: 0%

---


# Cloud Serviceの構築と展開としてのAEMのデバッグ

AdobeCloud Managerは、コードの構築とAEMへのCloud Serviceとしてのデプロイを容易にします。 エラーは、ビルドプロセスのステップ中に発生し、解決のために操作が必要になる場合があります。 このガイドでは、展開時に発生する一般的なエラーと、その最適なアプローチ方法について詳しく説明します。

![Cloud Manageビルドパイプライン](./assets/build-and-deployment/build-pipeline.png)

## 検証

検証手順では、Cloud Managerの基本的な設定が有効であることを確認するだけです。 一般的な検証エラーには、次のものがあります。

### 環境が無効な状態です

+ __エラーメッセージ：__ 環境は無効な状態です。
   ![環境が無効な状態です](./assets/build-and-deployment/validation__invalid-state.png)
+ __原因：__ パイプラインのターゲット環境は、移行中の状態で、新しいビルドを受け入れることができません。
+ __解像度：__ 状態が実行中（または更新可能）の状態に解決されるまで待ちます。 環境を削除する場合は、環境を再作成するか、別の環境を選択して作成します。

### パイプラインに関連付けられた環境が見つかりません

+ __エラーメッセージ：__ 環境は削除済みとマークされます。
   ![環境は削除済みとマークされます](./assets/build-and-deployment/validation__environment-marked-as-deleted.png)
+ __原因：__ パイプラインが使用するように構成されている環境が削除されました。
同じ名前の新しい環境が再作成されても、Cloud Managerは、同じ名前の環境にパイプラインを自動的に再関連付けしません。
+ __解像度：__ パイプラインコンフィギュレーションを編集し、デプロイ先の環境を再選択します。

### パイプラインに関連付けられたGitブランチが見つかりません

+ __エラーメッセージ：__ 無効なパイプライン：XXXXX. Reason=Branch=xxxxがリポジトリに見つかりません。
   ![無効なパイプライン：XXXXX. Reason=Branch=xxxxがリポジトリに見つかりません](./assets/build-and-deployment/validation__branch-not-found.png)
+ __原因：__ パイプラインが使用するように構成されているGitブランチが削除されました。
+ __解像度：__ 見つからないGitブランチを同じ名前で再作成するか、別の既存のブランチから構築するようにパイプラインを再設定します。

## ビルドおよび単体テスト

![構築と単体テスト](./assets/build-and-deployment/build-and-unit-testing.png)

ビルドとユニットのテスト段階では、パイプラインの構成済みGitブランチからチェックアウトされたプロジェクトのMavenビルド(`mvn clean package`)が実行されます。

このフェーズで識別されるエラーは、次の例外を除き、ローカルでのプロジェクト構築の再生産が可能です。

+ Maven Centralで使用できないMaven [依存関係が使用され](https://search.maven.org/) 、依存関係を含むMavenリポジトリは次のいずれかです。
   + プライベートの内部Mavenリポジトリなど、Cloud Managerからの未到達、またはMavenリポジトリに認証が必要で、正しくない秘密鍵証明書が提供されています。
   + プロジェクトのに明示的に登録されていません `pom.xml`。 Mavenリポジトリを含めると、構築時間が長くなるので、使用しないことに注意してください。
+ タイミングの問題が原因で、単体テストが失敗しました。 これは、ユニットテストがタイミングに依存する場合に発生する可能性があります。 強力なインジケーターは、テストコード `.sleep(..)` に依存しています。
+ サポートされていないMavenプラグインの使用。

## コードスキャン

![コードスキャン](./assets/build-and-deployment/code-scanning.png)

コードスキャンは、JavaとAEM固有のベストプラクティスを組み合わせて使用し、静的コード分析を実行します。

コードにCritical Securityの脆弱性が存在する場合、コードをスキャンするとビルドエラーが発生します。 小さい違反は上書きできますが、修正することをお勧めします。 コードのスキャンは不完全で、 [偽陽性が発生する可能性があることに注意してください](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/understand-test-results.html#dealing-with-false-positives)。

コードスキャンの問題を解決するには、Cloud Managerから提供されるCSV形式のレポートを「 **詳細をダウンロード** 」ボタンからダウンロードし、エントリを確認します。

詳しくは、AEM固有のルールを参照してください。Cloud Managerドキュメントの [カスタムAEM固有のコードスキャンルール](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/custom-code-quality-rules.html)。

## 画像を作成

![画像を作成](./assets/build-and-deployment/build-images.png)

ビルド画像は、ビルドとユニットのテストの手順で作成されたビルド済みのコードアーティファクトとAEMリリースを組み合わせて、デプロイ可能な単一のアーティファクトを形成する役割を持ちます。

コードの構築とコンパイルの問題はビルドと単体テストで見つかりますが、カスタムビルドの成果物をAEMリリースと組み合わせようとすると、設定や構造上の問題が発生する可能性があります。

### 重複OSGi設定

複数のOSGi設定が、ターゲットAEM環境の実行モードで解決される場合、のビルド手順は次のエラーで失敗します。

```
[ERROR] Unable to convert content-package [/tmp/packages/enduser.all-1.0-SNAPSHOT.zip]: 
Configuration ‘com.example.ExampleComponent’ already defined in Feature Model ‘com.example.groupId:example.all:slingosgifeature:xxxxx:X.X’, 
set the ‘mergeConfigurations’ flag to ‘true’ if you want to merge multiple configurations with same PID
```

#### 原因1

+ __原因：__ AEMプロジェクトのすべてのパッケージには複数のコードパッケージが含まれ、同じOSGi設定が複数のコードパッケージで提供されているため、競合が発生し、イメージのビルド手順でどちらを使用すべきかを判断できず、ビルドに失敗します。 一意の名前が付けられている限り、OSGiファクトリ設定には適用されません。
+ __解像度：__ AEMアプリケーションの一部として展開されているすべてのコードパッケージ（付属のサードパーティコードパッケージを含む）を確認し、実行モードを介してターゲット環境に解決される重複OSGi設定を探します。 エラーメッセージの「mergeConfigurationsフラグをtrueに設定」は、AEMではクラウドサービスとして使用できないので、無視する必要があります。

#### 原因2

+ __原因：__ AEMプロジェクトに同じコードパッケージが誤って2回含まれている場合、そのパッケージに含まれるOSGi設定が複製されます。
+ __解像度：__ すべてのプロジェクトに埋め込まれたすべてのpom.xmlのパッケージを確認し、 `filevault-package-maven-plugin` 設定がに [設定されていることを確認し](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html#cloud-manager-target)`<cloudManagerTarget>none</cloudManagerTarget>`ます。

### 誤ったレポイントスクリプト

レポイントスクリプトは、ベースラインコンテンツ、ユーザー、ACLなどを定義します。 AEMではCloud Serviceとして、レポイントスクリプトはビルドイメージの間に適用されますが、AEM SDKのローカルクイックスタートでは、OSGiレポイントファクトリ設定がアクティブ化されるときに適用されます。 このため、AEM SDKのローカルクイックスタートで、Repointスクリプトが（ログを使用して）静かに失敗し、イメージのビルド手順が失敗してデプロイメントが停止する場合があります。

+ __原因：__ レポイントスクリプトの形式が正しくありません。 失敗したスクリプトがリポジトリに対して実行される後は、すべてのレポジトリスクリプトがリポジトリに対して実行されるので、リポジトリは不完全な状態のままになる可能性があります。
+ __解像度：__ レポイントスクリプトOSGi設定がデプロイされている場合にAEM SDKのローカルクイックスタートを確認し、エラーの有無と内容を確認します。

### 未満のレポイントコンテンツの依存関係

レポイントスクリプトは、ベースラインコンテンツ、ユーザー、ACLなどを定義します。 AEM SDKのローカルクイックスタートでは、リポジトリがアクティブになった後、またはコンテンツの直接変更やコンテンツパッケージ経由でコンテンツが変更された後に、リポイントスクリプトが適用されます。 AEMでは、Cloud Serviceとして、レポイントスクリプトは、レポイントスクリプトが依存するコンテンツを含まないリポジトリに対して、イメージの構築中に適用されます。

+ __原因：__ レポイントスクリプトは、存在しないコンテンツに依存します。
+ __解像度：__ レポイントスクリプトが依存するコンテンツが存在することを確認します。 これは、多くの場合、欠落しているが必要なコンテンツ構造を定義するディレクティブが欠けている、適切に定義されていないレポイントスクリプトを示しています。 これは、AEMを削除し、JARを解凍し、repointスクリプトを含むリポイントOSGi設定をinstallフォルダーに追加して、AEMを起動することで、ローカルで再現できます。 このエラーは、AEM SDKのローカルクイックスタートのerror.logに表示されます。


### アプリケーションのコアコンポーネントのバージョンがデプロイ済みのバージョンよりも大きい

_この問題は、最新のAEMリリースに対して自動更新されない非実稼働環境にのみ影響します。_

AEMは、各AEMリリースで最新のコアコンポーネントバージョンが自動的にCloud Serviceに含まれます。つまり、Cloud Service環境としてのAEMが自動的または手動で更新された後は、最新バージョンのコアコンポーネントがデプロイされます。

次の場合に「イメージをビルド」ステップが失敗する可能性があります。

+ デプロイアプリケーションは、 `core` （OSGiバンドル）プロジェクトの依存バージョンに対応したコアコンポーネントを更新します
+ 展開アプリケーションは、新しいコアコンポーネントバージョンを含むAEMリリースを使用するように更新されていないCloud Service環境としてサンドボックス（実稼働環境以外）AEMに展開されます。

この障害を防ぐには、Cloud Service環境としてAEMのアップデートが利用可能な場合は常に、次のビルド/デプロイの一環としてアップデートを含め、必ずアプリケーションコードベースのコアコンポーネントのバージョンを増やしてからアップデートを含めます。

+ __症状：__「Build Image（イメージをビルド）」の手順は、「 
`com.adobe.cq.wcm.core.components...` 特定のバージョン範囲のパッケージを `core` プロジェクトで読み込めませんでした。

   ```
   [ERROR] Bundle com.example.core:0.0.3-SNAPSHOT is importing package(s) Package com.adobe.cq.wcm.core.components.models;version=[12.13,13) in start level 20 but no bundle is exporting these for that start level in the required version range.
   [ERROR] Analyser detected errors on feature 'com.adobe.granite:aem-ethos-app-image:slingosgifeature:aem-runtime-application-publish-dev:1.0.0-SNAPSHOT'. See log output for error messages.
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD FAILURE
   [INFO] ------------------------------------------------------------------------
   ```

+ __原因：__ アプリケーションのOSGiバンドル（プロジェクトで定義）は、コアコンポーネントのコア依存関係から、AEMにCloud Serviceとしてデプロイされているものとは異なるバージョンレベルでJavaクラスをインポートします。 `core`
+ __解像度:__
   + Gitを使用して、コアコンポーネントのバージョンが増加する前に存在する作業コミットに戻します。 このコミットをCloud Manager Gitブランチにプッシュし、このブランチから環境の更新を実行します。 これにより、AEMがCloud Serviceとして最新のAEMリリースにアップグレードされます。リリースには、最新のコアコンポーネントバージョンが含まれます。 Cloud ServiceとしてのAEMが最新のAEMリリースに更新され、最新のコアコンポーネントバージョンがリリースされたら、最初に失敗したコードを再デプロイします。
   + この問題をローカルで再現するには、AEM SDKのバージョンが、AEMとCloud Service環境が使用しているAEMのリリースバージョンと同じであることを確認してください。


### Adobeサポートケースの作成

上記のトラブルシューティング方法で問題が解決しない場合は、次の方法でAdobeサポートケースを作成してください。

+ [Adobe Admin Console](https://adminconsole.adobe.com) /「サポート」タブ/ケースを作成

   _複数のAdobe組織のメンバーの場合は、ケースを作成する前に、Adobe組織スイッチャーで、失敗したパイプラインを持つAdobe組織が選択されていることを確認します。_

## 展開先

「展開先」の手順では、ビルド画像で生成されたコードアーティファクトを取得し、開始がそれを使用して新しいAEM作成者と発行サービスを作成し、成功した場合は、古いAEM作成者と発行サービスを削除します。 この手順では、可変コンテンツのパッケージとインデックスもインストールおよび更新します。

「配置先」の手順をデバッグする前に、 [AEMをCloud Serviceログとして確認し](./logs.md) 、 ログには、ポッドの開始のアップとシャットダウンに関する情報が含まれています。この情報は、問題へのデプロイに関する情報である可能性があります。 `aemerror` Cloud Managerの展開先ステップの「ログをダウンロード」ボタンから利用できるログはログではなく、アプリケーションの開始に関する詳細な情報は含まれていないことに注意してください。 `aemerror`

![展開先](./assets/build-and-deployment/deploy-to.png)

次の3つの主な原因で、「Deploy to」ステップが失敗する場合があります。

### Cloud Managerパイプラインには、古いAEMバージョンが含まれています

+ __原因：__ Cloud Managerのパイプラインには、ターゲット環境にデプロイされるAEMよりも古いバージョンが含まれています。 これは、パイプラインが再使用され、新しいバージョンのAEMを実行している新しい環境を指し示す場合に発生する可能性があります。 これは、環境のAEMのバージョンがパイプラインのAEMのバージョンより大きいかどうかを確認することで識別できます。
   ![Cloud Managerパイプラインには、古いAEMバージョンが含まれています](./assets/build-and-deployment/deploy-to__pipeline-holds-old-aem-version.png)
+ __解像度:__
   + ターゲット環境に利用可能な更新がある場合は、環境のアクションから「更新」を選択し、ビルドを再実行します。
   + ターゲット環境に利用可能なアップデートがない場合は、最新バージョンのAEMが実行されています。 この問題を解決するには、パイプラインを削除し、再作成します。


### Cloud Managerがタイムアウトになりました

新しくデプロイされたAEMサービスの開始中に実行されるコードは、長い時間がかかるので、デプロイを完了する前にCloud Managerがタイムアウトします。 この場合、Cloud Managerのステータスが「失敗」と報告された場合でも、最終的に展開が成功する可能性があります。

+ __原因：__ カスタムコードは、大きなクエリやコンテンツトラーバージョンなどの操作を、OSGiバンドルやコンポーネントのライフサイクルの早い段階でトリガーされ、AEMの開始アップ時間を大幅に遅延させる場合があります。
+ __解像度：__ OSGi Bundleのライフサイクルの初めに実行されるコードの実装を確認し、Cloud Managerで示されるように、失敗前後のAEM AuthorおよびPublishサービスの `aemerror` ログ(ログ時間(GMT))を確認し、カスタムログ実行プロセスを示すログメッセージを探します。

### 互換性のないコードまたは設定

ほとんどのコードと設定の違反は、ビルドの前のバージョンで検出されますが、カスタムコードや設定では、Cloud ServiceとしてAEMとの互換性がなく、コンテナで実行されるまで検出されません。

+ __原因：__ カスタムコードを使用すると、大規模なクエリやコンテンツトラーバージョンなど、長い操作がOSGiバンドルの初期段階でトリガーされたり、コンポーネントのライフサイクルがAEMの開始を大幅に遅延させたりする場合があります。
+ __解像度：__ Cloud Managerで示されているように、失敗(ログ時間(GMT))前後に、AEM AuthorおよびPublishサービスの `aemerror` ログを確認します。
   1. ログで、カスタムアプリケーションが提供するJavaクラスがスローしたERRORを確認します。 問題が見つかった場合は、解決し、修正済みのコードをプッシュして、パイプラインを再構築します。
   1. カスタムアプリケーションで拡張/対話を行うAEMの側面で報告されたERRORがログにないか確認し、それらを調べます。これらのエラーは、Javaクラスに直接関連付けられない場合があります。 問題が見つかった場合は、解決し、修正済みのコードをプッシュして、パイプラインを再構築します。

### コンテンツパッケージに/varを含める

`/var` は、様々な一過性のランタイムコンテンツを含む可変です。 コンテンツ `/var` パッケージに含める(例： `ui.content`)をCloud Manager経由でデプロイすると、デプロイの手順が失敗する場合があります。

この問題は、最初の展開でエラーが発生することはなく、後続の展開でのみ識別が困難です。 顕著な症状は次のとおりです。

+ ただし、初期のデプロイメントは成功しましたが、デプロイメントに含まれる新しいまたは変更可能なコンテンツは、AEM Publishサービスに存在しないように見えます。
+ AEM Author内のコンテンツのアクティベーション/アクティベーション解除がブロックされています
+ その後のデプロイは、「展開先」の手順で失敗し、「展開先」の手順は約60分後に失敗します。

この問題を検証するには、次の手順に従います。

1. 展開の一部である少なくとも1つのコンテンツパッケージを特定すると、に書き込みが行われ `/var`ます。
1. 次の場所でプライマリ（太字の）配布キューがブロックされていることを確認します。
   + AEM Author/ツール/デプロイメント/配布
      ![ブロックされた配布キュー](./assets/build-and-deployment/deploy-to__var--distribution.png)
1. 後続のデプロイメントが失敗した場合は、「ログをダウンロード」ボタンを使用して、Cloud Managerの「デプロイ先」ログをダウンロードします。

   ![ログへの展開のダウンロード](./assets/build-and-deployment/deploy-to__var--download-logs.png)

   ...を実行し、ログ・ステートメント間に約60分があることを確認します。

   ```
   2020-01-01T01:01:02+0000 Begin deployment in aem-program-x-env-y-dev [CorrelationId: 1234]
   ```

   ... および ...

   ```
   2020-01-01T02:04:10+0000 Failed deployment in aem-program-x-env-y-dev
   ```

   このログには、最初のデプロイメントで成功とレポートされるインジケータは含まれず、失敗した後のデプロイメントの場合にのみ表示されます。

+ __原因：__ コンテンツパッケージをAEM発行サービスに展開するために使用されるAEMレプリケーションサービスユーザーは、AEM発行に書き込むこ `/var` とができません。 この結果、AEM Publishサービスへのコンテンツパッケージのデプロイメントは失敗します。
+ __解像度：__ この問題を解決する次の方法は、優先順に表示されます。
   1. リソースが不要な場合は、 `/var` コンテンツパッケージから、アプリケーションの一部としてデプロイされ `/var` ているリソースをすべて削除します。
   2. リソースが必要な場合は、リポイントを使用してノード構造を定義し `/var` ます [](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit)。 レポイントスクリプトは、OSGi実行モードを介して、AEM Author、AEM Publish、またはその両方を対象にすることができます。
   3. リソースがAEM作成者上でのみ必要で、レポイントを使用して適切にモデル化できない場合は `/var` 、AEM Author Runmodeフォルダー( [)にパッケージを](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/deploying/overview.html#repoinit)埋め込むことでAEM Authorにインストールされる [](https://docs.adobe.com/content/help/en/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html#embeddeds)`all``<target>/apps/example-packages/content/install.author</target>`、個別のコンテンツパッケージに移動します。

### Adobeサポートケースの作成

上記のトラブルシューティング方法で問題が解決しない場合は、次の方法でAdobeサポートケースを作成してください。

+ [Adobe Admin Console](https://adminconsole.adobe.com) /「サポート」タブ/ケースを作成

   _複数のAdobe組織のメンバーの場合は、ケースを作成する前に、Adobe組織スイッチャーで、失敗したパイプラインを持つAdobe組織が選択されていることを確認します。_
