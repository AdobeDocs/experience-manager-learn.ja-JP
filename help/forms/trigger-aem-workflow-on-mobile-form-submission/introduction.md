---
title: HTM5 フォーム送信での AEM ワークフローのトリガー（概要）
description: モバイルフォーム送信での AEM ワークフローのトリガー
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2024-09-17T00:00:00Z
jira: kt-16215
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: ce51b25f-6069-4799-9a61-98c0a672e821
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 100%

---

# モバイルフォーム送信での AEM ワークフローのトリガー

一般的なユースケースは、データ取得アクティビティで XDP を HTML としてレンダリングすることです。このフォームの送信時に、AEM ワークフローのトリガーが必要になる場合があります。AEM ワークフローでは、データを XDP テンプレートと結合し、生成された PDF をレビューおよび承認の対象として表示できます。フォームは、パブリッシュインスタンスでレンダリングされ、ワークフローは AEM 処理インスタンスでトリガーされます。

ユースケースには、次の手順が含まれます

* ユーザーは HTML5 フォーム（XDP の HTML5 レンディション）に入力して送信します。
* 送信は、パブリッシュインスタンスのサーブレットで処理されます。
* サーブレットは、AEM 処理インスタンスのリポジトリの定義済みフォルダーにデータを保存します。
* ワークフローランチャーは、新しいファイルを特定のフォルダーに追加するたびに AEM ワークフローをトリガーするように設定されています。

このチュートリアルでは、上記のユースケースを達成するために必要な手順について説明します。 このチュートリアルに関連するサンプルコードとアセットは、[こちらから入手](./deploy-assets.md)できます。


## 次の手順

[フォームの送信処理](./handle-form-submission.md)
