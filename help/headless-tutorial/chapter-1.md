---
title: 第1章 — チュートリアルのセットアップとダウンロード
seo-title: AEM Content Services使用の手引き — 第1章 — チュートリアルの設定
description: AEMヘッドレスチュートリアルの第1章では、チュートリアル用のAEMインスタンスの基準設定について説明します。
seo-description: AEMヘッドレスチュートリアルの第1章では、チュートリアル用のAEMインスタンスの基準設定について説明します。
translation-type: tm+mt
source-git-commit: ecbd4d21c5f41b2bc6db3b409767b767f00cc5d1
workflow-type: tm+mt
source-wordcount: '450'
ht-degree: 1%

---


# チュートリアルの設定

AEMおよびAEM WCMコアコンポーネントの最新バージョンを常にお勧めします。

* AEM 6.5 以降
* AEM WCMコアコンポーネント2.4.0以降
   * 以下の [WKND Mobile AEMアプリケーションコンテンツパッケージに含まれています](#wknd-mobile-application-packages)

このチュートリアルを開始する前に、次のAEMインスタンスがローカルマシンに [インストールされ、実行されていることを確認してください](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#Default%20Local%20Install)。

* **AEM作成者** ( **ポート4502)**
* **ポート4503でのAEM Publish****。**

## WKNDモバイルアプリケーションパッケージ{#wknd-mobile-application-packages}

を使用して、AEM AuthorとAEM Publishの **両方に** 、次のAEMコンテンツパッケージをインストール [!DNL AEM Package Manager]します。

* [ui.apps:GitHub/アセット/com.adobe.aem.guides.wknd-mobile.ui.apps-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile Empty Template Type]
   * [!DNL WKND Mobile] AEM WCMコアコンポーネントのプロキシコンポーネント
   * [!DNL WKND Mobile] AEM Content ServicesページのCSS（マイナースタイルの場合）
* [ui.content:GitHub > com.adobe.aem.guides.wknd-mobile.ui.content-x.x.x.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
   * [!DNL WKND Mobile] サイト構造
   * [!DNL WKND Mobile] DAMフォルダー構造
   * [!DNL WKND Mobile] 画像アセット

第7 [章では、](./chapter-7.md) Android Studio [!DNL WKND Mobile] と付属のAPK(Android Application Package)を使用して [](https://developer.android.com/studio) Androidモバイルアプリを実行します。

* [[!DNL Androidモバイルアプリ：GitHub/アセット/wknd-mobile.x.x.x.apk]](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## 第AEMコンテンツパッケージ

この一連のコンテンツパッケージは、関連するチャプターおよび前のすべてのチャプターで説明されているコンテンツと設定を作成します。 これらのパッケージはオプションですが、コンテンツの作成を迅速に行うことができます。

* [第2章内容：GitHub/アセット/com.adobe.aem.guides.wknd-mobile.content.chapter-2.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第3章内容：GitHub/アセット/com.adobe.aem.guides.wknd-mobile.content.chapter-3.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第4章内容：GitHub/アセット/com.adobe.aem.guides.wknd-mobile.content.chapter-4.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
* [第5章内容：GitHub/アセット/com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)

## ソースコード

AEMプロジェクトととの両方のソースコード [!DNL Android Mobile App] は、 [[!DNL AEM Guides - WKND Mobile GitHub Project]で入手できます](https://github.com/adobe/aem-guides-wknd-mobile)。 このチュートリアルでは、ソースコードを作成または変更する必要はありません。チュートリアルのあらゆる要素を構築する方法を完全に透明にするために用意されています。

チュートリアルまたはコードに問題がある場合は、 [GitHubの問題を残してください](https://github.com/adobe/aem-guides-wknd-mobile/issues)。

## 最後までスキップ

チュートリアルの最後まで進むために、 [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) コンテンツパッケージをAEM AuthorとAEM Publishの **** 両方にインストールできます。 コンテンツと設定はAEM Authorで公開されたものとして表示されませんが、手動でのデプロイにより、必要なすべてのコンテンツと設定がAEM Publishで利用でき、コンテンツにアクセスでき [!DNL WKND Mobile App] るようになります。


## 次の手順

* [第2章 —イベントコンテンツフラグメントモデルの定義](./chapter-2.md)
