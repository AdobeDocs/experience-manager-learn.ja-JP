---
title: AEM Service Credentials
description: AEM Developer Console からサービス資格情報をダウンロードします。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 9980
exl-id: 4c5173f1-d57d-43ac-83e6-399ce4ead203
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '109'
ht-degree: 0%

---

# サービス資格情報

AEM as a Cloud Serviceとの統合では、AEMに対して安全に認証できる必要があります。 AEM開発者コンソールは、サービス資格情報を生成します。これは、外部のアプリケーション、システムおよびサービスによって、HTTP 経由で AEM オーサーまたはパブリッシュサービスとプログラム的にやり取りするために使用されます。

>[!VIDEO](https://video.tv.adobe.com/v/330519?quality=12&learn=on)

ダウンロードしたサービス資格情報ファイルは、指定した Eclipse の service_token.json というリソースファイルとして保存されます。 service_token ファイルの値は、JWT の生成と、JWT とアクセストークンの交換に使用されます。 ユーティリティクラス GetServiceCredentials は、 service_token.json リソースファイルからプロパティ値を取得するために使用されます。
