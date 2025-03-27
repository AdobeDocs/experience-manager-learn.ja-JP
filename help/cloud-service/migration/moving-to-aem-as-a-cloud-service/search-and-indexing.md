---
title: AEM as a Cloud Service での検索とインデックス作成
description: AEM as a Cloud Service の検索インデックス、AEM 6 のインデックス定義の変換方法およびインデックスのデプロイ方法について説明します。
version: Experience Manager as a Cloud Service
feature: Search
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8634
thumbnail: 336963.jpeg
exl-id: f752df86-27d4-4dbf-a3cb-ee97b7d9a17e
duration: 1231
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '297'
ht-degree: 100%

---

# 検索とインデックス作成

AEM as a Cloud Service の検索インデックス、AEM 6 インデックス定義を変換して AEM as a Cloud Service と互換性を持たせる方法、インデックスを AEM as a Cloud Service にデプロイする方法について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/336963?quality=12&learn=on)

## インデックスコンバーターツール

![インデックスコンバーターツール](./assets/index-converter.png)

コードベースのリファクタリングの一環として、[インデックスコンバーターツール](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter)を使用して、カスタム Oak インデックス定義を AEM as a Cloud Service 互換インデックス定義に変換します。

インデックスコンバーターの機能の完全な現在のセットについては、[インデックスコンバーターのドキュメント](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/refactoring-tools/index-converter.html?lang=ja)を参照してください。

## 重要なアクティビティ

+ [Adobe I/O Workflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) ツールを使用してアセット処理ワークフローを移行し、Asset Compute マイクロサービスを使用します。
+ [ローカル開発環境](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=ja)を設定して、カスタマイズされたインデックスをデプロイします。 更新されたインデックスが最新であることを確認します。
+ 更新されたコードベースを AEM as a Cloud Service の開発環境にデプロイし、検証を続行します。
+ 標準提供のインデックスを変更する場合は&#x200B;**常に**、最新のリリースで実行されている AEM as a Cloud Service 環境から最新のインデックス定義をコピーします。 必要に応じて、コピーしたインデックス定義を変更します。

## 実践練習

この実践練習で学んだことを試して、知識を適用します。

実践練習を行う前に、上記のビデオを視聴し、理解し、次の資料を確認してください。

+ [AEM as a Cloud Service についての考え方](./introduction.md)
+ [リポジトリの最新化](./repository-modernization.md)

また、前の実践演習を完了していることを確認します。

+ [コンテンツ移行ツールの実践演習](./content-migration/content-transfer-tool.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing"><img alt="実践演習 GitHub リポジトリ" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">インデックスの実践</div>
            <p style="margin:1rem 0">
                Oak インデックスの定義と AEM as a Cloud Service へのデプロイについて説明します。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">インデックス作成を試す</span>
            </a>
        </td>
    </tr>
</table>
