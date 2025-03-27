---
title: SPA でのヘッドレスアダプティブフォームの使用
description: SPA でのヘッドレスお問い合わせフォームの実装
feature: Adaptive Forms
version: Experience Manager 6.5
jira: KT-13285
topic: Development
role: User
level: Intermediate
exl-id: 7b457ce8-f11a-4e2b-8548-6ac3910cb61e
duration: 24
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '124'
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
