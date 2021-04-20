---
title: AEM FormsとAdobe Signの併用
description: Adobe SignとAEM Formsでは、複雑なトランザクションを自動化し、シームレスなデジタルエクスペリエンスの一環として法的な電子署名を含めることができます。
feature: アダプティブForms、Adobe Sign
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
topic: 開発
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '163'
ht-degree: 36%

---

# AEM FormsとAdobe Signの併用

Adobe Sign により、アダプティブフォームの電子署名ワークフローを有効にすることができます。電子署名を使用すると、法務、販売、給与、人事管理など、さまざまな分野におけるドキュメント処理ワークフローが改善されます。AEM FormsとAdobe Signの統合により、以下のことが可能になります。

* アダプティブFormsを使用して、データを取り込み、署名用にレコード(DoR)の自動生成ドキュメントを表示します。
* PDFテンプレートに基づいてアダプティブFormsを作成します。 データとPDFテンプレートをマージし、同じデータを署名に表示する
* 署名ドキュメントワークフローコンポーネントを使用して署名用のドキュメントを送信する

## 前提条件

Adobe Sign を AEM Forms に統合するには、以下のものが必要になります。

* SSL対応のAEM Formsサーバー
* 有効な Adobe Sign 開発者アカウント
* Adobe Sign API アプリケーション
* Adobe Sign API アプリケーションの資格情報（クライアントの ID と秘密鍵）

