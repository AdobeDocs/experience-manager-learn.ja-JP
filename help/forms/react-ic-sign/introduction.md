---
title: AEM FormsおよびAcrobat Signを使用した React アプリ
description: Acrobat Signと AEM Forms では、複雑なトランザクションを自動化し、シームレスなデジタルエクスペリエンスの一環として法的な電子サインを含めることができます。
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: 2ff7be5b-884c-420d-9a06-f0e2a99d3ef3
source-git-commit: 4709035983a5c6705c4e807d877ee71145f48987
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 13%

---

# AEM FormsとAcrobat Sign Web フォーム


このチュートリアルでは、 [React](https://react.dev/) アプリを作成し、Acrobat Sign Web フォームを使用して署名用に生成されたドキュメントを表示します。

次に、使用例の流れを示します

* ユーザーが React アプリのフォームに入力します。
* フォームデータは、インタラクティブ通信ドキュメントを生成するためにAEM Formsエンドポイントに送信されます。
* 生成されたドキュメントを使用して、Acrobat Signウィジェットの URL を作成します。
* ユーザーがドキュメントに署名するための呼び出し元のアプリケーションへのウィジェット URL を表示します。

## 前提条件

このユースケースが機能するには、以下が必要になります。

* Formsアドオンパッケージを含むAEMサーバー
* An [Acrobat Signアプリケーションの統合キー](https://helpx.adobe.com/sign/kb/how-to-create-an-integration-key.html)

## 次の手順

を書く [インタラクティブ通信ドキュメントを生成するためのカスタム OSGi サービス](./create-ic-document.md) ドキュメント化された API の使用
