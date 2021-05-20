---
title: Cloud Servicesを使用したAdobe Experience ManagerとAdobe Targetの統合
seo-title: レガシーCloud Servicesを使用したAdobe Experience Manager(AEM)とAdobe Targetの統合
description: AEMCloud Serviceを使用したAdobe Experience Manager(AEM)とAdobe Targetの統合方法に関する詳しい手順
seo-description: AEMCloud Serviceを使用したAdobe Experience Manager(AEM)とAdobe Targetの統合方法に関する詳しい手順
feature: エクスペリエンスフラグメント
topic: パーソナライズ機能
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 3%

---


# AEMレガシーCloud Servicesの使用

この節では、レガシーCloud Servicesを使用してAdobe Experience Manager(AEM)をAdobe Targetと設定する方法について説明します。

>[!NOTE]
>
> Adobe TargetとのAEMレガシーCloud Serviceは、AEMからTargetへのコンテンツの公開を容易にする、AEMオーサーとAdobe Targetのバックエンド接続を直接確立するために&#x200B;**のみ**&#x200B;使用されます。 AdobeLaunchは、AEMが提供する、公開されているWebサイトエクスペリエンス上でAdobe Targetを公開するために使用されます。

AEMエクスペリエンスフラグメントオファーを使用してパーソナライゼーションアクティビティを強化するには、次の章に進み、従来のクラウドサービスを使用してAEMとAdobe Targetを統合します。 この統合は、エクスペリエンスフラグメントをHTML/JSONオファーとしてAEMからTargetにプッシュし、TargetオファーをAEMと同期させるために必要です。 この統合は、概要の節](./overview.md#personalization-using-aem-experience-fragment)で説明した[シナリオ1を実装するために必要です。

## 前提条件

* **AEM**

   * このチュートリアルを完了するには、AEMオーサーインスタンスとパブリッシュインスタンスが必要です。 AEMインスタンスをまだ設定していない場合は、[ここ](./implementation.md#set-up-aem)の手順に従います。

* **Experience Cloud**
   * 組織へのアクセスAdobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * 次のソリューションでプロビジョニングされたExperience Cloud
      * [Adobe Target](https://experiencecloud.adobe.com)

      >[!NOTE]
      >
      > お客様は、[Adobeサポート](https://helpx.adobe.com/jp/contact/enterprise-support.ec.html)からExperience Platform LaunchとAdobe I/Oをプロビジョニングするか、システム管理者に問い合わせる必要があります



### AEMとAdobe Targetの統合

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. AdobeIMSCloud Service(*Adobe Target API*&#x200B;を使用)を使用したAdobe Target認証の作成(00:34)
2. Adobe Target Client Code (01:50)の取得
3. Adobe Target (02:08)のAdobeIMS設定の作成
4. Targetコンソール内でTarget APIにアクセスするためのテクニカルAdobe I/Oを作成する(02:08)
5. Adobe TargetCloud ServiceをAEM Experience Fragmentsに追加(04:12)

この時点で、オプション2で説明されているように、従来のCloud Services](./using-aem-cloud-services.md#integrating-aem-target-options)を使用して、Adobe Targetと[AEMを正常に統合できました。 これで、AEM内でエクスペリエンスフラグメントを作成し、エクスペリエンスフラグメントをHTMLオファーまたはJSONオファーとしてAdobe Targetに公開できるようになり、これを使用してアクティビティを作成できます。
