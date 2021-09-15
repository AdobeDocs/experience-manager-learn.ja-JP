---
title: AEM Service Credentials
description: AEM Developer Consoleからサービス資格情報をダウンロードします。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 8192
thumbnail: 330519.jpg
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '109'
ht-degree: 0%

---


# サービス資格情報

AEM as a AEMとの統合では、Cloud Serviceに対して安全に認証できる必要があります。 AEM開発者コンソールは、サービス資格情報を生成します。この資格情報は、外部のアプリケーション、システム、サービスによって、HTTP経由でAEMオーサーまたはパブリッシュサービスとプログラム的にやり取りするために使用されます。

>[!VIDEO](https://video.tv.adobe.com/v/330519/?quality=12&learn=on)

ダウンロードしたサービス資格情報ファイルは、指定したeclipse内にservice_token.jsonというリソースファイルとして保存されます。 service_tokenファイルの値を使用してJWTを生成し、JWTをアクセストークンと交換します。 ユーティリティクラスGetServiceCredentialsを使用して、 service_token.jsonリソースファイルからプロパティ値を取得します。
