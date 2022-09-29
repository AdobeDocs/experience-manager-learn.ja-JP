---
title: 第 1 章 — チュートリアルの設定とダウンロード — コンテンツサービス
seo-title: Getting Started with AEM Content Services - Chapter 1 -  Tutorial Set up
description: AEMヘッドレスチュートリアルの第 1 章では、このチュートリアルのAEMインスタンスのベースライン設定について説明します。
seo-description: Chapter 1 of the AEM Headless tutorial the baseline setup for the AEM instance for the tutorial.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: f24a75f6-9062-498c-b782-7d9011aa0bcf
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '411'
ht-degree: 2%

---

# チュートリアルの設定

AEMおよびAEM WCM コアコンポーネントの最新バージョンを常にお勧めします。

* AEM 6.5 以降
* AEM WCM Core Components 2.4.0 以降
   * 次に含まれる [以下の WKND Mobile AEM Application コンテンツパッケージ](#wknd-mobile-application-packages)

このチュートリアルを開始する前に、次のAEMインスタンスが [ローカルマシンにインストールされ、実行されている](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install):

* **AEM オーサー** オン **ポート 4502**
* **AEM パブリッシュ** オン **ポート 4503**

## WKND モバイルアプリケーションパッケージ{#wknd-mobile-application-packages}

次のAEMコンテンツパッケージをにインストールします。 **両方** AEM オーサーと AEM パブリッシュ（を使用） [!DNL AEM Package Manager].

* [ui.apps:GitHub /アセット/ com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] AEM WCM コアコンポーネントのプロキシコンポーネント
   * [!DNL WKND Mobile] AEM Content Services ページの CSS（マイナースタイル設定用）
* [ui.content:GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] サイト構造
   * [!DNL WKND Mobile] DAM フォルダー構造
   * [!DNL WKND Mobile] 画像アセット

In [第 7 章](./chapter-7.md) 私たちが実行します [!DNL WKND Mobile] Android モバイルアプリ（を使用） [Android Studio](https://developer.android.com/studio) と、提供された APK（Android アプリケーションパッケージ）:

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## Chapter AEM Content Packages

この一連のコンテンツパッケージは、関連する章および前の章で説明したコンテンツと設定を作成します。 これらのパッケージはオプションですが、コンテンツの作成を迅速におこなうことができます。

* [第 2 章内容：GitHub / Assets / com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第 3 章内容：GitHub / Assets / com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第 4 章内容：GitHub / Assets / com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第 5 章内容：GitHub / Assets / com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## ソースコード

AEMプロジェクトと [!DNL Android Mobile App] は [[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile). このチュートリアルでは、ソースコードを作成または変更する必要はありません。チュートリアルのすべての側面を構築する方法を完全に透明にするために提供されています。

チュートリアルまたはコードに問題がある場合は、 [GitHub の問題](https://github.com/adobe/aem-guides-wknd-mobile/issues).

## 最後までスキップ

チュートリアルの最後まで進むには、 [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) コンテンツパッケージは次の場所にインストールできます。 **両方** AEM オーサーと AEM パブリッシュ コンテンツと設定は AEM オーサーで公開されたとおりに表示されませんが、手動でのデプロイメントにより、AEM Publish で必要なすべてのコンテンツと設定を使用して、 [!DNL WKND Mobile App] をクリックして、コンテンツにアクセスします。


## 次の手順

* [第 2 章 — イベントコンテンツフラグメントモデルの定義](./chapter-2.md)
