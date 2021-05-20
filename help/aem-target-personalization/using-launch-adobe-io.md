---
title: Experience Platform LaunchとAdobe I/Oを使用したAdobe Experience ManagerとAdobe Targetの統合
seo-title: Experience Platform LaunchとAdobe I/Oを使用したAdobe Experience ManagerとAdobe Targetの統合
description: Experience Platform LaunchとAdobe I/Oを使用してAdobe Experience ManagerとAdobe Targetを統合する方法を順を追って説明します
seo-description: Experience Platform LaunchとAdobe I/Oを使用してAdobe Experience ManagerとAdobe Targetを統合する方法を順を追って説明します
feature: エクスペリエンスフラグメント
topic: パーソナライズ機能
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1101'
ht-degree: 4%

---


# Adobe I/Oコンソールを使用したAdobe Experience Platform Launchの使用

## 前提条件

* [AEMオーサーインスタンスとパブリッシュイン](./implementation.md#set-up-aem) スタンスがそれぞれlocalhostポート4502と4503で実行される
* **Experience Cloud**
   * 組織へのアクセスAdobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * 次のソリューションでプロビジョニングされたExperience Cloud
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe I/Oコンソール](https://console.adobe.io)

      >[!NOTE]
      >Launchで開発、承認、公開、拡張機能の管理、および環境の管理の権限が必要です。 ユーザーインターフェイスオプションが使用できず、これらの手順を完了できない場合は、Experience Cloud管理者に連絡してアクセス権を要求してください。 Launchの権限について詳しくは、[ドキュメント](https://docs.adobelaunch.com/administration/user-permissions)を参照してください。


* **ブラウザープラグイン**
   * Adobe Experience Cloud Debugger([Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj))
   * LaunchとDTMスイッチ([Chrome](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk))

## 関係するユーザー

この統合では、次のオーディエンスが関与し、タスクを実行するために、管理アクセス権が必要になる場合があります。

* 開発者
* AEM Admin
* Experience Cloud管理者

## はじめに

AEM は、Experience Platform Launch との標準の統合を提供します。この統合により、AEM管理者は使いやすいインターフェイスで簡単にExperience Platform Launchを設定できるので、これらの2つのツールを設定する際の労力とエラー数を削減できます。 また、Adobe Target拡張機能をExperience Platform Launchに追加するだけで、AEM Webページ上でAdobe Targetのすべての機能を使用できます。

この節では、次の統合手順について説明します。

*  の起動
   * Launch プロパティの作成
   * Target拡張機能の追加
   * データ要素の作成
   * ページルールの作成
   * 環境の設定
   * ビルドとパブリッシュ
* AEM
   * Cloud Service
   * 作成

###  の起動

#### Launch プロパティの作成

プロパティは、サイトにタグを導入する際に拡張機能、ルール、データ要素およびライブラリを入力するコンテナです。

1. 組織[Adobe Experience Cloud](https://experiencecloud.adobe.com/) (<https://>`<yourcompany>`.experiencecloud.adobe.com)に移動します。
2. Adobe IDを使用してログインし、正しい組織に属していることを確認します。
3. ソリューション切り替えボタンで、「**Launch**」をクリックし、「**Launchに移動**」ボタンを選択します。

   ![Experience Cloud- Launch](assets/using-launch-adobe-io/exc-cloud-launch.png)

4. 適切な組織に属していることを確認し、Launchプロパティの作成に進みます。
   ![Experience Cloud- Launch](assets/using-launch-adobe-io/launch-create-property.png)

   *プロパティの作成について詳しくは、製品ドキュメ [ントの「プ](https://docs.adobelaunch.com/administration/companies-and-properties#create-a-property) ロパティの作成」を参照してください。*
5. 「**新しいプロパティ**」ボタンをクリックします。
6. プロパティの名前を指定します(例：*AEM Target Tutorial*)。
7. ドメインとして、*localhost.com*&#x200B;と入力します。これは、WKNDデモサイトが実行されているドメインです。 「*Domain*」フィールドは必須ですが、Launchプロパティは、実装されているすべてのドメインで機能します。 プライマリの目的は、ルールビルダーでメニューオプションを事前に設定することです。
8. 「**保存**」ボタンをクリックします。

   ![Launch — 新しいプロパティ](assets/using-launch-adobe-io/exc-launch-property.png)

9. 作成したプロパティを開き、「拡張機能」タブをクリックします。

#### Target拡張機能の追加

Adobe Target拡張機能は、最新のWeb用Target JavaScript SDK `at.js`を使用したクライアント側実装をサポートします。 Targetの古いライブラリ`mbox.js`、[をまだ使用しているお客様がLaunchを使用するには、at.js](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/upgrading-from-atjs-1x-to-atjs-20.html)にアップグレードする必要があります。

Target拡張機能は、次の2つの主要部分で構成されます。

* コアライブラリ設定を管理する拡張機能の設定
* 次の操作を実行するルールアクション：
   * Target(at.js)の読み込み
   * すべてのmboxにパラメーターを追加
   * Add Params to Global Mbox
   * Fire Global Mbox

1. **拡張機能**&#x200B;の下に、Launchプロパティ用に既にインストールされている拡張機能のリストが表示されます。 ([Experience Platform Launchコア拡張機能](https://exchange.adobe.com/experiencecloud.details.100223.adobe-launch-core-extension.html)はデフォルトでインストールされます)
2. 「**拡張機能カタログ**」オプションをクリックし、フィルターでTargetを検索します。
3. Adobe Target at.jsの最新バージョンを選択し、「****をインストール」オプションをクリックします。
   ![Launch — 新しいプロパティ](assets/using-launch-adobe-io/launch-target-extension.png)

4. 「****を設定」ボタンをクリックすると、読み込まれたTargetアカウントの資格情報と、この拡張機能のat.jsバージョンが設定ウィンドウに表示されます。
   ![Target — 拡張機能の設定](assets/using-launch-adobe-io/launch-target-extension-2.png)

   Targetが非同期のLaunch埋め込みコードを使用してデプロイされる場合、コンテンツのちらつきを管理するために、Launch埋め込みコードの前に、ページ上に事前に非表示になるスニペットをハードコードする必要があります。 後で、事前非表示スニペットの詳細を説明します。 事前に非表示になっているスニペット[は、こちら](assets/using-launch-adobe-io/prehiding.js)からダウンロードできます。

5. 「**保存**」をクリックして、LaunchプロパティへのTarget拡張機能の追加を完了すると、「**インストール済み**&#x200B;拡張機能」リストにTarget拡張機能が表示されます。

6. 上記の手順を繰り返して、「Experience CloudIDサービス」拡張機能を検索し、インストールします。
   ![拡張機能 —Experience CloudIDサービス](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### 環境の設定

1. サイトプロパティの「**環境**」タブをクリックすると、サイトプロパティ用に作成された環境のリストが表示されます。 デフォルトでは、開発、ステージング、実稼動用にそれぞれ1つのインスタンスが作成されます。

![データ要素 — ページ名](assets/using-launch-adobe-io/launch-environment-setup.png)

#### ビルドとパブリッシュ

1. サイトプロパティの「**公開**」タブをクリックし、ライブラリを作成して変更（データ要素、ルール）を開発環境にデプロイします。
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. 変更を開発環境からステージング環境にパブリッシュします。
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3. **ステージング用にビルド**&#x200B;を実行します。
4. ビルドが完了したら、**パブリッシュの承認**を実行します。これにより、変更内容がステージング環境から実稼動環境に移動します。
   ![実稼動環境へのステージング](assets/using-launch-adobe-io/build-staging.png)
5. 最後に、「**ビルドして実稼動環境に公開**」オプションを実行して、変更を実稼動環境にプッシュします。
   ![ビルドして実稼動環境に公開](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> 中央のチームが少数のAdobe I/O](https://docs.adobe.com/content/help/en/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html)でのみAPI主導の変更をおこなえるように、適切な[役割を持つ選択したワークスペースに対するワークスペース統合のアクセス権を付与します。

1. AEMで、Adobe I/Oの資格情報を使用してIMS統合を作成します(01:12 ～ 03:55)。
2. Experience Platform Launchで、プロパティを作成します。 （[上](#create-launch-property)をカバー）
3. 手順1のIMS統合を使用して、Experience Platform Launch統合を作成し、Launchプロパティを読み込みます。
4. AEMで、ブラウザー設定を使用してExperience Platform Launch統合をサイトにマッピングします。 (05:28～06:14)
5. 手動で統合を検証する。 (06:15～06:33)
6. Launch/DTMブラウザープラグインの使用。 (06:34～06:50)
7. Adobe Experience Cloud Debuggerブラウザープラグインの使用 (06:51～07:22)

この時点で、Adobe Experience Platform Launch](./using-aem-cloud-services.md#integrating-aem-target-options)を使用して、オプション1で説明したように、Adobe Targetと[AEMを正常に統合できました。

AEMエクスペリエンスフラグメントオファーを使用してパーソナライゼーションアクティビティを強化するには、次の章に進み、従来のクラウドサービスを使用してAEMとAdobe Targetを統合します。
