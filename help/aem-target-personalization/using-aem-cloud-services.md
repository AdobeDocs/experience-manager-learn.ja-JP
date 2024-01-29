---
title: クラウドサービスを使用した Adobe Experience Manager と Adobe Target の統合
description: AEM Cloud Service を使用して Adobe Experience Manager（AEM）と Adobe Target を統合する方法を順を追って説明します
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 9b191211-2030-4b62-acad-c7eb45b807ca
duration: 357
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '358'
ht-degree: 100%

---

# AEM の従来のクラウドサービスの使用

この節では、従来のクラウドサービスを使用して Adobe Experience Manager（AEM）と Adobe Target の連携をセットアップする方法を説明します。

>[!NOTE]
>
> AEM の従来のクラウドサービスと Adobe Target の連携は、AEM オーサーから Adobe Target への直接のバックエンド接続を確立して、AEM から Target にコンテンツを公開しやすくするために&#x200B;**のみ**&#x200B;使用します。Adobe Launch は、AEM から提供される公開 web サイトエクスペリエンスに Adobe Target を表示するために使用します。

AEM エクスペリエンスフラグメントオファーを使用してパーソナライゼーションアクティビティを強化するには、次の章に進み、従来のクラウドサービスを使用して AEM と Adobe Target を統合します。この統合は、AEM からエクスペリエンスフラグメントを HTML／JSON オファーとして Target にプッシュし、ターゲットオファーと AEM の同期を保つために必要です。この統合は、[概要の節で説明したシナリオ 1](./overview.md#personalization-using-aem-experience-fragment) を実装するために必要です。

## 前提条件

* **AEM**

   * このチュートリアルを完了するには、AEM オーサーインスタンスおよびパブリッシュインスタンスが必要です。AEM インスタンスをまだセットアップしていない場合は、[こちら](./implementation.md#set-up-aem)の手順に従ってください。

* **Experience Cloud**
   * 組織の Adobe Experience Cloud にアクセス - `https://<yourcompany>.experiencecloud.adobe.com`
   * 次のソリューションをプロビジョニングされた Experience Cloud
      * [Adobe Target](https://experiencecloud.adobe.com)

     >[!NOTE]
     >
     > 顧客は、[アドビサポート](https://helpx.adobe.com/jp/contact/enterprise-support.ec.html)に依頼して Experience Platform Launch と Adobe I/O をプロビジョニングするか、システム管理者に連絡する必要があります。

### AEM と Adobe Target の統合

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. Adobe IMS 認証を使用した Adobe Target Cloud Service の作成（*Adobe Target API を使用*）（00:34）
2. Adobe Target クライアントコードの取得（01:50）
3. Adobe Target の Adobe IMS 設定の作成（02:08）
4. Adobe I/O コンソール内でターゲット API にアクセスするためのテクニカルアカウントの作成（02:08）
5. AEM エクスペリエンスフラグメントへの Adobe Target Cloud Service の追加（04:12）

この時点で、オプション 2 で説明した[従来のクラウドサービスを使用して AEM と Adobe Target](./using-aem-cloud-services.md#integrating-aem-target-options) を正常に統合しました。これで、AEM 内でエクスペリエンスフラグメントを作成し、そのエクスペリエンスフラグメントを HTML オファーまたは JSON オファーとして Adobe Target に公開したあと、アクティビティの作成に使用できるようになります。
