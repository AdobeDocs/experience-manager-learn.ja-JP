---
title: Adobe Experience ManagerとAdobe Targetの統合
seo-title: An article covering different ways to integrate Adobe Experience Manager(AEM) with Adobe Target for delivering personalized content.
description: 様々なシナリオでAdobe TargetとAdobe Experience Managerを設定する方法を説明する記事です。
seo-description: An article covering how to set up Adobe Experience Manager with Adobe Target for different scenarios.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: 54a30cd9-d94a-4de5-82a1-69ab2263980d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '659'
ht-degree: 5%

---

# Adobe Experience ManagerとAdobe Targetの統合

この節では、様々なシナリオでAdobe Experience ManagerとAdobe Targetを設定する方法について説明します。 シナリオと組織要件に基づきます。

* **Adobe Target JavaScript ライブラリの追加（すべてのシナリオで必要）**
AEMでホストされるサイトの場合、を使用して Target ライブラリをサイトに追加できます。 [起動](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html). Launch は、顧客体験の実現に必要なすべてのタグをデプロイおよび管理するためのシンプルな手段を提供します。
* **Adobe TargetCloud Servicesの追加（エクスペリエンスフラグメントシナリオで必要）**
AEMのお客様がエクスペリエンスフラグメントオファーを使用してAdobe Target内でアクティビティを作成する場合、従来のCloud Servicesを使用してAdobe TargetとAEMを統合する必要があります。 この統合は、エクスペリエンスフラグメントをHTML/JSON オファーとしてAEMから Target にプッシュし、オファーとAEMの同期を維持するために必要です。 
*この統合は、シナリオ 1 の実装に必要です。*

## 前提条件

* **Adobe Experience Manager (AEM){#aem}**
   * AEM 6.5 (*最新のサービスパックをお勧めします*)
   * AEM WKND リファレンスサイトパッケージをダウンロードします。
      * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
      * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
      * [コアコンポーネント](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
      * [Digital Data Layer](assets/implementation/digital-data-layer.zip)

* **Experience Cloud**
   * 組織へのアクセスAdobe Experience Cloud - `https://<yourcompany>.experiencecloud.adobe.com`
   * 次のソリューションでプロビジョニングされたExperience Cloud
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe I/Oコンソール](https://console.adobe.io)

* **環境**
   * Java 1.8 または Java 11(AEM 6.5 以降のみ )
   * Apache Maven（3.3.9 以降）
   * Chrome

>[!NOTE]
>
> 顧客は、からのExperience Platform LaunchとAdobe I/Oをプロビジョニングする必要がある [Adobeサポート](https://helpx.adobe.com/jp/contact/enterprise-support.ec.html) または、システム管理者に問い合わせてください。

### AEMの設定{#set-up-aem}

このチュートリアルを完了するには、AEMオーサーインスタンスとパブリッシュインスタンスが必要です。 オーサーインスタンスがで動作しています `http://localhost:4502` およびパブリッシュインスタンスが実行されている状態 `http://localhost:4503`. 詳しくは、以下を参照してください。 [ローカルAEM開発環境の設定](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/local-aem-dev-environment-article-setup.html).

#### AEM オーサーインスタンスとパブリッシュインスタンスのセットアップ

1. のコピーを取得 [AEM Quickstart Jar とライセンス。](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)
2. 次のようなフォルダー構造をコンピューター上に作成します。
   ![フォルダー構造](assets/implementation/aem-setup-1.png)
3. クイックスタート JAR の名前をに変更します。 `aem-author-p4502.jar` そしてそれをの下に置く `/author` ディレクトリ。 を `license.properties` の下のファイル `/author` ディレクトリ。
   ![AEM オーサーインスタンス](assets/implementation/aem-setup-author.png)
4. クイックスタート JAR のコピーを作成し、名前をに変更します。 `aem-publish-p4503.jar` そしてそれをの下に置く `/publish` ディレクトリ。 のコピーを追加 `license.properties` の下のファイル `/publish` ディレクトリ。
   ![AEM パブリッシュインスタンス](assets/implementation/aem-setup-publish.png)
5. 次をダブルクリックします。 `aem-author-p4502.jar` ファイルを開き、オーサーインスタンスをインストールします。 これにより、ローカルコンピューターのポート 4502 で実行されているオーサーインスタンスが起動します。
6. 以下の資格情報を使用してログインします。ログインに成功すると、AEM Home Page 画面に移動します。
ユーザー名： **admin**
パスワード： **admin**
   ![AEM パブリッシュインスタンス](assets/implementation/aem-author-home-page.png)
7. 次をダブルクリックします。 `aem-publish-p4503.jar` ファイルを使用して、パブリッシュインスタンスをインストールします。 パブリッシュインスタンスのブラウザーで新しいタブが開き、ポート 4503 で実行され、WeRetail ホームページが表示されていることがわかります。 このチュートリアルでは WKND リファレンスサイトを使用しており、オーサーインスタンスにパッケージをインストールします。
8. Web ブラウザー ( ) で AEM オーサーに移動します。 `http://localhost:4502`. AEM Start 画面で、に移動します。 *[ツール/導入/パッケージ](http://localhost:4502/crx/packmgr/index.jsp)*.
9. AEM用のパッケージをダウンロードしてアップロードします ( 上記の *[前提条件/ AEM](#aem)*)
   * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
   * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
   * [core.wcm.components.all-2.5.0.zip](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
   * [digital-data-layer.zip](assets/implementation/digital-data-layer.zip)

   >[!VIDEO](https://video.tv.adobe.com/v/28377?quality=12&learn=on)
10. AEM オーサーにパッケージをインストールした後、AEM Package Manager でアップロードされた各パッケージを選択し、「 」を選択します。 **詳細 > レプリケート** パッケージを AEM パブリッシュにデプロイする場合。
11. この時点で、WKND リファレンスサイトと、このチュートリアルに必要なすべての追加パッケージが正常にインストールされました。

[次の章](./using-launch-adobe-io.md):次の章では、Launch をAEMと統合します。
