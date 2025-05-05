---
title: AEM Forms と Acrobat Sign を使用した React アプリ
description: Acrobat Signと AEM Forms では、複雑なトランザクションを自動化し、シームレスなデジタルエクスペリエンスの一環として法的な電子サインを含めることができます。
feature: Adaptive Forms,Acrobat Sign
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: 64172af3-2905-4bc8-8311-68c2a70fb39e
duration: 31
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '169'
ht-degree: 100%

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
* [Acrobat Sign アプリケーションの統合キー](https://helpx.adobe.com/jp/sign/kb/how-to-create-an-integration-key.html)

## 次の手順

ドキュメント化された API を使用した[インタラクティブ通信ドキュメントを生成するためのカスタム OSGi サービス](./create-ic-document.md)の記述
