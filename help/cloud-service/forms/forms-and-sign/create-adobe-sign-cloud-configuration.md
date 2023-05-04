---
title: Acrobat Sign クラウド設定の作成クラウドサービス
description: クラウドサービス設定を使用して、AEM Forms と Acrobat Sign の統合を作成します。
solution: Experience Manager,Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 7428
thumbnail: 332437.jpg
exl-id: a55773a5-0486-413f-ada6-bb589315f0b1
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 39%

---

# Acrobat Sign クラウド設定の作成

AEM のクラウドサービス設定を使用すると、AEM と他のクラウドアプリケーションとの統合を作成できます。

次のビデオでは、AEM と Acrobat Sign を統合するためのクラウドサービス設定を作成するために必要な手順を説明します

>[!VIDEO](https://video.tv.adobe.com/v/332437?quality=12&learn=on)

## トラブルシューティング

Abobe Sign クラウドのクローン構成の設定でエラーが発生した場合は、次の手順を実行して撮影に問題が発生する可能性があります
* Acrobat Sign API アプリケーションで指定されたリダイレクト URL が次の形式であることを確認してください。
&lt;your instance=&quot;&quot; name=&quot;&quot;>/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/&lt;container>.
例： https://author-p24107-e32034.adobeaemcloud.com/libs/adobesign/cloudservices/adobesign/createcloudconfigwizard/cloudservices.html/conf/FormsCS FormsCS は、クラウド設定を保持するコンテナの名前です
* oAuth URL が正しいことを確認します。
* クライアント ID とクライアント秘密鍵の確認
* 匿名ウィンドウモードを試す

