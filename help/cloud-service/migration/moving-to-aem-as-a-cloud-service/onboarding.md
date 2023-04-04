---
title: AEM as a Cloud Service のオンボーディング
description: Cloud Manager を使用した環境の設定に至るまで、契約段階から始めて、AEMas a Cloud Serviceのオンボーディングについて説明します。
version: Cloud Service
feature: Onboarding
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8631
thumbnail: 336959.jpeg
exl-id: 9d2004e5-e928-4190-8298-695635c8e92c
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 22%

---

# AEM as a Cloud Service のオンボーディング

契約段階から Cloud Manager を使用した環境のセットアップまで、AEM as a Cloud Service へのオンボーディングについて説明します。

>[!VIDEO](https://video.tv.adobe.com/v/336959?quality=12&learn=on)

## Cloud Manager と Admin Console

![概要図のオンボーディング](assets/onboarding-diagram.png)

オンボーディングの重要な部分は、AEMas a Cloud Serviceのプログラムを作成し、Cloud Manager を使用して様々な環境をプロビジョニングすることです。 この [Admin Console](https://adminconsole.adobe.com/) は、役割を割り当て、組織内のユーザーにAEM環境へのアクセスを提供するために使用します。

## 主要なアクティビティ

+ システム管理者は、 [Admin Console](https://adminconsole.adobe.com/) 1 人以上のユーザーを [Cloud Manager — ビジネスオーナー](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/setting-up-users-and-roles.html?lang=ja) 製品プロファイル。
+ ビジネスオーナー製品プロファイルに割り当てられたユーザーは、 [Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html?lang=ja) から [プログラムを作成](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/production-programs/creating-production-program.html) および [環境の追加](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/using-cloud-manager/manage-environments.html?lang=ja)
+ 以下を使用： [Admin Console](https://adminconsole.adobe.com/) 開発者とユーザーを別の [Cloud Manager のロール](https://experienceleague.adobe.com/docs/experience-manager-cloud-manager/using/requirements/setting-up-users-and-roles.html?lang=ja) 様々なAEM環境に権限を付与できます。

## 実践練習

この実践練習で学んだことを試して、知識を適用します。

実践練習を行う前に、上記のビデオを視聴し、理解し、次の資料を確認してください。

+ [AEM as a Cloud Serviceについての考え方](./introduction.md)
+ [Cloud Manager](./cloud-manager.md)

また、前の実践演習を完了していることを確認します。

+ [AEM Modernization Tools 実践演習](./aem-modernization-tools.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session3-onboarding#bootcamp---session-3-on-boarding"><img alt="実践エクササイズ GitHub リポジトリ" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">オンボーディングを使用したハンズオン</div>
            <p style="margin:1rem 0">
                AEMas a Cloud Serviceのオンボーディングプロセスと、AEM SDK にAEMアプリケーションをデプロイする方法について説明します。
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session3-onboarding#bootcamp---session-3-on-boarding" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">オンボーディングを試す</span>
            </a>
        </td>
    </tr>
</table>
