---
title: アダプティブフォームへのPDFファイルからのデータの読み込み
description: PDFファイルの読み込みによるアダプティブフォームの入力に関するチュートリアル
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
source-git-commit: 17ab178f385619b589a9dde6089410bfa4515ffa
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 3%

---

# サンプルアセットのデプロイ

サンプルアセットをデプロイして、ローカルのAEM Formsインスタンス上でこのソリューションを動作させることができます

* [パッケージマネージャーを使用して PDF フォームをアップロードするためのクライアントライブラリとカスタムコンポーネントを読み込みます。](./assets/client-libs-custom-component.zip)
* OSGi Web コンソールを使用してバンドルをダウンロードしてデプロイします。[カスタムドキュメントサービスバンドル](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* OSGi Web コンソールを使用してバンドルをダウンロードしてデプロイします。 [サービスユーザーバンドルを使用した開発](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* OSGi Web コンソールを使用してバンドルをダウンロードしてデプロイします。[pdf ファイルからデータを読み込み](./assets/onlineToOffline.core-1.0.0-SNAPSHOT.jar)
* エントリを追加 _**DevelopingWithServiceUser.core:getresourceresolver=data**_ （内） _**Apache Sling Service User Mapper Service**_ OSGi 設定コンソール