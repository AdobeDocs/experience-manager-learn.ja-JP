---
title: Acrobat Sign クラウド設定の作成クラウドサービス
description: クラウドサービス設定を使用して、AEM Forms と Acrobat Sign の統合を作成します。
solution: Experience Manager,Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-7428
thumbnail: 332437.jpg
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: a55773a5-0486-413f-ada6-bb589315f0b1
duration: 222
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '155'
ht-degree: 100%

---

# Acrobat Sign クラウド設定の作成

AEM のクラウドサービス設定を使用すると、AEM と他のクラウドアプリケーションとの統合を作成できます。

次のビデオでは、AEM と Acrobat Sign を統合するためのクラウドサービス設定を作成するために必要な手順を説明します

>[!VIDEO](https://video.tv.adobe.com/v/332437?quality=12&learn=on)

## トラブルシューティング

Abobe Sign クラウドのクローン設定でエラーが発生した場合は、次の手順を実行してトラブルシューティングを行うことができます
* Acrobat Sign API アプリケーションで指定されたリダイレクト URL が次の形式であることを確認してください。
&lt;your instance name>/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/&lt;container>
例：https://author-p24107-e32034.adobeaemcloud.com/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/FormsCSFormsCS は、クラウド設定を保持するコンテナの名前です
* oAuth URL が正しいことを確認します
* クライアント ID とクライアント秘密鍵の確認
* 匿名ウィンドウモードを試す

