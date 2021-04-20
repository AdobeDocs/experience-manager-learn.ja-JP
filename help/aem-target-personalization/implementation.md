---
title: Adobe Experience ManagerとAdobe Targetの統合
seo-title: パーソナライズされたコンテンツを配信するための、様々な方法でAdobe Experience Manager(AEM)とAdobe Targetを統合する記事。
description: 様々なシナリオでのAdobe TargetとのAdobe Experience Managerの設定方法を取り上げた記事。
seo-description: 様々なシナリオでのAdobe TargetとのAdobe Experience Managerの設定方法を取り上げた記事。
feature: エクスペリエンスフラグメント
topic: パーソナライズ機能
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '702'
ht-degree: 6%

---


# Adobe Experience ManagerとAdobe Targetの統合

この節では、Adobe TargetとのAdobe Experience Managerの設定方法を、様々なシナリオで説明します。 シナリオと組織の要件に基づきます。

* **Adobe Target追加 JavaScriptライブラリ（すべてのシナリオで必要）AEMでホストされるサイト**
の場合、「 [起動](https://docs.adobe.com/content/help/ja-JP/launch/using/overview.html)」を使用してターゲットライブラリをサイトに追加できます。Launchは、関連する顧客エクスペリエンスに電力を供給するために必要なすべてのタグを簡単に導入および管理する方法を提供します。
* **Adobe Target追加のCloud Services（エクスペリエンスフラグメントのシナリオで必要）**
AEMのお客様は、エクスペリエンスフラグメントオファーを使用してAdobe Target内でアクティビティを作成する場合、Cloud Servicesをレガシーを使用しAEMて統合する必要があります。この統合は、エクスペリエンスフラグメントをAEMからHTML/JSONオファーとしてターゲットにプッシュし、オファーとAEMの同期を維持するために必要です。 
*この統合は、シナリオ1の実装に必要です。*

## 前提条件

* **Adobe Experience Manager (AEM){#aem}**
   * AEM 6.5 （*最新のService Packを推奨*）
   * AEM WKNDリファレンスサイトパッケージのダウンロード
      * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
      * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
      * [コアコンポーネント](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
      * [デジタルデータレイヤー](assets/implementation/digital-data-layer.zip)

* **Experience Cloud**
   * 組織へのアクセス：Adobe Experience Cloud- <https://>`<yourcompany>`.experiencecloud.adobe.com
   * 次のソリューションでプロビジョニングされたExperience Cloud
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe I/Oコンソール](https://console.adobe.io)

* **環境**
   * Java 1.8またはJava 11(AEM 6.5以降のみ)
   * Apache Maven（3.3.9 以降）
   * Chrome

>[!NOTE]
>
> お客様は、[Adobeサポート](https://helpx.adobe.com/jp/contact/enterprise-support.ec.html)からExperience Platform LaunchとAdobe I/Oを利用してプロビジョニングするか、システム管理者に連絡する必要があります。

### AEM{#set-up-aem}のセットアップ

このチュートリアルを完了するには、AEMオーサーインスタンスとパブリッシュインスタンスが必要です。 `http://localhost:4502`で実行されている作成者インスタンスと、`http://localhost:4503`で実行されている発行インスタンスがあります。 詳しくは、次を参照してください。[ローカルAEM開発環境](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/local-aem-dev-environment-article-setup.html)を設定します。

#### AEM作成者インスタンスと発行インスタンスの設定

1. [AEM Quickstart Jarのコピーとライセンスを取得します。](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)
2. 次のようなフォルダー構造をコンピューター上に作成します。
   ![フォルダー構造](assets/implementation/aem-setup-1.png)
3. Quickstart jarの名前を`aem-author-p4502.jar`に変更し、`/author`ディレクトリの下に配置します。 追加`/author`ディレクトリの下の`license.properties`ファイル。
   ![AEM作成者インスタンス](assets/implementation/aem-setup-author.png)
4. Quickstart jarのコピーを作成し、`aem-publish-p4503.jar`に名前を変更して`/publish`ディレクトリの下に配置します。 追加`/publish`ディレクトリの下の`license.properties`ファイルのコピー。
   ![AEM発行インスタンス](assets/implementation/aem-setup-publish.png)
5. 重複が`aem-author-p4502.jar`ファイルをクリックして、作成者インスタンスをインストールします。 これにより、ローカルコンピューターのポート4502で実行されている作成者インスタンスが開始されます。
6. 以下の資格情報を使用してサインインします。ログインに成功すると、AEMホームページ画面に誘導されます。
username :**admin**
パスワード：**管理者**
   ![AEM発行インスタンス](assets/implementation/aem-author-home-page.png)
7. 重複が`aem-publish-p4503.jar`ファイルをクリックして、発行インスタンスをインストールします。 発行インスタンスの新しいタブがブラウザーで開き、ポート4503で実行され、WebRetailホームページが表示されています。 このチュートリアルでは、WKNDリファレンスサイトを使用して、パッケージを作成者インスタンスにインストールします。
8. Webブラウザー(`http://localhost:4502`)で「AEM作成者」に移動します。 AEM開始画面で、*[ツール/展開/パッケージ](http://localhost:4502/crx/packmgr/index.jsp)*&#x200B;に移動します。
9. AEM用のパッケージをダウンロードしてアップロードします(上記の&#x200B;*[前提条件/AEM](#aem)*&#x200B;に記載)。
   * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
   * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
   * [core.wcm.components.all-2.5.0.zip](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
   * [digital-data-layer.zip](assets/implementation/digital-data-layer.zip)

   >[!VIDEO](https://video.tv.adobe.com/v/28377?quality=12&learn=on)
10. パッケージをAEM Authorにインストールした後、AEM Package Managerでアップロードした各パッケージを選択し、**詳細/**&#x200B;を複製を選択して、パッケージがAEM Publishに展開されることを確認します。
11. この時点で、WKNDリファレンスサイトと、このチュートリアルに必要なすべての追加パッケージが正常にインストールされました。

[次の章](./using-launch-adobe-io.md):次の章では、LaunchとAEMを統合します。
