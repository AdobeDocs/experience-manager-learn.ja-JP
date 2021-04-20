---
title: Cloud Servicesを使用したAdobe Experience ManagerとAdobe Targetの統合
seo-title: レガシーCloud Servicesを使用したAdobe Experience Manager(AEM)とAdobe Targetの統合
description: AEMCloud Serviceを使用してAdobe Experience Manager(AEM)をAdobe Targetと統合する方法に関する手順説明を順を追って説明します。
seo-description: AEMCloud Serviceを使用してAdobe Experience Manager(AEM)をAdobe Targetと統合する方法に関する手順説明を順を追って説明します。
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 3%

---


# AEMレガシーCloud Servicesの使用

この節では、レガシーCloud Servicesを使用してAdobe Experience Manager(AEM)をAdobe Targetとセットアップする方法について説明します。

>[!NOTE]
>
> Adobe TargetとのAEMレガシーCloud Serviceは、AEMからターゲットへのコンテンツの公開を容易にするAEM AuthorからAdobe Targetへの直接接続を確立するために使用される&#x200B;**唯一**&#x200B;です。 Adobeの開始は、AEMが提供する公開されたWebサイトエクスペリエンスにAdobe Targetを公開するために使用されます。

AEMエクスペリエンスフラグメントオファーを使用してアクティビティのパーソナライズを強化するには、次の章に進み、レガシークラウドサービスを使用してAEMをAdobe Targetと統合します。 この統合は、エクスペリエンスフラグメントをAEMからHTML/JSONオファーとしてターゲットにプッシュし、ターゲットオファーをAEMと同期させるために必要です。 この統合は、[概要セクション](./overview.md#personalization-using-aem-experience-fragment)で説明されているシナリオ1の実装に必要です。

## 前提条件

* **AEM**

   * このチュートリアルを完了するには、AEMオーサーインスタンスとパブリッシュインスタンスが必要です。 AEMインスタンスをまだ設定していない場合は、[ここ](./implementation.md#set-up-aem)の手順に従ってください。

* **Experience Cloud**
   * 組織へのアクセス：Adobe Experience Cloud- <https://>`<yourcompany>`.experiencecloud.adobe.com
   * 次のソリューションでプロビジョニングされたExperience Cloud
      * [Adobe Target](https://experiencecloud.adobe.com)

      >[!NOTE]
      >
      > お客様は、[Adobeサポート](https://helpx.adobe.com/jp/contact/enterprise-support.ec.html)からExperience Platform LaunchとAdobe I/Oを利用してプロビジョニングするか、システム管理者に連絡する必要があります。



### AEMとAdobe Targetの統合

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. AdobeIMS認証を使用してAdobe TargetCloud Serviceを作成(*Adobe TargetAPI*&#x200B;を使用)(00:34)
2. Adobe Targetクライアントコードの取得(01:50)
3. Adobe Target用のAdobeIMS設定の作成(02:08)
4. Adobe I/Oコンソール内のターゲットAPIにアクセスするためのテクニカルアカウントを作成する(02:08)
5. Adobe Target追加Cloud ServiceとAEMエクスペリエンスフラグメント(04:12)

この時点で、Option 2で詳しく説明しているように、従来のCloud Services](./using-aem-cloud-services.md#integrating-aem-target-options)を使用して[AEMをAdobe Targetに正常に統合できました。 これで、AEM内でエクスペリエンスフラグメントを作成し、エクスペリエンスフラグメントをHTMLオファーまたはJSONオファーとしてAdobe Targetに発行し、アクティビティの作成に使用できるようになります。
