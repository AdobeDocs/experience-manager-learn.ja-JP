---
title: SPAでのヘッドレスアダプティブフォームの使用
description: SPAにヘッドレス連絡フォームを実装する
feature: Adaptive Forms
version: 6.5
kt: 13285
topic: Development
role: User
level: Intermediate
source-git-commit: c6e83a627743c40355559d9cdbca2b70db7f23ed
workflow-type: tm+mt
source-wordcount: '133'
ht-degree: 2%

---


# ヘッドレスアダプティブフォームの埋め込み

この記事では、SPA Web サイトへのヘッドレスアダプティブフォームの埋め込みの基本について説明します。 この記事では、既存の単一ページアプリがあり、コアコンポーネントを使用してAEM Forms 6.5.16 で作成したアダプティブフォームを埋め込むことを前提としています。
単一ページアプリにフォームを含めると、ページを更新しなくても、ユーザーはデータをシームレスに入力および送信できます。 これにより、アプリケーションのインタラクティビティと効率が向上します。

次のスクリーンショットは、SPAに埋め込まれた連絡先 US フォームを示しています

![contact-us-form](./assets/contact-us-form.png)

## 前提条件

* React エクスペリエンス

* AEM Forms 6.5.16 の実行中のインスタンス

* [オーサーインスタンスとパブリッシュインスタンスでヘッドレスフォームを有効にする](https://experienceleague.adobe.com/docs/experience-manager-headless-adaptive-forms/using/quick-setup/enable-headless-adaptive-forms-and-core-components.html?lang=en)

## 次の手順

[依存関係をインストール](./install-af-react-libraries.md)

