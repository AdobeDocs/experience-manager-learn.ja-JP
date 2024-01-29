---
title: AEM Sites と Adobe Target の統合
description: 様々なシナリオで Adobe Target と Adobe Experience Manager の連携をセットアップする方法を説明する記事です。
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 54a30cd9-d94a-4de5-82a1-69ab2263980d
duration: 173
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '595'
ht-degree: 100%

---

# AEM Sites と Adobe Target の統合

この節では、様々なシナリオで Adobe Experience Manager Sites と Adobe Target の連携をセットアップする方法について説明します。シナリオと組織要件に基づいています。

* **Adobe Target JavaScript ライブラリの追加（すべてのシナリオで必要）**
AEM でホストされるサイトの場合、[Launch](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=ja) を使用して Target ライブラリをサイトに追加できます。 Launch は、関連性の高いカスタマーエクスペリエンス（顧客体験）の実現に必要なすべてのタグをデプロイおよび管理するためのシンプルな手段を提供します。
* **Adobe Target クラウドサービスの追加（エクスペリエンスフラグメントシナリオで必要）**
AEM のお客様がエクスペリエンスフラグメントオファーを使用してAdobe Target 内でアクティビティを作成する場合、従来のクラウドサービスを使用して Adobe Target と AEM を統合する必要があります。この統合は、AEM からエクスペリエンスフラグメントを HTML／JSON オファーとして Target にプッシュし、オファーと AEM の同期を保つために必要です。*この統合は、シナリオ 1 の実装に必要です。*

## 前提条件

* **Adobe Experience Manager（AEM）{#aem}**
   * AEM 6.5（*最新のサービスパックを推奨*）
   * AEM WKND リファレンスサイトパッケージをダウンロードします。
      * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
      * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
      * [コアコンポーネント](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
      * [デジタルデータレイヤー](assets/implementation/digital-data-layer.zip)

* **Experience Cloud**
   * 組織の Adobe Experience Cloud にアクセス - `https://<yourcompany>.experiencecloud.adobe.com`
   * 次のソリューションをプロビジョニングされた Experience Cloud
      * [Adobe Experience Platform Launch](https://experiencecloud.adobe.com)
      * [Adobe Target](https://experiencecloud.adobe.com)
      * [Adobe I/O コンソール](https://console.adobe.io)

* **環境**
   * Java 1.8 または Java 11（AEM 6.5 以降のみ）
   * Apache Maven（3.3.9 以降）
   * Chrome

>[!NOTE]
>
> 顧客は、[アドビサポート](https://helpx.adobe.com/jp/contact/enterprise-support.ec.html)から Experience Platform Launch と Adobe I/O をプロビジョニングするか、システム管理者に連絡する必要があります。

### AEM の設定{#set-up-aem}

このチュートリアルを完了するには、AEM オーサーインスタンスとパブリッシュインスタンスが必要です。 オーサーインスタンスが `http://localhost:4502` で動作し、パブリッシュインスタンスが `http://localhost:4503` で動作しています。詳しくは、[ローカル AEM 開発環境の設定](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/development/set-up-a-local-aem-development-environment.html?lang=ja)を参照してください。

#### AEM オーサーインスタンスとパブリッシュインスタンスのセットアップ

1. [AEM クイックスタート JAR とライセンス](https://helpx.adobe.com/jp/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware)のコピーを取得します。
2. 次のようなフォルダー構造をコンピューター上に作成します。
   ![フォルダー構造](assets/implementation/aem-setup-1.png)
3. クイックスタート JAR の名前を `aem-author-p4502.jar` に変更して、`/author` ディレクトリの下に置きます。`license.properties` ファイルを `/author` ディレクトリの下に追加します。
   ![AEM オーサーインスタンス](assets/implementation/aem-setup-author.png)
4. クイックスタート JAR のコピーを作成し、名前を `aem-publish-p4503.jar` に変更して、`/publish` ディレクトリの下に置きます。`license.properties` ファイルのコピーを `/publish` ディレクトリの下に追加します。
   ![AEM パブリッシュインスタンス](assets/implementation/aem-setup-publish.png)
5. `aem-author-p4502.jar` ファイルをダブルクリックして、オーサーインスタンスをインストールします。 これによりオーサーインスタンスが起動され、ローカルコンピューターのポート 4502 で動作します。
6. 以下の資格情報を使用してログインします。ログインに成功すると、AEM ホームページ画面に移動します。
ユーザー名： **admin**
パスワード： **admin**
   ![AEM パブリッシュインスタンス](assets/implementation/aem-author-home-page.png)
7. `aem-publish-p4503.jar` ファイルをダブルクリックして、パブリッシュインスタンスをインストールします。 ポート 4503 で動作するパブリッシュインスタンス用に新しいタブがブラウザーで開き、WeRetail ホームページが表示されることがわかります。 このチュートリアルでは WKND リファレンスサイトを使用しており、オーサーインスタンスにパッケージをインストールします。
8. Web ブラウザーで AEM オーサー（`http://localhost:4502`）に移動します。AEM 開始画面で&#x200B;*[ツール／デプロイメント／パッケージ](http://localhost:4502/crx/packmgr/index.jsp)*&#x200B;に移動します。
9. AEM 用のパッケージ（上記の&#x200B;*[前提条件／AEM](#aem)* の下にリストされています）をダウンロードしてアップロードします。
   * [aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.apps-0.0.1-SNAPSHOT.zip)
   * [aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip](https://github.com/adobe/aem-guides-wknd/releases/download/archetype-18.1/aem-guides-wknd.ui.content-0.0.1-SNAPSHOT.zip)
   * [core.wcm.components.all-2.5.0.zip](https://github.com/adobe/aem-core-wcm-components/releases/download/core.wcm.components.reactor-2.5.0/core.wcm.components.all-2.5.0.zip)
   * [digital-data-layer.zip](assets/implementation/digital-data-layer.zip)

   >[!VIDEO](https://video.tv.adobe.com/v/28377?quality=12&learn=on)
10. AEM オーサーにパッケージをインストールした後、アップロードされた各パッケージを AEM パッケージマネージャーで選択し、**詳細／レプリケート**&#x200B;を選択して、パッケージが AEM パブリッシュにデプロイされていることを確認します。 
11. これで、WKND リファレンスサイトと、このチュートリアルに必要なすべての追加パッケージが正常にインストールされました。

[次の章](./using-launch-adobe-io.md)：次の章では、Launch を AEM と統合します。
