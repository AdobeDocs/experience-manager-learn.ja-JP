---
title: AEM サービス資格情報
description: AEM Developer Console からサービス資格情報をダウンロードします。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-9980
exl-id: 4c5173f1-d57d-43ac-83e6-399ce4ead203
duration: 455
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '106'
ht-degree: 100%

---

# サービス資格情報

AEM as a Cloud Service との統合では、AEM に対して安全に認証できる必要があります。 AEM の Developer Consoleは、サービス資格情報を生成します。これは、外部のアプリケーション、システムおよびサービスによって、HTTP 経由で AEM オーサーまたはパブリッシュサービスとプログラム的にやり取りするために使用されます。

>[!VIDEO](https://video.tv.adobe.com/v/330519?quality=12&learn=on)

ダウンロードしたサービス資格情報ファイルは、指定した Eclipse の service_token.json というリソースファイルとして保存されます。 service_token ファイルの値は、JWT の生成と、JWT とアクセストークンの交換に使用されます。 ユーティリティクラス GetServiceCredentials は、 service_token.json リソースファイルからプロパティ値を取得するために使用されます。
