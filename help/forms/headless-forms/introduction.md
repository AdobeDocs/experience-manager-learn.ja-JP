---
title: SPAでのヘッドレスアダプティブフォームの使用
description: SPAにヘッドレス連絡フォームを実装する
feature: Adaptive Forms
version: 6.5
kt: 13285
topic: Development
role: User
level: Intermediate
source-git-commit: 6aa3dff44a7e6f1f8ac896e30319958d84ecf57f
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 2%

---


# ヘッドレスアダプティブフォームの埋め込み

この [このチュートリアルでは、様々なヘッドレス API について説明します](https://opensource.adobe.com/aem-forms-af-runtime/api/#section/Introduction) フォームのリスト表示、送信を行うための

この記事では、アダプティブフォームをヘッドレスで一覧表示、表示、送信するために提供される様々なヘッドレス API について説明します。

この記事では、既存の単一ページアプリを使用し、SPA Web サイトにヘッドレスアダプティブフォームのリストを作成して表示することを前提としています。

次のスクリーンショットは、SPAに埋め込まれた連絡先 US フォームを示しています

![contact-us-form](./assets/contact-us-form.png)

## 前提条件

* React エクスペリエンス

* AEM Forms 6.5.16 の実行中のインスタンス

* [オーサーインスタンスとパブリッシュインスタンスでヘッドレスフォームを有効にする](https://experienceleague.adobe.com/docs/experience-manager-headless-adaptive-forms/using/quick-setup/enable-headless-adaptive-forms-and-core-components.html?lang=en)

## 次の手順

[依存関係をインストール](./install-af-react-libraries.md)

