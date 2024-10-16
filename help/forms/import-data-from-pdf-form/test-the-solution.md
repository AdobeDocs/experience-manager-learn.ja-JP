---
title: PDF ファイルからアダプティブフォームへのデータの読み込み
description: PDFファイルの読み込みによるアダプティブフォームへのデータの入力に関するチュートリアル
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f21753b2-f065-4556-add4-b1983fb57031
duration: 21
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '117'
ht-degree: 100%

---

# サンプルアセットのデプロイ

サンプルアセットをデプロイして、ローカルの AEM Forms インスタンス上でこのソリューションを動作させることができます

* [パッケージマネージャーを使用して PDF フォームをアップロードするためのクライアントライブラリとカスタムコンポーネントを読み込む](./assets/client-libs-custom-component.zip)
* OSGi web コンソールの[カスタムドキュメントサービスバンドル](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)を使用して、バンドルをダウンロードおよびデプロイ
* OSGi web コンソールの[サービスユーザーバンドルを使用した開発](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)を使用して、バンドルをダウンロードおよびデプロイ
* OSGi web コンソールの [PDF ファイルからデータを読み込み](./assets/onlineToOffline.core-1.0.0-SNAPSHOT.jar)を使用して、バンドルをダウンロードおよびデプロイ
* _**Apache Sling Service User Mapper Service**_ OSGi 設定コンソールにエントリ _**DevelopingWithServiceUser.core:getresourceresolver=data**_ を追加
