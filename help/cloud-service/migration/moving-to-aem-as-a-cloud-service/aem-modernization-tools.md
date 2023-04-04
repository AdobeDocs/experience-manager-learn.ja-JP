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
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 6%

---


# AEM モダナイゼーションツール

AEM Modernization Tools を使用して、既存のAEM Sitesコンテンツをアップグレードし、AEMとas a Cloud Serviceした互換性を保ち、ベストプラクティスに合わせる方法を説明します。

## オールインワンコンバータ

>[!VIDEO](https://video.tv.adobe.com/v/338802?quality=12&learn=on)

## ページコンバージョン

>[!VIDEO](https://video.tv.adobe.com/v/338799?quality=12&learn=on)

## コンポーネントの変換

>[!VIDEO](https://video.tv.adobe.com/v/338788?quality=12&learn=on)

## ポリシーのインポート

>[!VIDEO](https://video.tv.adobe.com/v/338797?quality=12&learn=on)

## AEM Modernization Tools の使用

![AEM Modernization Tools のライフサイクル](./assets/aem-modernization-tools.png)

AEM Modernization ツールは、編集可能なテンプレート、AEM Core WCM コンポーネント、レイアウトコンテナなどの最新のアプローチを使用するために、従来の静的テンプレート、基盤コンポーネント、parsys で構成された既存のAEM Pages を自動的に変換します。

## 主要なアクティビティ

+ AEM 6.x の実稼動環境を複製してAEM Modernization ツールを実行する
+ をダウンロードしてインストールする [最新のAEM Modernizations ツール](https://github.com/adobe/aem-modernize-tools/releases/latest) パッケージマネージャーを使用したAEM 6.x の実稼動クローン

+ [ページ構造コンバーター](https://opensource.adobe.com/aem-modernize-tools/pages/structure/about.html) レイアウトコンテナを使用して、既存のページコンテンツを静的テンプレートからマッピングされた編集可能テンプレートに更新します
   + OSGi 設定を使用してコンバージョンルールを定義する
   + 既存のページに対してページ構造コンバーターを実行する

+ [コンポーネントコンバータ](https://opensource.adobe.com/aem-modernize-tools/pages/component/about.html) レイアウトコンテナを使用して、既存のページコンテンツを静的テンプレートからマッピングされた編集可能テンプレートに更新します
   + JCR ノード定義/XML を使用して変換ルールを定義
   + 既存のページに対してコンポーネントコンバーターツールを実行する

+ [ポリシーインポーター](https://opensource.adobe.com/aem-modernize-tools/pages/policy/about.html) デザイン設定からポリシーを作成します
   + JCR ノード定義/XML を使用して変換ルールを定義する
   + 既存のデザイン定義に対してポリシーインポーターを実行する
   + インポートしたポリシーのAEMコンポーネントおよびコンテナへの適用

## 実践練習

この実践練習で学んだことを試して、知識を適用します。

実践練習を行う前に、上記のビデオを視聴し、理解し、次の資料を確認してください。

+ [AEM as a Cloud Serviceについての考え方](./introduction.md)
+ [リポジトリの最新化](./repository-modernization.md)
+ [可変コンテンツと不変コンテンツ](../../developing/basics/mutable-immutable.md)
+ [AEMプロジェクト構造](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html?lang=ja)

また、前の実践演習を完了していることを確認します。

+ [BPA と CAM のハンズオン演習](./bpa-and-cam.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session2-migration#bootcamp---session-2-migration-methodology"><img alt="実践エクササイズ GitHub リポジトリ" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">AEMの最新化に関する実践</div>
            <p style="margin:1rem 0">
                AEM Modernization Tools を使用して、AEMのas a Cloud Serviceのベストプラクティスに従って従来の WKND サイトを更新する方法を説明します。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session2-migration#bootcamp---session-2-migration-methodology" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">AEM Modernization Tools を試す</span>
            </a>
        </td>
    </tr>
</table>

## その他のリソース

+ [AEM Modernizations ツールのダウンロード](https://github.com/adobe/aem-modernize-tools/releases/latest)
+ [AEM Modernization Tools ドキュメント](https://opensource.adobe.com/aem-modernize-tools/)
+ [AEM Gems - AEM Modernization Suite の概要](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/Introducing-the-AEM-Modernization-Suite.html)

1. ローカルAEM SDK に、新しく最新化された wknd-legacy サイトをデプロイします。 AEM ASK は次の場所からダウンロードできます。
   + [ソフトウェア配布ポータル](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html).
