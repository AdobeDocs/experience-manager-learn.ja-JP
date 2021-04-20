---
title: Experience Platform LaunchとAdobe I/Oを使ってAdobe Experience ManagerとAdobe Targetを統合する
seo-title: Experience Platform LaunchとAdobe I/Oを使ってAdobe Experience ManagerとAdobe Targetを統合する
description: Experience Platform LaunchとAdobe I/Oを使ってAdobe Experience ManagerとAdobe Targetを統合する方法を順を追って説明します。
seo-description: Experience Platform LaunchとAdobe I/Oを使ってAdobe Experience ManagerとAdobe Targetを統合する方法を順を追って説明します。
feature: エクスペリエンスフラグメント
topic: パーソナライズ機能
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1101'
ht-degree: 4%

---


# Adobe I/Oコンソールを使用したAdobe Experience Platform Launchの使用

## 前提条件

* [AEM authorおよびpublish](./implementation.md#set-up-aem) インスタンスは、それぞれlocalhostポート4502および4503で実行
* **Experience Cloud**
   * 組織へのアクセス：Adobe Experience Cloud- <https://>`<yourcompany>`.experiencecloud.adobe.com
   * 次のソリューションでプロビジョニングされたExperience Cloud
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe I/Oコンソール](https://console.adobe.io)

      >[!NOTE]
      >「起動」で、環境の開発、承認、投稿、拡張機能の管理および管理を行う権限が必要です。 ユーザインターフェイスオプションが使用できないために、これらの手順を完了できない場合は、Experience Cloud管理者に問い合わせて、アクセスを要求してください。 起動権限について詳しくは、[ドキュメント](https://docs.adobelaunch.com/administration/user-permissions)を参照してください。


* **ブラウザープラグイン**
   * Adobe Experience Cloudデバッガー([Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj))
   * 起動とDTMスイッチ([Chrome](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk))

## 関係するユーザー

この統合では、次のオーディエンスが関与し、一部のタスクを実行するには、管理者アクセスが必要になる場合があります。

* 開発者
* AEM管理者
* Experience Cloud管理者

## はじめに

AEM は、Experience Platform Launch との標準の統合を提供します。この統合により、AEM管理者は使いやすいインターフェイスを使用して簡単にExperience Platform Launchを設定できるので、これら2つのツールを設定する際の作業量とエラー数を削減できます。 また、Experience Platform LaunchにAdobe Target拡張機能を追加するだけで、AEM WebページのAdobe Targetの全機能を利用できます。

この節では、次の統合手順を説明します。

*  の起動
   * Launch プロパティの作成
   * ターゲット式の追加
   * データ要素の作成
   * ページルールの作成
   * 設定環境
   * ビルドとパブリッシュ
* AEM
   * Cloud Serviceの作成
   * 作成

###  の起動

#### Launch プロパティの作成

プロパティとは、サイトにタグを導入する際に、拡張子、ルール、データ要素、ライブラリを使用して入力するコンテナです。

1. 組織[Adobe Experience Cloud](https://experiencecloud.adobe.com/) (<https://>`<yourcompany>`.experiencecloud.adobe.com)に移動します。
2. Adobe IDを使用してログインし、正しい組織に属していることを確認します。
3. ソリューションの切り替えボタンで、**「**&#x200B;を起動」をクリックし、**「**&#x200B;起動に移動」ボタンを選択します。

   ![Experience Cloud — 起動](assets/using-launch-adobe-io/exc-cloud-launch.png)

4. 正しい組織に属していることを確認し、起動プロパティの作成に進みます。
   ![Experience Cloud — 起動](assets/using-launch-adobe-io/launch-create-property.png)

   *プロパティの作成について詳しくは、製品ドキュメントの「 [プロパティの](https://docs.adobelaunch.com/administration/companies-and-properties#create-a-property) 作成」を参照してください。*
5. 「**新しいプロパティ**」ボタンをクリックします
6. プロパティの名前を指定します(例：*AEMターゲットチュートリアル*)
7. ドメインとして、*localhost.com*&#x200B;と入力します。これは、WKNDデモサイトが実行されているドメインです。 &#39;*Domain*&#39;フィールドは必須ですが、Launchプロパティは、このフィールドが実装されている任意のドメインで機能します。 このフィールドのプライマリ目的は、ルールビルダーのメニューオプションを事前に設定することです。
8. 「**保存**」ボタンをクリックします。

   ![起動 — 新しいプロパティ](assets/using-launch-adobe-io/exc-launch-property.png)

9. 作成したプロパティを開き、「拡張子」タブをクリックします。

#### ターゲット式の追加

Adobe Targetの拡張機能は、最新のWeb用ターゲットJavaScript SDKを使用したクライアント側実装をサポートしています(`at.js`)。 まだターゲット古いライブラリ`mbox.js`、[を使用しているお客様は、Launchを使用するためにat.js](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/upgrading-from-atjs-1x-to-atjs-20.html)にアップグレードする必要があります。

ターゲット式は、次の2つの主要な部分で構成されます。

* コアライブラリ設定を管理する拡張機能の設定
* ルールのアクションを使用して、次の操作を行います。
   * ターゲットの読み込み(at.js)
   * す追加べてのmboxに対するパラメーター
   * グロ追加ーバルmboxへのパラメーター
   * グローバルmboxを起動

1. **拡張機能**&#x200B;の下に、起動プロパティ用に既にインストールされている拡張機能のリストが表示されます。 ([Experience Platform Launchコア拡張機能](https://exchange.adobe.com/experiencecloud.details.100223.adobe-launch-core-extension.html)はデフォルトでインストールされます)
2. 「**拡張機能カタログ**」オプションをクリックし、フィルターでターゲットを検索します。
3. 最新バージョンのAdobe Targetat.jsを選択し、**「**をインストール」オプションをクリックします。
   ![起動 — 新しいプロパティ](assets/using-launch-adobe-io/launch-target-extension.png)

4. 「**設定**」ボタンをクリックすると、ターゲットアカウントの資格情報が読み込まれた設定ウィンドウと、この拡張機能のat.jsバージョンが表示されます。
   ![ターゲット — 拡張機能の設定](assets/using-launch-adobe-io/launch-target-extension-2.png)

   非同期起動の埋め込みコードを使用してターゲットをデプロイする場合、コンテンツのちらつきを管理するには、開始の埋め込みコードの前に、ページの非表示のスニペットをハードコードする必要があります。 後で、前の隠し狙撃手の詳細を確認します。 事前非表示のスニペット[ここ](assets/using-launch-adobe-io/prehiding.js)をダウンロードできます。

5. 「**保存**」をクリックしてターゲット式をLaunchプロパティに追加すると、**インストール済み**&#x200B;拡張機能リストの下に一覧表示されたターゲット式を確認できるようになります。

6. 上記の手順を繰り返して「Experience CloudIDサービス」拡張を検索し、インストールします。
   ![拡張機能 —Experience CloudIDサービス](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### 設定環境

1. サイトプロパティの[**環境**]タブをクリックすると、サイトプロパティ用に作成される環境のリストを確認できます。 デフォルトでは、開発用、ステージング用、実稼動用にそれぞれ1つのインスタンスが作成されます。

![データ要素 — ページ名](assets/using-launch-adobe-io/launch-environment-setup.png)

#### ビルドとパブリッシュ

1. サイトプロパティの&#x200B;**「発行**」タブをクリックして、変更（データ要素、ルール）を作成し、開発環境に展開するためのライブラリを作成します。
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. 開発環境からステージング環境に変更を発行します。
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3. **ステージング用のビルドオプション**&#x200B;を実行します。
4. ビルドが完了したら、**発行の承認**を実行します。これにより、変更がステージング環境から実稼動環境に移行します。
   ![実稼動へのステージング](assets/using-launch-adobe-io/build-staging.png)
5. 最後に、「**ビルドして実稼動環境に発行**」オプションを実行し、変更内容を実稼動環境にプッシュします。
   ![ビルドして実稼働環境に発行](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> Adobe I/O統合に適切な[ロールを持つ選択ワークスペースへのアクセスを許可し、中央チームが少数のワークスペース](https://docs.adobe.com/content/help/en/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html)でのみAPIに基づく変更を行えるようにします。

1. Adobe I/Oの資格情報を使用して、AEMでIMS統合を作成します。(01:12 ～ 03:55)
2. Experience Platform Launchで、プロパティを作成します。 （[上](#create-launch-property)をカバー）
3. 手順1のIMS統合を使用して、Experience Platform Launch統合を作成し、Launchプロパティをインポートします。
4. AEMで、ブラウザーの設定を使用してExperience Platform Launch統合をサイトにマッピングします。 (05:28 ～ 06:14)
5. 統合を手動で検証する。 (06:15 ～ 06:33)
6. Launch/DTMブラウザープラグインを使用しています。 (06:34 ～ 06:50)
7. Adobe Experience Cloudデバッガーブラウザープラグインの使用 (06:51 ～ 07:22)

この時点で、オプション1で詳しく説明するように、Adobe Experience Platform Launch](./using-aem-cloud-services.md#integrating-aem-target-options)を使用して[AEMをAdobe Targetに統合することに成功しました。

AEMエクスペリエンスフラグメントオファーを使用してアクティビティをパーソナライズする場合は、次の章に進み、レガシークラウドサービスを使用してAEMをAdobe Targetと統合します。
