---
title: 第1章 — チュートリアルの設定とダウンロード — コンテンツサービス
seo-title: AEM Content Servicesの概要 — 第1章 — チュートリアルの設定
description: AEMヘッドレスチュートリアルの第1章は、このチュートリアルのAEMインスタンスのベースライン設定です。
seo-description: AEMヘッドレスチュートリアルの第1章は、このチュートリアルのAEMインスタンスのベースライン設定です。
feature: コンテンツフラグメント、API
topic: ヘッドレス、コンテンツ管理
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '444'
ht-degree: 1%

---


# チュートリアルの設定

AEMおよびAEM WCMコアコンポーネントの最新バージョンを常にお勧めします。

* AEM 6.5 以降
* AEM WCMコアコンポーネント2.4.0以降
   * [の下の](#wknd-mobile-application-packages)WKND Mobile AEM Application Content Packageに含まれます。

このチュートリアルを開始する前に、次のAEMインスタンスがローカルマシン](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install)に[インストールされ、実行されていることを確認してください。

* **AEM** Authoronポ **ート4502**
* **AEM** Publishonポ **ート4503**

## WKNDモバイルアプリケーションパッケージ{#wknd-mobile-application-packages}

[!DNL AEM Package Manager]を使用して、次のAEMコンテンツパッケージを&#x200B;**AEMオーサーとAEMパブリッシュの両方にインストールします。**

* [ui.apps:GitHub /アセット/ com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] AEM WCMコアコンポーネントのプロキシコンポーネント
   * [!DNL WKND Mobile] AEM Content ServicesページのCSS（マイナースタイル設定用）
* [ui.content:GitHub / com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] サイト構造
   * [!DNL WKND Mobile] DAMフォルダー構造
   * [!DNL WKND Mobile] 画像アセット

[第7章](./chapter-7.md)では、[Android Studio](https://developer.android.com/studio)と提供されたAPK（Androidアプリケーションパッケージ）を使用して[!DNL WKND Mobile] Androidモバイルアプリを実行します。

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Chapter AEM Content Packages

この一連のコンテンツパッケージは、関連するチャプターおよびそれ以前のすべてのチャプターで説明されているコンテンツと設定を作成します。 これらのパッケージはオプションですが、コンテンツの作成を迅速化できます。

* [第2章内容：GitHub / Assets / com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第3章内容：GitHub / Assets / com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第4章内容：GitHub / Assets / com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第5章内容：GitHub / Assets / com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## ソースコード

AEMプロジェクトと[!DNL Android Mobile App]のソースコードは、[[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile)で利用できます。 このチュートリアルでは、ソースコードを作成または変更する必要はありません。チュートリアルのすべての側面を構築する方法を完全に透明にするために提供されています。

チュートリアルまたはコードに問題がある場合は、[GitHubの問題](https://github.com/adobe/aem-guides-wknd-mobile/issues)を残してください。

## 最後までスキップ

チュートリアルの最後まで進むために、 [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)コンテンツパッケージを&#x200B;**AEMオーサーとAEMパブリッシュの両方にインストールできます。**&#x200B;コンテンツと設定はAEMオーサーで公開されたものとして表示されませんが、手動でのデプロイにより、AEMパブリッシュで必要なコンテンツと設定がすべて使用でき、[!DNL WKND Mobile App]がコンテンツにアクセスできます。


## 次の手順

* [第2章 — イベントコンテンツフラグメントモデルの定義](./chapter-2.md)
