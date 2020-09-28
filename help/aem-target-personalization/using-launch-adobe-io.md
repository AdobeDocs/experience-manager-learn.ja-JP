---
title: Experience Platform LaunchI/OとAdobeI/Oを使用してAdobe Experience ManagerとAdobe Targetを統合
seo-title: Experience Platform LaunchI/OとAdobeI/Oを使用してAdobe Experience ManagerとAdobe Targetを統合
description: Experience Platform LaunchI/OとAdobeI/Oを使用してAdobe Experience ManagerをAdobe Targetと統合する方法を順を追って説明します。
seo-description: Experience Platform LaunchI/OとAdobeI/Oを使用してAdobe Experience ManagerをAdobe Targetと統合する方法を順を追って説明します。
translation-type: tm+mt
source-git-commit: 1209064fd81238d4611369b8e5b517365fc302e3
workflow-type: tm+mt
source-wordcount: '1098'
ht-degree: 2%

---


# AdobeI/Oコンソールを介したAdobe Experience Platform Launchの使用

## 前提条件

* [AEM authorインスタンス](./implementation.md#set-up-aem) 、およびlocalhost 4502ポート4503でそれぞれ実行されているパブリッシュインスタンス
* **Experience Cloud**
   * 組織へのアクセス：Adobe Experience Cloud- <https://>`<yourcompany>`.experiencecloud.adobe.com
   * 次のソリューションでプロビジョニングされたExperience Cloud
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [AdobeI/Oコンソール](https://console.adobe.io)

      >[!NOTE]
      >「起動」で、環境の開発、承認、投稿、拡張機能の管理および管理を行う権限が必要です。 ユーザインターフェイスオプションが使用できないために、これらの手順を完了できない場合は、Experience Cloud管理者に問い合わせて、アクセスを要求してください。 起動権限について詳しくは、ドキュメント [を参照してください](https://docs.adobelaunch.com/administration/user-permissions)。


* **ブラウザープラグイン**
   * Adobe Experience Cloudデバッガー([Chrome](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj))
   * 起動とDTM Switch([Chrome](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk))

## 関係するユーザー

この統合では、次のオーディエンスが関与し、一部のタスクを実行するには、管理者アクセスが必要になる場合があります。

* デベロッパー
* AEM管理者
* Experience Cloud管理者

## はじめに

AEMオファーは、Experience Platform Launchとの初期設定の統合を行います。 この統合により、AEM管理者は使いやすいインターフェイスを使用して簡単にExperience Platform Launchを設定できるので、これら2つのツールを設定する際の作業量とエラー数を削減できます。 また、Experience Platform LaunchにAdobe Target拡張機能を追加するだけで、AEM WebページのAdobe Targetの全機能を利用できます。

この節では、次の統合手順を説明します。

*  の起動
   * 起動プロパティの作成
   * ターゲット式の追加
   * データ要素の作成
   * ページルールの作成
   * 設定環境
   * 構築と公開
* AEM
   * Cloud Serviceの作成
   * 作成

###  の起動

#### 起動プロパティの作成

プロパティとは、サイトにタグを導入する際に、拡張子、ルール、データ要素、ライブラリを使用して入力するコンテナです。

1. 組織 [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (<https://>`<yourcompany>`.experiencecloud.adobe.com)に移動します。
2. Adobe IDを使用してログインし、正しい組織に属していることを確認します。
3. ソリューションの切り替えボタンで、「 **Launch** 」をクリックし、「 **Go To Launch** 」ボタンを選択します。

   ![Experience Cloud — 起動](assets/using-launch-adobe-io/exc-cloud-launch.png)

4. 正しい組織に属していることを確認し、起動プロパティの作成に進みます。
   ![Experience Cloud — 起動](assets/using-launch-adobe-io/launch-create-property.png)

   *プロパティの作成について詳しくは、製品ドキュメントの「プロパティの[作成](https://docs.adobelaunch.com/administration/companies-and-properties#create-a-property)」を参照してください。*
5. 「 **新規プロパティ** 」ボタンをクリックします。
6. プロパティの名前を指定します(例： *AEMターゲットチュートリアル*)
7. ドメインとして、 *localhost.com* と入力します。これは、WKNDデモサイトが実行されているドメインです。 [*Domain*]フィールドは必須ですが、Launchプロパティは、それが実装されている任意のドメインで機能します。 このフィールドのプライマリ目的は、ルールビルダーのメニューオプションを事前に設定することです。
8. Click the **Save** button.

   ![起動 — 新しいプロパティ](assets/using-launch-adobe-io/exc-launch-property.png)

9. 作成したプロパティを開き、「拡張子」タブをクリックします。

#### ターゲット式の追加

Adobe Target拡張機能は、最新のWeb用ターゲットJavaScript SDKを使用したクライアント側実装をサポートし `at.js`ます。 ターゲットの古いライブラリを引き続き使用している場合は、Launchを使用す `mbox.js`るに [はat.js](https://docs.adobe.com/content/help/en/target/using/implement-target/client-side/upgrading-from-atjs-1x-to-atjs-20.html) にアップグレードする必要があります。

ターゲット式は、次の2つの主要な部分で構成されます。

* コアライブラリ設定を管理する拡張機能の設定
* ルールのアクションを使用して、次の操作を行います。
   * ターゲットの読み込み(at.js)
   * す追加べてのmboxに対するパラメーター
   * グロ追加ーバルmboxへのパラメーター
   * グローバルmboxを起動

1. 「 **拡張子**」の下に、「起動」プロパティ用に既にインストールされている拡張子のリストが表示されます。 ([Experience Platform Launchコア拡張機能](https://exchange.adobe.com/experiencecloud.details.100223.adobe-launch-core-extension.html) はデフォルトでインストールされます)
2. 「 **拡張機能カタログ** 」オプションをクリックし、フィルターでターゲットを検索します。
3. Adobe Targetat.jsの最新バージョンを選択し、「 **インストール** 」オプションをクリックします。
   ![起動 — 新しいプロパティ](assets/using-launch-adobe-io/launch-target-extension.png)

4. 「 **設定** 」ボタンをクリックすると、ターゲットアカウントの資格情報が読み込まれた設定ウィンドウ、およびこの拡張機能のat.jsバージョンが表示されます。
   ![ターゲット — 拡張機能の設定](assets/using-launch-adobe-io/launch-target-extension-2.png)

   非同期起動の埋め込みコードを使用してターゲットをデプロイする場合、コンテンツのちらつきを管理するには、開始の埋め込みコードの前に、ページの非表示のスニペットをハードコードする必要があります。 後で、前の隠し狙撃手の詳細を確認します。 事前非表示のスニペットは [こちらからダウンロードできます](assets/using-launch-adobe-io/prehiding.js)

5. 「 **保存** 」をクリックして、ターゲット式を「起動」プロパティに追加するのを完了すると、「インストール済みの **** 拡張機能」リストの下にターゲット式が表示されます。

6. 上記の手順を繰り返して「Experience CloudIDサービス」拡張を検索し、インストールします。
   ![拡張機能 —Experience CloudIDサービス](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### 設定環境

1. サイトプロパティの[ **環境** ]タブをクリックすると、サイトプロパティ用に作成された環境のリストを確認できます。 デフォルトでは、開発用、ステージング用、実稼動用にそれぞれ1つのインスタンスが作成されます。

![データ要素 — ページ名](assets/using-launch-adobe-io/launch-environment-setup.png)

#### 構築と公開

1. サイトプロパティの「 **発行** 」タブをクリックし、ライブラリを作成して、変更（データ要素、ルール）を開発環境に展開します。
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. 開発環境からステージング環境に変更を発行します。
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3. 「 **Build for Staging」オプションを実行します**。
4. ビルドが完了したら、 **Approve for Publishing**（公開の承認）を実行します。これにより、変更内容がステージング環境から実稼動環境に移行します。
   ![実稼動へのステージング](assets/using-launch-adobe-io/build-staging.png)
5. 最後に、「 **ビルドして実稼働環境に発行** 」オプションを実行して変更を実稼動環境にプッシュします。
   ![ビルドして実稼働環境に発行](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> AdobeI/O統合に適切な [役割を持つ選択したワークスペースへのアクセス権を付与し、中央チームが少数のワークスペースでのみAPIに基づく変更を行えるようにします](https://docs.adobe.com/content/help/en/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html)。

1. AdobeI/Oの資格情報を使用して、AEMでIMS統合を作成します(01:12 ～ 03:55)。
2. Experience Platform Launchで、プロパティを作成します。 ( [上記](#create-launch-property))
3. 手順1のIMS統合を使用して、Experience Platform Launch統合を作成し、Launchプロパティをインポートします。
4. AEMで、ブラウザーの設定を使用してExperience Platform Launch統合をサイトにマッピングします。 (05:28 ～ 06:14)
5. 統合を手動で検証する。 (06:15 ～ 06:33)
6. Launch/DTMブラウザープラグインを使用しています。 (06:34 ～ 06:50)
7. Adobe Experience Cloudデバッガーブラウザープラグインの使用 (06:51 ～ 07:22)

この時点で、オプション1で詳しく説明するように、Adobe Experience Platform Launch [を使用してAEMとAdobe Targetの統合を成功させました](./using-aem-cloud-services.md#integrating-aem-target-options) 。

AEMエクスペリエンスフラグメントオファーを使用してアクティビティをパーソナライズする場合は、次の章に進み、レガシークラウドサービスを使用してAEMをAdobe Targetと統合します。
