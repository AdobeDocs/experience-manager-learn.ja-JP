---
title: AEM FormsとAcrobat Signの統合
description: Acrobat SignとAEM Formsを統合して、複雑なトランザクションを自動化し、シームレスなデジタルエクスペリエンスの一環として法的な電子署名を含めます。
feature: Adaptive Forms, Acrobat Sign
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Beginner
exl-id: 0be61f04-c3ed-4ecb-bab7-73fd308c14e0
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
last-substantial-update: 2020-03-20T00:00:00Z
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 63%

---

# AEM FormsとAcrobat Signの統合

AEM forms をAcrobat Signと統合し、アダプティブフォームの電子署名ワークフローを有効にする方法について説明します。 電子サインを使用すると、法務、販売、給与、人事管理など、様々な分野におけるドキュメント処理ワークフローが改善されます。

AEM FormsとAcrobat Signの統合により、次のことが可能になります。

* アダプティブフォームを使用してデータをキャプチャし、署名用に自動生成されたレコードのドキュメント（DoR）を表示する
* PDF テンプレートを基にしたアダプティブフォームの作成。データを PDF テンプレートと結合し、署名用に同じものを表示
* ドキュメントに署名ワークフローコンポーネントを使用して、署名用のドキュメントを送信

## 前提条件

Acrobat Sign を AEM Forms に統合するには、以下のものが必要です。

* SSL が有効になっている AEM Forms サーバー
* アクティブな Acrobat Sign 開発者アカウント。
* Acrobat Sign API アプリケーション
* Acrobat Sign API アプリケーションの資格情報（クライアントの ID とクライアントシークレット）

## 次の手順

[SSL を設定](./set-up-ssl.md)
