---
title: AEM Forms サービス資格情報
description: AEM Developer Console からサービス資格情報をダウンロードします。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-8192
thumbnail: 330519.jpg
exl-id: 74cb8c30-4c41-426c-a1b5-fc595a3167c8
duration: 453
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '109'
ht-degree: 100%

---

# AEM Forms サービス資格情報

AEM as a Cloud Service との統合では、AEM に対して安全に認証できる必要があります。AEM の Developer Console は、サービス資格情報を生成します。この資格情報は、外部のアプリケーション、システム、サービスが、HTTP 経由で AEM のオーサーサービスまたはパブリッシュサービスとプログラム的にやり取りするために使用します。

>[!VIDEO](https://video.tv.adobe.com/v/342226?quality=12&learn=on&captions=jpn)

ダウンロードしたサービス資格情報ファイルは、指定した Eclipse の service_token.json というリソースファイルとして保存されます。 service_token ファイルの値は、JWT の生成と、JWT とアクセストークンの交換に使用されます。 ユーティリティクラス GetServiceCredentials は、 service_token.json リソースファイルからプロパティ値を取得するために使用されます。
