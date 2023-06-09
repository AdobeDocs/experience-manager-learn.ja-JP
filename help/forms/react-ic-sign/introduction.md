---
title: AEM Forms と Acrobat Sign を使用した React アプリ
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
ht-degree: 92%

---

# Acrobat Sign Web フォームを使用した AEM Forms


このチュートリアルでは、[React](https://react.dev/) アプリから送信されたデータを使用してインタラクティブなコミュニケーションドキュメントを生成し、Acrobat Sign web フォームでの署名用に生成されたドキュメントを紹介する使用事例について説明します。

以下は、使用事例のフローです。

* ユーザーが React アプリのフォームに入力します。
* フォームデータは AEM Forms エンドポイントに送信され、インタラクティブ通信ドキュメントが生成されます。
* 生成されたドキュメントを使用して、Acrobat Sign ウィジェットの URL を作成します。
* ユーザーがドキュメントに署名できるように、呼び出し元のアプリケーションに対してウィジェットの URL を提示します。

## 前提条件

この使用事例が機能するには、以下が必要です。

* Forms アドオンパッケージを含む AEM サーバー
* [Acrobat Sign アプリケーションの統合キー](https://helpx.adobe.com/sign/kb/how-to-create-an-integration-key.html)

## 次の手順

を書く [インタラクティブ通信ドキュメントを生成するためのカスタム OSGi サービス](./create-ic-document.md) ドキュメント化された API の使用
