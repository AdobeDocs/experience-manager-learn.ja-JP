---
title: Cloud Servicesを使用したAdobe Experience ManagerとAdobe Targetの統合
seo-title: Integrating Adobe Experience Manager (AEM) with Adobe Target using Legacy Cloud Services
description: AEM Cloud Serviceを使用したAdobe Experience Manager(AEM) とAdobe Targetの統合方法に関する詳しい手順
seo-description: Step by step walkthrough on how to integrate Adobe Experience Manager (AEM) with Adobe Target using AEM Cloud Service
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
exl-id: 9b191211-2030-4b62-acad-c7eb45b807ca
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '354'
ht-degree: 14%

---

# AEMレガシーCloud Servicesの使用

この節では、レガシーCloud Servicesを使用してAdobe Experience Manager(AEM) をAdobe Targetと設定する方法について説明します。

>[!NOTE]
>
> Adobe TargetとのAEMレガシーCloud Serviceは、 **のみ** AEMから Target へのコンテンツの公開を容易にする、AEM オーサーとAdobe Targetのバックエンド接続を直接確立するために使用します。 AdobeLaunch は、AEMが提供する、公開されている Web サイトエクスペリエンス上でAdobe Targetを公開するために使用します。

AEM Experience Fragment オファーを使用してパーソナライゼーションアクティビティを強化するには、次の章に進み、従来のクラウドサービスを使用してAEMとAdobe Targetを統合します。 この統合は、エクスペリエンスフラグメントをHTML/JSON オファーとしてAEMから Target にプッシュし、Target オファーとAEMの同期を維持するために必要です。 この統合は、 [シナリオ 1：概要の節で説明](./overview.md#personalization-using-aem-experience-fragment).

## 前提条件

* **AEM**

   * このチュートリアルを完了するには、AEMオーサーインスタンスとパブリッシュインスタンスが必要です。 AEMインスタンスをまだ設定していない場合は、次の手順に従います [ここ](./implementation.md#set-up-aem).

* **Experience Cloud**
   * 組織の Adobe Experience Cloud にアクセス - `https://<yourcompany>.experiencecloud.adobe.com`
   * 次のソリューションをプロビジョニングされた Experience Cloud
      * [Adobe Target](https://experiencecloud.adobe.com)

     >[!NOTE]
     >
     > 顧客は、[アドビサポート](https://helpx.adobe.com/jp/contact/enterprise-support.ec.html)から Experience Platform Launch と Adobe I/O をプロビジョニングするか、システム管理者に連絡する必要があります。

### AEMとAdobe Targetの統合

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. Adobe IMS認証 (*Adobe Target API を使用*) (00:34)
2. Adobe Target Client Code (01:50)の取得
3. Adobe Target (02:08)のAdobe IMS設定の作成
4. Adobe I/Oコンソール内で Target API にアクセスするためのテクニカルアカウントを作成(02:08)ます
5. Adobe TargetCloud ServiceをAEM Experience Fragments に追加(04:12)

この時点で、正常に統合されました [レガシーCloud Servicesを使用したAdobe TargetとのAEM](./using-aem-cloud-services.md#integrating-aem-target-options) オプション 2 で詳しく説明されているように。 これで、AEM内でエクスペリエンスフラグメントを作成し、エクスペリエンスフラグメントをHTMLオファーまたは JSON オファーとしてAdobe Targetに公開できるようになり、これを使用してアクティビティを作成できます。
