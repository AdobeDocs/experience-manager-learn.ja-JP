---
title: AEM as a Cloud Serviceでの検索とインデックス作成
description: AEMas a Cloud Serviceの検索インデックス、AEM 6 のインデックス定義の変換方法、およびインデックスのデプロイ方法について説明します。
version: Cloud Service
feature: Search
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8634
thumbnail: 336963.jpeg
exl-id: f752df86-27d4-4dbf-a3cb-ee97b7d9a17e
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '309'
ht-degree: 2%

---

# 検索とインデックス作成

AEMas a Cloud Serviceの検索インデックス、AEM 6 のインデックス定義をAEMにas a Cloud Service的な互換性を持たせる方法、およびインデックスをAEM as a Cloud Serviceにデプロイする方法について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/336963?quality=12&learn=on)

## インデックスコンバータツール

![インデックスコンバータツール](./assets/index-converter.png)

コードベースのリファクタリングの一環として、 [インデックスコンバーターツール](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) カスタムの Oak インデックス定義をAEMas a Cloud Serviceの互換性のあるインデックス定義に変換する場合。

## 主要なアクティビティ

+ 以下を使用： [Adobe I/OWorkflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) ツールを使用してアセット処理ワークフローを移行し、Asset computeマイクロサービスを使用する
+ の設定 [ローカル開発環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ja) カスタマイズされたインデックスをデプロイします。 更新されたインデックスが最新であることを確認します。
+ 更新されたコードベースをAEMas a Cloud Serviceの開発環境にデプロイし、検証を続行します。
+ 標準提供のインデックスを変更する場合 **常に** 最新のリリースで実行されているAEMas a Cloud Service環境から最新のインデックス定義をコピーします。 必要に応じて、コピーしたインデックス定義を変更します。

## 実践練習

この実践練習で学んだことを試して、知識を適用します。

実践練習を行う前に、上記のビデオを視聴し、理解し、次の資料を確認してください。

+ [AEM as a Cloud Serviceについての考え方](./introduction.md)
+ [リポジトリの最新化](./repository-modernization.md)

また、前の実践演習を完了していることを確認します。

+ [コンテンツ転送ツールの実践演習](./content-migration/content-transfer-tool.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing"><img alt="実践エクササイズ GitHub リポジトリ" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">インデックス付き実践</div>
            <p style="margin:1rem 0">
                Oak インデックスの定義とAEM as a Cloud Serviceへのデプロイについて説明します。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">インデックス作成を試す</span>
            </a>
        </td>
    </tr>
</table>
