---
title: Adobe SignでのAEM Formsの使用
description: Adobe SignとAEM Formsでは、複雑なトランザクションを自動化し、シームレスなデジタルエクスペリエンスの一環として法的な電子サインを含めることができます。
feature: Adaptive Forms,Adobe Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 0be61f04-c3ed-4ecb-bab7-73fd308c14e0
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 37%

---

# Adobe SignでのAEM Formsの使用

Adobe Sign により、アダプティブフォームの電子署名ワークフローを有効にすることができます。電子サインを使用すると、法務、販売、給与、人事管理など、様々な分野におけるドキュメント処理ワークフローが改善されます。AEM FormsとAdobe Signの統合により、次の操作が可能になります

* アダプティブFormsを使用してデータをキャプチャし、署名用に自動生成されたレコードのドキュメント (DoR) を表示する
* アダプティブFormsを作成テンプレートに基づいてPDFします。 データを pdf テンプレートと結合し、署名に同じものを使用する
* ドキュメントに署名ワークフローコンポーネントを使用して署名用のドキュメントを送信する

## 前提条件

Adobe Sign を AEM Forms に統合するには、以下のものが必要になります。

* SSL が有効なAEM Formsサーバー
* 有効な Adobe Sign 開発者アカウント
* Adobe Sign API アプリケーション
* Adobe Sign API アプリケーションの資格情報（クライアントの ID と秘密鍵）
