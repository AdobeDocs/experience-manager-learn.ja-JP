---
title: SPA でのヘッドレスアダプティブフォームの使用
description: SPA でのヘッドレスお問い合わせフォームの実装
feature: Adaptive Forms
version: 6.5
kt: 13285
topic: Development
role: User
level: Intermediate
exl-id: a295e04a-d56b-4226-ab0d-b85728b5c1fe
source-git-commit: 0a8b60cb69f3f185375d34c8cb9ab90bc84c85cd
workflow-type: ht
source-wordcount: '135'
ht-degree: 100%

---

# ヘッドレスアダプティブフォームの埋め込み

この[チュートリアルでは、フォームのリスト、表示、送信を可能にする様々なヘッドレス API](https://opensource.adobe.com/aem-forms-af-runtime/api/#section/Introduction) について説明します。

この記事では、ヘッドレス方式でアダプティブフォームをリスト、表示、送信できるようにするために提供されている様々なヘッドレス API について説明します。

この記事では、既存の単一ページアプリを使用し、SPA web サイトでヘッドレスアダプティブフォームをリストして表示することを前提としています。

次のスクリーンショットは、SPA に埋め込まれたお問い合わせフォームを示しています。

![contact-us-form](./assets/contact-us-form.png)

## 前提条件

* React エクスペリエンス

* AEM Forms 6.5.16 の実行インスタンス

* [オーサーインスタンスとパブリッシュインスタンスでヘッドレスフォームを有効にする](https://experienceleague.adobe.com/docs/experience-manager-headless-adaptive-forms/using/quick-setup/enable-headless-adaptive-forms-and-core-components.html?lang=ja)

## 次の手順

[依存関係のインストール](./install-af-react-libraries.md)
