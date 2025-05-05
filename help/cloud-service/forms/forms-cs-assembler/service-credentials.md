---
title: AEM サービス資格情報
description: AEM Developer Console からサービス資格情報をダウンロードします。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-9980
exl-id: 4c5173f1-d57d-43ac-83e6-399ce4ead203
duration: 453
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '106'
ht-degree: 100%

---

# サービス資格情報

AEM as a Cloud Service との統合では、AEM に対して安全に認証できる必要があります。 AEM の Developer Consoleは、サービス資格情報を生成します。これは、外部のアプリケーション、システムおよびサービスによって、HTTP 経由で AEM オーサーまたはパブリッシュサービスとプログラム的にやり取りするために使用されます。

>[!VIDEO](https://video.tv.adobe.com/v/342226?quality=12&learn=on&captions=jpn)

ダウンロードしたサービス資格情報ファイルは、指定した Eclipse の service_token.json というリソースファイルとして保存されます。 service_token ファイルの値は、JWT の生成と、JWT とアクセストークンの交換に使用されます。 ユーティリティクラス GetServiceCredentials は、 service_token.json リソースファイルからプロパティ値を取得するために使用されます。
