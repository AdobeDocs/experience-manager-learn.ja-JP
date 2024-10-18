---
title: HTML 5 フォーム送信のトリガー AEM ワークフローの概要
description: モバイルフォーム送信時のAEM ワークフローのトリガー
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2024-09-17T00:00:00Z
jira: kt-16215
badgeVersions: label="AEM Forms 6.5" before-title="false"
source-git-commit: c6ffa8f7a398b01fc12e1e2efe4382c941900496
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 16%

---

# モバイルフォーム送信時のAEM ワークフローのトリガー

一般的なユースケースは、データキャプチャアクティビティのHTMLとして XDP をレンダリングすることです。 このフォームの送信時に、AEM ワークフローのトリガーが必要になる場合があります。 AEM ワークフローでは、データを XDP テンプレートと結合し、生成されたPDFをレビューと承認のために表示することができます。 フォームはパブリッシュインスタンスでレンダリングされ、ワークフローはAEM処理インスタンスでトリガーされます。

このユースケースには、次の手順が含まれます

* ユーザーはHTML5 フォーム（XDP のHTML5 レンディション）に入力して送信します。
* 送信は、パブリッシュインスタンスのサーブレットで処理されます。
* サーブレットは、AEM処理インスタンスのリポジトリ内の事前定義済みフォルダーにデータを格納します。
* ワークフローランチャーは、新しいファイルが特定のフォルダーに追加されるたびにAEM ワークフローをトリガーするように設定されています。

このチュートリアルでは、上記のユースケースを達成するために必要な手順について説明します。 このチュートリアルに関連するサンプルコードとアセットは、[こちらから入手](./deploy-assets.md)できます。


## 次の手順

[フォームの送信処理](./handle-form-submission.md)
