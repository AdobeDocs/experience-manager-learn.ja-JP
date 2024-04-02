---
title: タグとAdobe Developerを使用したAdobe Experience ManagerとAdobe Targetの統合
description: タグとAdobe Developerを使用してAdobe Experience ManagerとAdobe Targetを統合する方法に関する詳しい手順
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: b1d7ce04-0127-4539-a5e1-802d7b9427dd
duration: 747
source-git-commit: adf3fe30474bcfe5fc1a1e2a8a3d49060067726d
workflow-type: tm+mt
source-wordcount: '985'
ht-degree: 64%

---

# Adobe Developer Console を使用したタグの使用

## 前提条件

* [AEM オーサーとパブリッシュインスタンス](./implementation.md#set-up-aem)をそれぞれローカルホストのポート 4502 と 4503 で実行中である
* **Experience Cloud**
   * 組織の Adobe Experience Cloud へのアクセス - `https://<yourcompany>.experiencecloud.adobe.com`
   * 次のソリューションをプロビジョニングされた Experience Cloud
      * [データ収集](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe 開発者コンソール](https://developer.adobe.com/console/)

     >[!NOTE]
     >データ収集では、開発、承認、公開、拡張機能の管理、および環境の管理の権限が必要です。 ユーザーインターフェイスオプションが使用できず、これらの手順を完了できない場合は、Experience Cloud 管理者に問い合わせてアクセス権を要求してください。タグ権限の詳細は、 [ドキュメントを参照](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/user-permissions.html?lang=ja).

* **Chrome ブラウザー拡張機能**
   * Adobe Experience Cloud Debugger(https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)

## 関連するユーザー

この統合には、次のようなオーディエンスの関与が必要となります。また、一部のタスクでは管理者権限が必要となることがあります。

* デベロッパー
* AEM Admin
* Experience Cloud 管理者

## はじめに

AEMは、標準でタグとの統合を提供します。 この統合により、AEM管理者は使いやすいインターフェイスでタグを簡単に設定できるので、これら 2 つのツールを設定する際の労力とエラー数を削減できます。 また、タグにAdobe Target拡張機能を追加するだけで、AEM Web ページ上でAdobe Targetのすべての機能を使用できます。

この節では、次の統合手順を説明します。

* タグ
   * tags プロパティを作成する
   * Target 拡張機能の追加
   * データ要素の作成
   * ページルールの作成
   * 環境の設定
   * ビルドとパブリッシュ
* AEM
   * Cloud Service の作成
   * 作成

### タグ

#### タグプロパティを作成する

プロパティは、サイトにタグを導入する際に拡張機能、ルール、データ要素およびライブラリを入力するコンテナです。

1. 組織の [Adobe Experience Cloud](https://experiencecloud.adobe.com/) に移動（`https://<yourcompany>.experiencecloud.adobe.com`）
1. Adobe ID を使用してログインし、適切な組織に属していることを確認します。
1. ソリューション切り替えボタンで、 **Experience Platform**&#x200B;を、 **データ収集** セクションを選択し、 **タグ**.

![Experience Cloud — タグ](assets/using-launch-adobe-io/exc-cloud-launch.png)

1. 適切な組織に属していることを確認し、タグプロパティの作成に進みます。
   ![Experience Cloud — タグ](assets/using-launch-adobe-io/launch-create-property.png)

   *プロパティの作成について詳しくは、製品ドキュメント内の[プロパティの作成](https://experienceleague.adobe.com/docs/experience-platform/tags/admin/companies-and-properties.html?lang=ja#create-or-configure-a-property)を参照してください。*
1. 「**新しいプロパティ**」ボタンをクリック
1. プロパティの名前を指定（例：*AEM Target チュートリアル*）
1. これは、WKND デモサイトが実行されているドメインなので、ドメインに *localhost.com* と入力します。ただし、*ドメイン*「 」フィールドは必須です。tags プロパティは、実装されているすべてのドメインで機能します。 主な目的は、ルールビルダーでメニューオプションを事前に設定することです。
1. 「**保存**」ボタンをクリックします。

   ![tags — 新しいプロパティ](assets/using-launch-adobe-io/exc-launch-property.png)

1. 作成したプロパティを開き、「拡張機能」タブをクリックします。

#### Target 拡張機能の追加

Adobe Target 拡張機能は、最新の web 用の Target JavaScript SDK、`at.js` を使用したクライアントサイドの実装をサポートしています。Target の古いライブラリをまだ使用しているお客様 `mbox.js`, [at.js にアップグレードする必要がある](https://experienceleague.adobe.com/docs/target-dev/developer/client-side/at-js-implementation/upgrading-from-atjs-1x-to-atjs-20.html?lang=ja) タグを使用します。

Target 拡張機能は、次の 2 つの主要な部分で構成されています。

* コアライブラリ設定を管理する拡張機能の設定
* 次の操作を実行するルールのアクションです。
   * Target（at.js）を読み込み
   * すべての mbox にパラメーターを追加
   * グローバル mbox にパラメーターを追加
   * Fire Global mbox

1. の下 **拡張機能**&#x200B;の場合は、タグプロパティ用に既にインストールされている拡張機能のリストが表示されます。 ([AdobeLaunch Core 拡張機能](https://exchange.adobe.com/apps/ec/100223/adobe-launch-core-extension) はデフォルトでインストールされます )
2. 「**拡張機能カタログ**」オプションを選択し、フィルターで Target を検索します。
3. 最新バージョンの Adobe Target at.js を選択し、「**インストール**」オプションをクリックします。
   ![タグ — 新しいプロパティ](assets/using-launch-adobe-io/launch-target-extension.png)

4. 「**設定**」ボタンをクリックすると、Target アカウントの資格情報が読み込まれた設定ウィンドウと、この拡張機能の at.js のバージョンが表示されます。
   ![Target - 拡張機能の設定](assets/using-launch-adobe-io/launch-target-extension-2.png)

   非同期タグ埋め込みコードを使用して Target をデプロイする場合、コンテンツのちらつきを管理するには、タグ埋め込みコードの前に、ページ上で事前に非表示になるスニペットをハードコードする必要があります。 事前に非表示になるスニペットの詳細については、後で説明します。事前に非表示になるスニペットは[こちら](assets/using-launch-adobe-io/prehiding.js)からダウンロードできます。

5. クリック **保存** をクリックして、tags プロパティに Target 拡張機能を追加すると、Target 拡張機能が **インストール済み** 拡張機能リスト。

6. 上記の手順を繰り返して、「Experience Cloud ID サービス」拡張機能を検索し、インストールします。
   ![拡張機能 - Experience Cloud ID サービス](assets/using-launch-adobe-io/launch-extension-experience-cloud.png)

#### 環境の設定

1. 「**環境**」タブをクリックすると、サイトプロパティ用に作成された環境のリストを確認できます。デフォルトでは、開発、ステージ、実稼動用にそれぞれ 1 つのインスタンスが作成されます。

![データ要素 - ページ名](assets/using-launch-adobe-io/launch-environment-setup.png)

#### ビルドとパブリッシュ

1. 「**公開**」タブをクリックし、サイトプロパティ用にライブラリを作成して、変更（データ要素、ルール）を開発環境にビルドし、デプロイします。
   >[!VIDEO](https://video.tv.adobe.com/v/28412?quality=12&learn=on)
2. 変更を開発環境からステージング環境にパブリッシュします。
   >[!VIDEO](https://video.tv.adobe.com/v/28419?quality=12&learn=on)
3.  **ステージング用にビルドオプション**&#x200B;を実行します。
4. ビルドが完了したら、**公開の承認**を実行し、変更をステージング環境から実稼動環境に移動させます。
   ![ステージングから実稼動環境へ](assets/using-launch-adobe-io/build-staging.png)
5. 最後に、「**ビルドして実稼動環境に公開**」オプションを使用して、変更を実稼動環境にプッシュします。
   ![ビルドして実稼動環境にパブリッシュ](assets/using-launch-adobe-io/build-and-publish.png)

### Adobe Experience Manager

>[!VIDEO](https://video.tv.adobe.com/v/28416?quality=12&learn=on)

>[!NOTE]
>
> Adobe Developer 統合に、[中央のチームが少数のワークスペースのみで API 駆動型の変更を行えるようにする役割](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/configure-adobe-io-integration.html?lang=ja)が適切であるワークスペースを選択するアクセス権を付与します。

1. Adobe Developer の資格情報を使用して、AEM で IMS 統合を作成します。（01:12～03:55）
2. データ収集で、プロパティを作成します。 （[上記](#create-launch-property)が対象）
3. 手順 1 の IMS 統合を使用して、タグ統合を作成し、タグプロパティを読み込みます。
4. AEMで、ブラウザー設定を使用してタグ統合をサイトにマッピングします。 （05:28～06:14）
5. 統合を手動で検証します。（06:15～06:33）
6. Adobe Experience Cloud Debugger ブラウザープラグインを使用します。（06:51～07:22）

この時点で、正常に統合されました [タグを使用したAdobe TargetとのAEM](./using-aem-cloud-services.md#integrating-aem-target-options) オプション 1 で説明されているように。

AEM Experience Fragment オファーを使用してパーソナライゼーションアクティビティを強化するには、次の章に進み、従来のクラウドサービスを使用して AEM と Adobe Target を統合します。
