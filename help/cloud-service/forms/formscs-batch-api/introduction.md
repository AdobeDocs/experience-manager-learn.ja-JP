---
title: AEM Forms CS でのバッチ API を使用したドキュメント生成
description: バッチ操作を設定し、トリガーを設定してドキュメントを生成します。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
exl-id: 165e2884-4399-4970-81ff-1f2f8b041a10
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '141'
ht-degree: 24%

---

# はじめに

バッチリクエストでは、1 回に数十、数百、数千の類似ドキュメントが生成されます。 例：金融会社は、すべての顧客に送信するクレジットカード明細書を生成できます。
バッチ API（非同期 API）は、スケジュールに沿った高スループットの複数ドキュメント生成のユースケースに適しています。これらの API は、バッチでドキュメントを生成します。例えば、毎月生成される電話料金、クレジットカード明細、給付計算書などです。

AEM Forms CS バッチ操作 API を使用するには、次の設定が必要です

1. Azure ストレージアカウントの設定
1. Azure ストレージに基づいたクラウド設定の作成
1. バッチデータストア設定を作成
1. バッチ API の実行

詳しくは、 [API ドキュメント](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/batch-api.yaml?lang=en) このチュートリアルを使用する前に、を参照してください。
