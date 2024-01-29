---
title: AEM Forms CS でバッチ API を使用したドキュメント生成
description: バッチ操作を設定しトリガーを設定してドキュメントを生成します。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
exl-id: 165e2884-4399-4970-81ff-1f2f8b041a10
duration: 32
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '134'
ht-degree: 100%

---

# はじめに

バッチリクエストでは、1 回に何十、何百、何千もの類似ドキュメントが生成されます。 例：金融会社は、すべての顧客に送信するクレジットカード明細書を生成できます。
バッチ API（非同期 API）は、スケジュールに沿った高スループットの複数ドキュメント生成のユースケースに適しています。これらの API は、バッチでドキュメントを生成します。例えば、毎月生成される電話料金、クレジットカード明細、給付計算書などです。

AEM Forms CS バッチ操作 API を使用するには、次の設定が必要です。

1. Azure ストレージアカウントを設定します。
1. Azure ストレージに基づいたクラウド設定を作成します。
1. バッチデータストア設定を作成します。
1. バッチ API を実行します。

このチュートリアルの利用を開始する前に、[API ドキュメント](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/batch-api.yaml?lang=ja)を熟知しておくことをお勧めします。
