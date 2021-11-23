---
title: AEM as a Cloud Serviceへの移行時の Dispatcher の設定
description: AEM Dispatcher for AEMの主な変更点、Dispatcher 変換ツール、および Dispatcher ツール SDK の使用方法について説明します。
version: Cloud Service
feature: Dispatcher
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8633
thumbnail: 336962.jpeg
exl-id: 81397b21-b4f3-4024-a6da-a9b681453eff
source-git-commit: 1dcb66bc3535231c89f3e7fc127688fcf96f2b61
workflow-type: tm+mt
source-wordcount: '277'
ht-degree: 3%

---


# Dispatcher

AEM 6 向けAEM Dispatcher の主な変更点、Dispatcher 変換ツール、および Dispatcher ツール SDK の使用方法に重点を置いた、AEMas a Cloud Service向け Dispatcher について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/336962/?quality=12&learn=on)

## Dispatcher コンバーター

![Dispatcher コンバーター](./assets/dispatcher-converter-diagram.png)

コードベースのリファクタリングの一環として、 [AEM Dispatcher Converter](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/dispatcher-transformation-utility-tools.html) 既存のオンプレミス設定または Adobe Managed Services Dispatcher 設定を、AEM as a Cloud Serviceと互換性のある Dispatcher 設定にリファクタリングする場合。

## 主要なアクティビティ

+ 以下を使用： [Adobe I/ODispatcher コンバーターツール](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#aio-aem-migrationdispatcher-converter) 既存の Dispatcher 設定を移行する場合。
+ Dispatcher モジュールを [AEM Project Archetype](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud) ベストプラクティスとして。
+ [ローカル Dispatcher ツールの設定](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html) dispatcher を検証してから、dispatcher 環境でテストをCloud Serviceします。

## 実践練習

この実践練習で学んだことを試して、知識を適用します。

実践練習を行う前に、上記のビデオを視聴し、理解し、次の資料を確認してください。

+ [AEM Modernization Tools](./aem-modernization-tools.md)
+ [オンボーディング ](./onboarding.md)
+ [Cloud Manager](./cloud-manager.md)

また、前の実践演習を完了していることを確認します。

+ [Cloud Manager 実践演習](./cloud-manager.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session5-dispatcher#cloud-acceleration-bootcamp---session-5-dispatcher"><img alt="実践エクササイズ GitHub リポジトリ" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Dispatcher ツールを使用した実践</div>
            <p style="margin:1rem 0">
                AEM SDK の Dispatcher ツールを使用した Dispatcher の設定の検証、および Docker を使用したローカルでのAEM Dispatcher の実行の調査。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session5-dispatcher#cloud-acceleration-bootcamp---session-5-dispatcher" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Dispatcher ツールの試し</span>
            </a>
        </td>
    </tr>
</table>
