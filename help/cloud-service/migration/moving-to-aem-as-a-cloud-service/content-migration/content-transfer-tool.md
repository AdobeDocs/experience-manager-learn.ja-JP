---
title: コンテンツ転送ツールを使用したコンテンツ移行
description: コンテンツ転送ツールを使用して、AEM 6 からas a Cloud Serviceにコンテンツを移行する方法を説明します。
version: Cloud Service
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8919
thumbnail: 336970.jpeg
exl-id: c51ce8e3-e83c-4f8b-a835-70335ed3a5b9
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '359'
ht-degree: 2%

---


# コンテンツ転送ツール

コンテンツ転送ツールを使用して、AEM 6.3 以降からas a Cloud Serviceにコンテンツを移行する方法を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/336970?quality=12&learn=on)

## コンテンツ転送ツールの使用

![コンテンツ転送ツールのライフサイクル](../assets/content-transfer-tool.png)

コンテンツ転送ツールは、AEM 6.3 以降にインストールされ、コンテンツをAEM as a Cloud Serviceに転送します。

## 主要なアクティビティ

+ をダウンロードします。 [最新のコンテンツ転送ツール](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Content*+Transfer*+Tool*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Autoling&amp;orderby=%40jcr%3Fjcr%3AlastOrderby.sort&amp;layout=list&amp;p.offset=0&amp;p.limit=2).
+ AEM Author 6.3 以降の最終コンテンツをAEMas a Cloud Serviceオーサーサービスに転送します。
   + 転送する最終コンテンツを含むAEM 6.3 以降のオーサーにコンテンツ転送ツールをインストールします。
   + コンテンツ転送ツールをバッチで実行し、コンテンツのセットを転送します。
+ AEM Publish 6.3 以降の最終コンテンツをAEM as a Cloud Service Publish Service に転送します。
   + 転送する最終コンテンツを含むAEM 6.3 以降の Publish にコンテンツ転送ツールをインストールします。
   + コンテンツ転送ツールをバッチで実行し、コンテンツのセットを転送します。
+ （オプション）最後のコンテンツ転送以降に新しいコンテンツを転送して、AEMas a Cloud Service上の「追加」コンテンツを作成する

## 実践練習

この実践練習で学んだことを試して、知識を適用します。

実践練習を行う前に、上記のビデオを視聴し、理解し、次の資料を確認してください。

+ [AEM モダナイゼーションツール](../aem-modernization-tools.md)
+ [オンボーディング](../onboarding.md)
+ [Cloud Manager](../cloud-manager.md)

また、前の実践演習を完了していることを確認します。

+ [Dispatcher 実践演習](../dispatcher.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session6-transfercontent#cloud-acceleration-bootcamp---session-6-content"><img alt="実践エクササイズ GitHub リポジトリ" src="../assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">コンテンツ転送ツールを使用した実践</div>
            <p style="margin:1rem 0">
                コンテンツ転送ツールを使用して、コンテンツをAEM 6 からAEM as a Cloud Serviceに自動的に移動する方法を確認します。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session6-transfercontent#cloud-acceleration-bootcamp---session-6-content" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">コンテンツ転送ツールを試す</span>
            </a>
        </td>
    </tr>
</table>

## その他のリソース

+ [コンテンツ転送ツールをダウンロード](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Content*+Transfer*+Tool*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Autoling&amp;orderby=%40jcr%3Fjcr%3AlastOrderby.sort&amp;layout=list&amp;p.offset=0&amp;p.limit=2)
+ [一括読み込みサービスのハウツービデオ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/bulk-import.html)

