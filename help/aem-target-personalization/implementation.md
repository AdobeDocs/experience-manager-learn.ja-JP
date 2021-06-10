---
title: Adobe Experience ManagerとAdobe Targetの統合
seo-title: パーソナライズされたコンテンツを配信するためにAdobe Experience Manager(AEM)をAdobe Targetと統合する様々な方法を取り上げる記事です。
description: 様々なシナリオでAdobe Experience ManagerとAdobe Targetを設定する方法を説明する記事です。
seo-description: 様々なシナリオでAdobe Experience ManagerとAdobe Targetを設定する方法を説明する記事です。
feature: エクスペリエンスフラグメント
topic: パーソナライズ機能
role: Developer
level: Intermediate
source-git-commit: e82cc5e5de6db33e82b7c71c73bb606f16b98ea6
workflow-type: tm+mt
source-wordcount: '700'
ht-degree: 5%

---


# Adobe Experience ManagerとAdobe Targetの統合

この節では、様々なシナリオでAdobe Experience ManagerとAdobe Targetを設定する方法について説明します。 シナリオと組織の要件に基づきます。

* **Adobe Target JavaScriptライブラリの追加（すべてのシナリオで必要）**
AEMでホストされるサイトの場合、Launchを使用してTargetライブラリをサイトに追 [加できます。](https://experienceleague.adobe.com/docs/launch/using/home.html)Launchは、関連する顧客体験の実現に必要なすべてのタグをデプロイおよび管理するためのシンプルな手段を提供します。
* **Adobe TargetCloud Servicesの追加（エクスペリエンスフラグメントのシナリオで必要）**
 AEMのお客様は、エクスペリエンスフラグメントオファーを使用してAdobe Target内でアクティビティを作成する場合、従来のCloud Servicesを使用してAdobe TargetをAEMと統合する必要があります。この統合は、エクスペリエンスフラグメントをHTML/JSONオファーとしてAEMからTargetにプッシュし、オファーをAEMと同期させるために必要です。 
*この統合は、シナリオ1の実装に必要です。*

## 前提条件

* **Adobe Experience Manager (AEM){#aem}**
   * AEM 6.5（*最新のサービスパックを推奨*）
   * AEM WKNDリファレンスサイトパッケージのダウンロード
      * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
      * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
      * [コアコンポーネント](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
      * [デジタルデータレイヤー](assets/implementation/digital-data-layer.zip)

* **Experience Cloud**
   * 組織へのアクセスAdobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
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
> お客様は、[Adobeサポート](https://helpx.adobe.com/jp/contact/enterprise-support.ec.html)からExperience Platform LaunchとAdobe I/Oをプロビジョニングするか、システム管理者に問い合わせる必要があります

### AEM{#set-up-aem}の設定

このチュートリアルを完了するには、AEMオーサーインスタンスとパブリッシュインスタンスが必要です。 `http://localhost:4502`上で実行されているオーサーインスタンスと、`http://localhost:4503`上で実行されているパブリッシュインスタンスがあります。 詳しくは、以下を参照してください。[ローカルAEM開発環境](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/local-aem-dev-environment-article-setup.html)を設定します。

#### AEMオーサーインスタンスとパブリッシュインスタンスのセットアップ

1. [AEM Quickstart Jarとライセンスのコピーを取得します。](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)
2. 次のようなフォルダー構造をコンピューター上に作成します。
   ![フォルダー構造](assets/implementation/aem-setup-1.png)
3. クイックスタートjarの名前を`aem-author-p4502.jar`に変更し、`/author`ディレクトリの下に配置します。 `license.properties`ファイルを`/author`ディレクトリの下に追加します。
   ![AEMオーサーインスタンス](assets/implementation/aem-setup-author.png)
4. クイックスタートjarのコピーを作成し、名前を`aem-publish-p4503.jar`に変更して、`/publish`ディレクトリの下に配置します。 `license.properties`ファイルのコピーを`/publish`ディレクトリの下に追加します。
   ![AEMパブリッシュインスタンス](assets/implementation/aem-setup-publish.png)
5. `aem-author-p4502.jar`ファイルをダブルクリックして、オーサーインスタンスをインストールします。 これにより、ローカルコンピューターのポート4502で実行されているオーサーインスタンスが起動します。
6. 以下の資格情報を使用してログインします。ログインが成功すると、AEM Home Page Screenに移動します。
ユーザー名：**admin**
パスワード：**admin**
   ![AEMパブリッシュインスタンス](assets/implementation/aem-author-home-page.png)
7. `aem-publish-p4503.jar`ファイルをダブルクリックして、パブリッシュインスタンスをインストールします。 パブリッシュインスタンスのブラウザーで、ポート4503で実行され、WeRetailホームページが表示される新しいタブが開きます。 このチュートリアルでは、WKNDリファレンスサイトを使用して、オーサーインスタンスにパッケージをインストールします。
8. Webブラウザー(`http://localhost:4502`)でAEMオーサーに移動します。 AEMの開始画面で、*[ツール/デプロイメント/パッケージ](http://localhost:4502/crx/packmgr/index.jsp)*&#x200B;に移動します。
9. AEM用のパッケージをダウンロードしてアップロードします(上記の&#x200B;*[前提条件/ AEM](#aem)*&#x200B;に記載されています)。
   * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
   * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
   * [core.wcm.components.all-2.5.0.zip](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
   * [digital-data-layer.zip](assets/implementation/digital-data-layer.zip)

   >[!VIDEO](https://video.tv.adobe.com/v/28377?quality=12&learn=on)
10. AEMオーサーにパッケージをインストールした後、AEM Package Managerでアップロードした各パッケージを選択し、**詳細/レプリケート**&#x200B;を選択して、パッケージがAEMパブリッシュにデプロイされるようにします。
11. この時点で、WKNDリファレンスサイトと、このチュートリアルに必要なすべての追加パッケージが正常にインストールされました。

[次の章](./using-launch-adobe-io.md):次の章では、LaunchをAEMと統合します。
