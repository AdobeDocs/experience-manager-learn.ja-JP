---
title: 第1章 — チュートリアルのセットアップとダウンロード
seo-title: AEM Content Services使用の手引き — 第1章 — チュートリアルの設定
description: AEMヘッドレスチュートリアルの第1章では、チュートリアル用のAEMインスタンスの基準設定について説明します。
seo-description: AEMヘッドレスチュートリアルの第1章では、チュートリアル用のAEMインスタンスの基準設定について説明します。
translation-type: tm+mt
source-git-commit: ecbd4d21c5f41b2bc6db3b409767b767f00cc5d1
workflow-type: tm+mt
source-wordcount: '436'
ht-degree: 1%

---


# チュートリアルの設定

AEMおよびAEM WCMコアコンポーネントの最新バージョンを常にお勧めします。

* AEM 6.5 以降
* AEM WCMコアコンポーネント2.4.0以降
   * ](#wknd-mobile-application-packages)の下の[WKND Mobile AEMアプリケーションコンテンツパッケージに含まれます

このチュートリアルを開始する前に、次のAEMインスタンスがローカルマシン](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install)に[インストールされ、実行されていることを確認してください。

* **AEM** Authoron **ポート4502**
* **AEM** Publishon **ポート4503**

## WKNDモバイルアプリケーションパッケージ{#wknd-mobile-application-packages}

[!DNL AEM Package Manager]を使用して、**** AEM AuthorとAEM Publishの両方に、次のAEMコンテンツパッケージをインストールします。

* [ui.apps:GitHub/アセット/com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] AEM WCMコアコンポーネントのプロキシコンポーネント
   * [!DNL WKND Mobile] AEM Content ServicesページのCSS（マイナースタイルの場合）
* [ui.content:GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] サイト構造
   * [!DNL WKND Mobile] DAMフォルダー構造
   * [!DNL WKND Mobile] 画像アセット

[第7章](./chapter-7.md)では、[Android Studio](https://developer.android.com/studio)と提供されたAPK（Androidアプリケーションパッケージ）を使用して[!DNL WKND Mobile] Androidモバイルアプリを実行します。

* [[!DNL Android Mobile App: GitHub > Assets > wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## 第AEMコンテンツパッケージ

この一連のコンテンツパッケージは、関連するチャプターおよび前のすべてのチャプターで説明されているコンテンツと設定を作成します。 これらのパッケージはオプションですが、コンテンツの作成を迅速に行うことができます。

* [第2章内容：GitHub/アセット/com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第3章内容：GitHub/アセット/com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第4章内容：GitHub/アセット/com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第5章内容：GitHub/アセット/com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## ソースコード

AEMプロジェクトと[!DNL Android Mobile App]の両方のソースコードは、[[!DNL AEM Guides - WKND Mobile GitHub Project]](https://github.com/adobe/aem-guides-wknd-mobile)で入手できます。 このチュートリアルでは、ソースコードを作成または変更する必要はありません。チュートリアルのあらゆる要素を構築する方法を完全に透明にするために用意されています。

チュートリアルまたはコードに問題がある場合は、[GitHubの問題](https://github.com/adobe/aem-guides-wknd-mobile/issues)を残してください。

## 最後までスキップ

チュートリアルの最後までスキップするには、[com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)コンテンツパッケージを&#x200B;**AEM AuthorとAEM Publishの両方にインストールします。**&#x200B;コンテンツと設定はAEM Authorで公開されたものとして表示されませんが、手動でのデプロイにより、必要なすべてのコンテンツと設定がAEM Publishで利用でき、[!DNL WKND Mobile App]からコンテンツにアクセスできるようになります。


## 次の手順

* [第2章 —イベントコンテンツフラグメントモデルの定義](./chapter-2.md)
