---
title: ビルドとデプロイメント
description: Adobe Cloud Manager は、AEM as a Cloud Service へのコード構築とデプロイメントを容易にします。ビルドプロセスの手順中にエラーが発生し、解決のためのアクションが必要になる場合があります。 このガイドでは、デプロイメントでの一般的なエラーと、それらへの最適なアプローチ方法について説明します。
feature: Developer Tools
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-5434
thumbnail: kt-5424.jpg
topic: Development
role: Developer
level: Beginner
exl-id: b4985c30-3e5e-470e-b68d-0f6c5cbf4690
duration: 534
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '2476'
ht-degree: 100%

---

# AEM as a Cloud Service のビルドとデプロイメントでのデバッグ

Adobe Cloud Manager は、AEM as a Cloud Service へのコード構築とデプロイメントを容易にします。ビルドプロセスの手順中にエラーが発生し、解決のためのアクションが必要になる場合があります。 このガイドでは、デプロイメントでの一般的なエラーと、それらへの最適なアプローチ方法について説明します。

![Cloud Manager ビルドパイプライン](./assets/build-and-deployment/build-pipeline.png)

## 検証

検証手順では、基本的な Cloud Manager 設定が有効であることを確認するだけです。一般的な検証エラーは次のとおりです。

### 環境が無効な状態です

+ __エラーメッセージ：__環境が無効な状態です。
  ![環境が無効な状態です](./assets/build-and-deployment/validation__invalid-state.png)
+ __原因：__&#x200B;パイプラインのターゲット環境は、新しいビルドを受け入れることができない移行状態にあります。
+ __解決策：__&#x200B;実行中（または更新が利用可能）になるまで待ちます。環境が削除されている場合は、環境を再作成するか、別の環境を選択してビルドします。

### パイプラインに関連付けられている環境が見つかりません

+ __エラーメッセージ：__環境が削除済みとマークされています。
  ![環境が削除済みとマークされています](./assets/build-and-deployment/validation__environment-marked-as-deleted.png)
+ __原因：__パイプラインで使用するように設定された環境が削除されました。
同じ名前の新しい環境が再作成されても、Cloud Manager は、パイプラインを同じ名前の環境に自動的に再関連付けしません。
+ __解決策：__&#x200B;パイプライン設定を編集し、デプロイ先の環境を再選択します。

### パイプラインに関連付けられた Git ブランチが見つかりません

+ __エラーメッセージ：__無効なパイプライン：XXXXX。Reason=Branch=xxxx がリポジトリ内に見つかりません。
  ![無効なパイプライン：XXXXX。Reason=Branch=xxxx がリポジトリに見つかりません](./assets/build-and-deployment/validation__branch-not-found.png)
+ __原因：__&#x200B;パイプラインで使用するように設定された Git ブランチが削除されました。
+ __解決策：__&#x200B;見つからない Git ブランチを全く同じ名前で再作成するか、別の既存のブランチからビルドするようにパイプラインを再設定します。

## ビルドおよび単体テスト

![ビルドおよび単体テスト](./assets/build-and-deployment/build-and-unit-testing.png)

ビルドおよび単体テストフェーズでは、パイプラインの設定済み Git ブランチからチェックアウトされたプロジェクトの Maven ビルド（`mvn clean package`）を実行します。

このフェーズで特定されたエラーは、次の例外を除き、ローカルでプロジェクトをビルドすることで再現できます。

