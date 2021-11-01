---
title: AEM Modernization Tools を使用したAEM as a Cloud Serviceへの移行
description: AEM Modernization Tools を使用して、既存のAEMプロジェクトとコンテンツをアップグレードし、AEMとas a Cloud Serviceの互換性を保つ方法について説明します。
version: Cloud Service
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8629
thumbnail: 336965.jpeg
exl-id: 310f492c-0095-4015-81a4-27d76f288138
source-git-commit: 3657e7798774f9cc673ff6ccd8af1a555b1d4013
workflow-type: tm+mt
source-wordcount: '325'
ht-degree: 10%

---


# AEM Modernization Tools

AEM Modernization Tools を使用して、既存のAEM Sitesコンテンツをアップグレードし、AEMとas a Cloud Serviceした互換性を保ち、ベストプラクティスに合わせる方法を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/336965/?quality=12&learn=on)

## AEM Modernization Tools の使用

![AEM Modernization Tools のライフサイクル](./assets/aem-modernization-tools.png)

AEM Modernization ツールは、編集可能なテンプレート、AEM Core WCM コンポーネント、レイアウトコンテナなどの最新のアプローチを使用するために、従来の静的テンプレート、基盤コンポーネント、parsys で構成された既存のAEM Pages を自動的に変換します。

### 重要なアクティビティ

+ AEM 6.x の実稼動環境を複製してAEM Modernization ツールを実行する
+ をダウンロードしてインストールする [最新のAEM Modernizations ツール](https://github.com/adobe/aem-modernize-tools/releases/latest) パッケージマネージャーを使用したAEM 6.x の実稼動クローン

+ [ページ構造コンバーター](https://opensource.adobe.com/aem-modernize-tools/pages/tools/page-structure.html) レイアウトコンテナを使用して、既存のページコンテンツを静的テンプレートからマッピングされた編集可能テンプレートに更新します
   + OSGi 設定を使用してコンバージョンルールを定義する
   + 既存のページに対してページ構造コンバーターを実行する

+ [コンポーネントコンバータ](https://opensource.adobe.com/aem-modernize-tools/pages/tools/component.html) レイアウトコンテナを使用して、既存のページコンテンツを静的テンプレートからマッピングされた編集可能テンプレートに更新します
   + JCR ノード定義/XML を使用して変換ルールを定義
   + 既存のページに対してコンポーネントコンバーターツールを実行する

+ [ポリシーインポーター](https://opensource.adobe.com/aem-modernize-tools/pages/tools/policy-importer.html) デザイン設定からポリシーを作成します
   + JCR ノード定義/XML を使用して変換ルールを定義する
   + 既存のデザイン定義に対してポリシーインポーターを実行する
   + インポートしたポリシーのAEMコンポーネントおよびコンテナへの適用

+ [ダイアログコンバータ](https://opensource.adobe.com/aem-modernize-tools/pages/tools/dialog.html) Classic(ExtJS) および CoralUI 2 ベースのコンポーネントダイアログを CoralUI 3 TouchUI ベースのダイアログに変換します。
   + 既存の ExtJS または Coral2 UI ベースのダイアログに対して Dialog Converter ツールを実行します。
   + 変換されたダイアログを Git リポジトリに同期

### Adobe  に関するその他のリソース

+ [AEM Modernizations ツールのダウンロード](https://github.com/adobe/aem-modernize-tools/releases/latest)
+ [AEM Modernization Tools ドキュメント](https://opensource.adobe.com/aem-modernize-tools/)
+ [AEM Gems - AEM Modernization Suite の概要](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/Introducing-the-AEM-Modernization-Suite.html)
