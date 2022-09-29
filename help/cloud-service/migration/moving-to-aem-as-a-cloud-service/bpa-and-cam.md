---
title: BPA および CAM プロジェクトを設定する
description: ベストプラクティスアナライザーと Cloud Acceleration Manager が、AEM as a Cloud Serviceへの移行に関するカスタマイズされたガイドを提供する方法を説明します。
version: Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8627
thumbnail: 336957.jpeg
exl-id: f8289dd4-b293-4b8f-b14d-daec091728c0
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 4%

---

# ベストプラクティスアナライザーと Cloud Acceleration Manager

ベストプラクティスアナライザー (BPA) と Cloud Acceleration Manager(CAM) が、AEM as a Cloud Serviceへの移行に関するカスタマイズされたガイドを提供する方法について説明します。 

>[!VIDEO](https://video.tv.adobe.com/v/336957/?quality=12&learn=on)

## BPA と CAM の使用

![BPA および CAM の概要図](assets/bpa-cam-diagram.png)

BPA パッケージは、実稼動AEM 6.x 環境のクローンにインストールする必要があります。 BPA は、レポートを生成し、その後、CAM にアップロードできます。このレポートは、AEMas a Cloud Serviceに移行するために実行する必要のある主要なアクティビティに関するガイダンスを提供します。

## 主要なアクティビティ

+ 実稼動 6.x 環境のクローンを作成します。 コンテンツとリファクタリングコードを移行する際に、実稼動環境のクローンを作成すると、様々なツールや変更をテストする際に役立ちます。
+ 最新の BPA ツールをからダウンロードします。 [ソフトウェア配布ポータル](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html) およびをAEM 6.x のクローン環境にインストールします。
+ BPA ツールを使用して、Cloud Acceleration Manager(CAM) にアップロードできるレポートを生成します。 CAM には、 [https://experience.adobe.com/](https://experience.adobe.com/) > **Experience Manager** > **Cloud Acceleration Manager**.
+ CAM を使用して、AEM as a Cloud Serviceに移行するために現在のコードベースと環境に対して必要な更新に関するガイダンスを提供します。

## 実践練習

この実践練習で学んだことを試して、知識を適用します。

実践練習を行う前に、上記のビデオを視聴し、理解し、次の資料を確認してください。

+ [AEM as a Cloud Serviceについての考え方](./introduction.md)
+ [AEMとは](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/introduction/what-is-aem-as-a-cloud-service.html?lang=en)
+ [AEM as a Cloud Service のアーキテクチャ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/introduction/architecture.html?lang=en)
+ [可変コンテンツと不変コンテンツ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/mutable-immutable.html?lang=en)
+ [AEM as a Cloud Service用とAEM 6.x 用の開発の違い](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html#developing)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session1-differently#bootcamp---session-1-introduction-and-thinking-differently"><img alt="実践エクササイズ GitHub リポジトリ" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">ベストプラクティスアナライザーを使用した実践</div>
            <p style="margin:1rem 0">
                ベストプラクティスアナライザー (BPA) を調べ、サンプル違反を含むレガシー WKND コードベースに対してアナライザーを実行して結果を確認します。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session1-differently#bootcamp---session-1-introduction-and-thinking-differently" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">ベストプラクティスアナライザーの試用</span>
            </a>
        </td>
    </tr>
</table>


## その他のリソース

+ [ベストプラクティスアナライザーのダウンロード](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Best*+Practices*+Analyzer*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)