+ [Maven Central](https://search.maven.org/) で利用できない Maven 依存関係が使用され、依存関係を含む Maven リポジトリは次のいずれかになります。
   + プライベートな内部 Maven リポジトリなど、Cloud Manager から到達できないか、Maven リポジトリに認証が必要で、不正な資格情報が提供されています。
   + プロジェクトの `pom.xml` に明示的に登録されていません。Maven リポジトリを含めるとビルド時間が長くなるので、推奨されません。
+ タイミングの問題が原因で単体テストが失敗します。これは、単体テストがタイミングに依存する場合に発生する可能性があります。 強力な指標は、テストコードで `.sleep(..)` に依存しています。
+ 未対応の Maven プラグインの使用。

## コードスキャン

![コードスキャン](./assets/build-and-deployment/code-scanning.png)

コードスキャンは、Java と AEM 固有のベストプラクティスを組み合わせて使用し、静的コード分析を実行します。

コードに重大なセキュリティの脆弱性が存在する場合、コードスキャンはビルドに失敗します。比較的小さな違反は無視できますが、修正することをお勧めします。コードスキャンは不完全であり、[誤検知](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/test-results/overview-test-results.html?lang=ja#dealing-with-false-positives)が発生する可能性があることに注意してください。

コードスキャンの問題を解決するには、Cloud Manager が提供する CSV 形式のレポートを「**詳細をダウンロード**」ボタンからダウンロードし、エントリを確認します。

AEM 固有のルールについて詳しくは、Cloud Manager のドキュメント「[AEM 固有のカスタムのコードスキャンルール](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/how-to-use/custom-code-quality-rules.html?lang=ja)」を参照してください。

## 画像を作成

![画像を作成](./assets/build-and-deployment/build-images.png)

ビルドイメージは、ビルドと単体テストの手順で作成されたビルドコードアーティファクトと AEM リリースを組み合わせ、デプロイ可能な単一のアーティファクトを形成する役割を持ちます。

コードのビルドとコンパイルの問題はビルドと単体テストで見つかりますが、カスタムビルドのアーティファクトと AEM リリースを組み合わせようとすると、設定や構造上の問題が見つかる場合があります。

### OSGi 設定の重複

複数の OSGi 設定がターゲットの AEM 環境の実行モードで解決された場合、イメージのビルド手順が次のエラーで失敗します。

```
[ERROR] Unable to convert content-package [/tmp/packages/enduser.all-1.0-SNAPSHOT.zip]: 
Configuration 'com.example.ExampleComponent' already defined in Feature Model 'com.example.groupId:example.all:slingosgifeature:xxxxx:X.X', 
set the 'mergeConfigurations' flag to 'true' if you want to merge multiple configurations with same PID
```

#### 原因 1

+ __原因：__ AEM プロジェクトの all パッケージには複数のコードパッケージが含まれ、同じ OSGi 設定が複数のコードパッケージで提供されているので競合が発生し、イメージのビルド手順はどちらを使用するかを決定できずビルドに失敗します。一意の名前がある限り、これは OSGi ファクトリ設定には適用されません。
+ __解決策：__ AEM アプリケーションの一部としてデプロイされているすべてのコードパッケージ（サードパーティのコードパッケージを含みます）を確認し、実行モードを介してターゲット環境に解決される、重複した OSGi 設定を探します。AEM as a Cloud Service では、エラーメッセージの「mergeConfigurations フラグを true に設定する」というガイダンスは使用できないので無視します。

#### 原因 2

+ __原因：__ AEM プロジェクトに同じコードパッケージが誤って 2 回含まれていると、そのパッケージに含まれる OSGi 設定が重複します。
+ __解決策：__&#x200B;すべてのプロジェクトに埋め込まれているすべての pom.xml パッケージを確認し、`filevault-package-maven-plugin` [ 設定 ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html?lang=ja#cloud-manager-target) が `<cloudManagerTarget>none</cloudManagerTarget>` に設定されていることを確認します。

### 不正な repoinit スクリプト

repoinit スクリプトは、ベースラインコンテンツ、ユーザー、ACL などを定義します。AEM as a Cloud Service では、repoinit スクリプトはイメージのビルド中に適用されますが、AEM SDK のローカルクイックスタートでは、OSGi repoinit のファクトリ設定がアクティブ化されると適用されます。このため、AEM SDK のローカルクイックスタートで repoinit スクリプトが静かに失敗し（ログに記録されます）、イメージのビルド手順が失敗してデプロイが停止する場合があります。

+ __原因：__ repoinit スクリプトの形式が正しくありません。これにより、失敗したスクリプトの後の repoinit スクリプトがリポジトリに対して実行されないため、リポジトリが不完全な状態のままになる可能性があります。
+ __解決策：__ repoinit スクリプトの OSGi 設定がデプロイされている場合は、AEM SDK のローカルクイックスタートを確認して、エラーの有無と内容を特定します。

### 満たされていない repoinit コンテンツの依存関係

repoinit スクリプトは、ベースラインコンテンツ、ユーザー、ACL などを定義します。AEM SDK のローカルクイックスタートでは、repoinit OSGi ファクトリ設定がアクティブになったとき、つまり、リポジトリがアクティブで、直接またはコンテンツパッケージを介してコンテンツの変更が発生した場合に、repoinit スクリプトが適用されます。AEM as a Cloud Service では、repoinit スクリプトが依存するコンテンツを含まない可能性のあるリポジトリに対して、イメージのビルド中に repoinit スクリプトが適用されます。

+ __原因：__ repoinit スクリプトが依存しているコンテンツが存在していません。
+ __解決策：__ repoinit スクリプトが依存するコンテンツが、存在することを確認します。多くの場合、これは repoinit スクリプトの定義が不適切で、欠落している必要なコンテンツ構造を定義するディレクティブがないことを示しています。これをローカルで再現するには、AEM を削除し、JAR を展開し、repoinit スクリプトを含む repoinit OSGi 設定をインストールフォルダーに追加して AEM を起動します。エラーは、AEM SDK のローカルクイックスタートの error.log に表示されます。


### アプリケーションのコアコンポーネントのバージョンが、デプロイされたバージョンよりも新しい

_この問題が影響するのは、最新の AEM リリースに自動更新されない、実稼動以外の環境のみです。_

AEM as a Cloud Service では、すべての AEM リリースで最新のコアコンポーネントバージョンが自動的に含まれます。つまり、AEM as a Cloud Service 環境が自動または手動で更新された後に、最新バージョンのコアコンポーネントがデプロイされます。

次の場合に、イメージのビルドの手順が失敗する可能性があります。

+ デプロイするアプリケーションは、`core`（OSGi バンドル）プロジェクトのコアコンポーネント Maven 依存関係バージョンを更新します
+ その後、デプロイアプリケーションは、新しいコアコンポーネントバージョンを含む AEM リリースを使用するように更新されていない、サンドボックス（非実稼動）の AEM as a Cloud Service 環境にデプロイされます。

このエラーを防ぐには、AEM as a Cloud Service 環境のアップデートが使用可能な場合は常に、次のビルドまたはデプロイの一部としてアップデートを含め、アプリケーションコードベースでコアコンポーネントのバージョンを繰り上げた後に必ずアップデートを含めます。

+ __症状：__
イメージのビルド手順が失敗し、特定のバージョン範囲の `com.adobe.cq.wcm.core.components...` パッケージを `core` プロジェクトで読み込めないというエラーが通知されます。

  ```
  [ERROR] Bundle com.example.core:0.0.3-SNAPSHOT is importing package(s) Package com.adobe.cq.wcm.core.components.models;version=[12.13,13) in start level 20 but no bundle is exporting these for that start level in the required version range.
  [ERROR] Analyser detected errors on feature 'com.adobe.granite:aem-ethos-app-image:slingosgifeature:aem-runtime-application-publish-dev:1.0.0-SNAPSHOT'. See log output for error messages.
  [INFO] ------------------------------------------------------------------------
  [INFO] BUILD FAILURE
  [INFO] ------------------------------------------------------------------------
  ```

+ __原因：__&#x200B;アプリケーションの OSGi バンドル（`core` プロジェクトで定義）は、AEM as a Cloud Service にデプロイされたものとは異なるバージョンレベルで、コアコンポーネントのコア依存関係から Java クラスを読み込みます。
+ __解決策：__
   + Git を使用して、コアコンポーネントのバージョンを繰り上げる前に存在する作業用コミットに戻します。このコミットを Cloud Manager Git ブランチにプッシュし、このブランチから環境の更新を実行します。これにより、AEM as a Cloud Service が最新の AEM リリースにアップグレードされ、最新のコアコンポーネントバージョンが含まれます。AEM as a Cloud Service が最新の AEM リリースに更新され、最新のコアコンポーネントバージョンが含まれるようになったら、最初に失敗したコードを再デプロイします。
   + この問題をローカルで再現するには、AEM SDK のバージョンが、AEM as a Cloud Service 環境で使用されている AEM のリリースバージョンと同じであることを確認します。


### アドビサポートケースの作成

上記のトラブルシューティング方法で問題が解決しない場合は、次の方法でアドビサポートケースを作成してください。

+ [Adobe Admin Console](https://adminconsole.adobe.com)／「サポート」タブ／ケースを作成

  _複数のアドビ組織のメンバーの場合は、ケースを作成する前に、アドビ組織スイッチャーで、失敗したパイプラインを持つアドビ組織が選択されていることを確認します。_

## デプロイ

「デプロイ」手順では、ビルド画像で生成されたコードアーティファクトを取得し、それを使用して新しい AEM オーサーサービスとパブリッシュサービスを開始し、成功したら、古い AEM オーサーサービスとパブリッシュサービスを削除します。 可変コンテンツパッケージとインデックスも、この手順でインストールおよび更新します。

「デプロイ」手順をデバッグする前に、[AEM as a Cloud Service ログ](./logs.md)について学んでください。`aemerror` ログには、「デプロイ」の問題に関連する可能性のあるポッドの起動および停止に関する情報が含まれています。 Cloud Manager の「デプロイ」手順の「ログをダウンロード」ボタンから利用できるログは、 `aemerror` ログではなく、アプリケーションの起動に関する詳細情報は含まれません。

![デプロイ](./assets/build-and-deployment/deploy-to.png)

「デプロイ」手順が失敗する主な 3 つの理由は次の通りです。

### Cloud Manager のパイプラインに古い AEM バージョンが含まれる

+ __原因__：Cloud Manager のパイプラインに、ターゲット環境にデプロイされる AEM よりも古いバージョンの CD が含まれます。 これは、新しいバージョンの AEM を実行している新しい環境をパイプラインが再利用し、指すように設定された場合に発生する可能性があります。 これは、環境の AEM のバージョンがパイプラインの AEM のバージョンよりも大きいかどうかを確認することで識別できます。
  ![Cloud Manager のパイプラインに古い AEM バージョンが含まれる](./assets/build-and-deployment/deploy-to__pipeline-holds-old-aem-version.png)
+ __解決策：__
   + ターゲット環境で「更新可能」が設定されている場合は、環境のアクションから「更新」を選択し、ビルドを再実行します。
   + 対象の環境で「更新可能」が設定されていない場合、最新バージョンの AEM が実行されています。 これを解決するには、パイプラインを削除し、再作成します。


### Cloud Manager がタイムアウトする

新しくデプロイされた AEM サービスの起動中にコードを実行すると、デプロイが完了する前に Cloud Manager がタイムアウトするほど時間がかかります。この場合、Cloud Manager のステータスが「失敗」とレポートされても、最終的にデプロイメントが成功する可能性があります。

+ __原因__：カスタムコードは、OSGi バンドルまたはコンポーネントのライフサイクルの早い段階でトリガーされた大きなクエリやコンテンツトラバーサルなどの操作を実行し、AEM の起動時間を大幅に遅らせる場合があります。
+ __解決策__：OSGi バンドルのライフサイクルの初期に実行されるコードの実装を確認し、Cloud Manager で示されるように、失敗前後の AEM オーサーサービスおよびパブリッシュサービスの `aemerror` ログ（GMT でのログ時間）と、カスタムログ実行プロセスを示すログメッセージを探します。

### 互換性のないコードまたは設定

ほとんどのコードおよび設定違反はビルドの前半でキャッチされますが、カスタムコードまたは構成が AEM as a Cloud Service と互換性がなく、コンテナで実行されるまで検出されない可能性があります。

+ __原因__：カスタムコードを使用すると、大きなクエリやコンテンツトラバーサルなどの、AEM の起動時間が OSGi バンドルやコンポーネントのライフサイクルの早い段階でトリガーされる長い操作によって大幅に遅延する場合があります。
+ __解決策__：Cloud Manager によって示されるように、AEM オーサーサービスとパブリッシュ サービスの `aemerror` ログを確認します。
   1. ログで、カスタムアプリケーションで提供される Java クラスでスローされたエラーを確認します。 問題が見つかった場合は、解決し、修正されたコードをプッシュして、パイプラインを再構築します。
   1. カスタムアプリケーションで拡張または操作している AEM の側面によって報告されたエラーのログを確認し、それらを調査します。 これらのエラーは、Java クラスに直接起因するものではない可能性があります。問題が見つかった場合は、解決し、修正されたコードをプッシュして、パイプラインを再構築します。

### コンテンツパッケージに /var を含める

`/var` は、さまざまな一時的なランタイムコンテンツを含む変更可能です。`/var` を Cloud Manager を介してデプロイされたコンテンツパッケージ（例：`ui.content`）に含めると、「デプロイ」手順が失敗する場合があります。

この問題は、最初のデプロイメントでは失敗せず、後続のデプロイメントでのみ失敗するので、特定が困難です。 顕著な症状は次のとおりです。

+ 初期デプロイメントは成功しますが、デプロイメントの一部である新規または変更された可変コンテンツが AEM Publish サービスに存在しないように見えます。
+ AEM オーサーでのコンテンツのアクティベート／アクティベート解除がブロックされている
+ その後のデプロイメントは「デプロイ」手順で失敗し、「デプロイ」の手順は約 60 分後に失敗する。

この問題を検証するには、次のように動作が失敗する原因が考えられます。

1. デプロイメントに含まれるコンテンツパッケージを少なくとも 1 つ特定し、`/var` に書き込みます。
1. 次の場所で、プライマリ（太字の）配布キューがブロックされていることを確認します。
   + AEM オーサー／ツール／デプロイメント／配布
     ![ブロックされた配布キュー](./assets/build-and-deployment/deploy-to__var--distribution.png)
1. 後続のデプロイメントに失敗した場合は、「ログをダウンロード」ボタンを使用して、Cloud Manager の「デプロイ」ログをダウンロードします。

   ![「デプロイ」ログをダウンロードする](./assets/build-and-deployment/deploy-to__var--download-logs.png)

   ログステートメントの間に約 60 分があることを確認します。

   ```
   2020-01-01T01:01:02+0000 Begin deployment in aem-program-x-env-y-dev [CorrelationId: 1234]
   ```

   ... および ...

   ```
   2020-01-01T02:04:10+0000 Failed deployment in aem-program-x-env-y-dev
   ```

   このログでは、これらの指標は、成功したと報告された最初のデプロイメントのもの含まれず、後続の失敗したデプロイメントのもののみが含まれます。

+ __原因__：コンテンツパッケージを AEM パブリッシュサービスにデプロイするために使用される AEM のレプリケーションサービスユーザーは、AEM パブリッシュの `/var` に書き込むことができません。その結果、AEM パブリッシュサービスへのコンテンツパッケージのデプロイメントに失敗します。
+ __解像度：__&#x200B;以下に、この問題の解決方法を優先順に一覧表示しています。
   1. `/var`リソースが不要な場合は、アプリケーションの一部としてデプロイされたコンテンツパッケージの下にある`/var`すべてのリソースを削除します。
   2. `/var` リソースが必要な場合は、[repoinit](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html?lang=ja#repoinit) を使用してノード構造を定義します。repoinit スクリプトは、OSGi 実行モードを介して、AEM オーサー、AEM パブリッシュ、またはその両方をターゲットにすることができます。
   3. `/var` リソースが AEM オーサーでのみ必要であり、[repoinit](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/overview.html?lang=ja#repoinit) を使用して合理的にモデル化できない場合は、個別のコンテンツパッケージに移動します。これにより、AEM オーサー実行モードフォルダー（`<target>/apps/example-packages/content/install.author</target>`）の `all` パッケージに[埋め込み](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html?lang=ja#embeddeds)、AEM オーサーにのみインストールされます。
   4. この [Adobe KB](https://helpx.adobe.com/jp/experience-manager/kb/cm/cloudmanager-deploy-fails-due-to-sling-distribution-aem.html) で説明されているように、`sling-distribution-importer` サービスユーザーに適切な ACL を指定します。

### アドビサポートケースの作成

上記のトラブルシューティング方法で問題が解決しない場合は、次の方法でアドビサポートケースを作成してください。

+ [Adobe Admin Console](https://adminconsole.adobe.com)／「サポート」タブ／ケースを作成

  _複数の Adobe 組織のメンバーである場合は、ケースを作成する前に、パイプラインが失敗している Adobe 組織が Adobe 組織スイッチャーで選択されていることを確認します。_
