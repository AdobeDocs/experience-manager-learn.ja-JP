---
title: Acrobat Sign での AEM Forms の使用
description: Acrobat Signと AEM Forms では、複雑なトランザクションを自動化し、シームレスなデジタルエクスペリエンスの一環として法的な電子サインを含めることができます。
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 0be61f04-c3ed-4ecb-bab7-73fd308c14e0
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: 81b96f59450448a3d5b17a61aa025acd60d0cce1
workflow-type: ht
source-wordcount: '158'
ht-degree: 100%

---

# Acrobat Sign での AEM Forms の使用

Acrobat Sign により、アダプティブフォームの電子サインワークフローを有効にできます。電子サインを使用すると、法務、販売、給与、人事管理など、様々な分野におけるドキュメント処理ワークフローが改善されます。AEM Forms と Acrobat Sign の統合により、次の操作が可能になります。

* アダプティブフォームを使用してデータをキャプチャし、署名用に自動生成されたレコードのドキュメント（DoR）を表示する
* PDF テンプレートを基にしたアダプティブフォームの作成。データを PDF テンプレートと結合し、署名用に同じものを表示
* ドキュメントに署名ワークフローコンポーネントを使用して、署名用のドキュメントを送信

## 前提条件

Acrobat Sign を AEM Forms に統合するには、以下のものが必要です。

* SSL が有効になっている AEM Forms サーバー
* アクティブな Acrobat Sign 開発者アカウント。
* Acrobat Sign API アプリケーション
* Acrobat Sign API アプリケーションの資格情報（クライアントの ID とクライアントシークレット）
