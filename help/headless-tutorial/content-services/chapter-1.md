---
title: 第 1 章 - チュートリアルの設定とダウンロード - コンテンツサービス
description: AEM ヘッドレスチュートリアルの第 1 章では、このチュートリアルの AEM インスタンスのベースライン設定について説明します。
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: f24a75f6-9062-498c-b782-7d9011aa0bcf
duration: 85
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 100%

---

# チュートリアルの設定

AEM および AEM WCM コアコンポーネントの最新バージョンを常にお勧めします。

* AEM 6.5 以降
* AEM WCM コアコンポーネント 2.4.0 以降
   * [以下の WKND Mobile AEM アプリケーションのコンテンツパッケージ](#wknd-mobile-application-packages)に含まれる

このチュートリアルを開始する前に、次の AEM インスタンスが[ローカルマシンにインストールされ動作している](https://helpx.adobe.com/jp/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install)ことを確認してください。

* **ポート 4502** の **AEM オーサー**
* **ポート 4503** の **AEM パブリッシュ**

## AEM Mobile アプリケーションパッケージ{#wknd-mobile-application-packages}

[!DNL AEM Package Manager] を使用して、AEM オーサーと AEM パブリッシュの&#x200B;**両方**&#x200B;に次の AEM コンテンツパッケージをインストールします。

* [ui.apps：GitHub／Assets／com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] AEM WCM コアコンポーネントのプロキシコンポーネント
   * [!DNL WKND Mobile] AEM コンテンツサービスページの CSS（マイナースタイル設定用）
* [ui.content：GitHub／com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] サイト構造
   * [!DNL WKND Mobile] DAM フォルダー構造
   * [!DNL WKND Mobile] 画像アセット

[第 7 章](./chapter-7.md)では、[Android Studio](https://developer.android.com/studio) と提供された APK（Android アプリケーションパッケージ）を使用して、[!DNL WKND Mobile] Android モバイルアプリを実行します。

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## AEM コンテンツパッケージの章

この一連のコンテンツパッケージは、関連する章およびその前のすべての章で説明したコンテンツと設定を作成します。これらのパッケージはオプションですが、コンテンツの作成を迅速化できます。

* [第 2 章の内容： GitHub／Assets／com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第 3 章の内容：GitHub／Assets／com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第 4 章の内容：GitHub／Assets／com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第 5 章の内容：GitHub／Assets／com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## ソースコード

AEM プロジェクトと [!DNL Android Mobile App] の両方のソースコードは、[[!DNL AEM Guides - WKND Mobile GitHub Project] で入手できます](https://github.com/adobe/aem-guides-wknd-mobile)。 このチュートリアルでは、ソースコードを作成したり変更したりする必要はありません。チュートリアルのあらゆる側面を作成する方法を十分明確に理解できるように提供されています。

チュートリアルまたはコードに問題を見つけた場合は、[GitHub の問題](https://github.com/adobe/aem-guides-wknd-mobile/issues)に報告してください。

## 最後までスキップ

チュートリアルの最後までスキップするには、[com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) コンテンツパッケージを、AEM オーサーと AEM パブリッシュの&#x200B;**両方**&#x200B;にインストールします。コンテンツと設定は AEM オーサーで公開されたとおりには表示されませんが、手動でのデプロイメントにより、必要なすべてのコンテンツと設定が AEM パブリッシュで利用可能になり、[!DNL WKND Mobile App] からコンテンツにアクセスできます。


## 次の手順

* [第 2 章 - イベントコンテンツフラグメントモデルの定義](./chapter-2.md)
