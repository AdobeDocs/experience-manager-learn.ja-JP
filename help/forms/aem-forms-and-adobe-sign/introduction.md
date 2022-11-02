---
title: Acrobat SignでのAEM Formsの使用
description: Acrobat SignとAEM Formsでは、複雑なトランザクションを自動化し、シームレスなデジタルエクスペリエンスの一環として法的な電子サインを含めることができます。
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 0be61f04-c3ed-4ecb-bab7-73fd308c14e0
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 81b96f59450448a3d5b17a61aa025acd60d0cce1
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 11%

---

# Acrobat SignでのAEM Formsの使用

Acrobat Signを使用すると、アダプティブフォームの電子署名ワークフローが有効になります。 電子サインを使用すると、法務、販売、給与、人事管理など、様々な分野におけるドキュメント処理ワークフローが改善されます。AEM FormsとAcrobat Signの統合により、次の操作が可能になります

* アダプティブFormsを使用してデータをキャプチャし、署名用に自動生成されたレコードのドキュメント (DoR) を表示する
* アダプティブFormsを作成テンプレートに基づいてPDFします。 データを pdf テンプレートと結合し、署名に同じものを使用する
* ドキュメントに署名ワークフローコンポーネントを使用して署名用のドキュメントを送信する

## 前提条件

Acrobat SignをAEM Formsと統合するには、以下が必要です。

* SSL が有効なAEM Formsサーバー
* アクティブなAcrobat Sign開発者アカウント。
* Acrobat Sign API アプリケーション
* Acrobat Sign API アプリケーションの資格情報（クライアント ID およびクライアントの秘密鍵）。
