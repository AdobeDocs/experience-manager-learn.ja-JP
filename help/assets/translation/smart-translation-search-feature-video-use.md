---
title: AEM Assetsでのスマート翻訳検索の使用
description: スマート翻訳検索を使用すると、アセットとページの両方のAEMコンテンツで、言語をまたいだ検索と検出を自動的に実行でき、50以上の言語をサポートし、手動でのコンテンツ翻訳の必要性を軽減できます。
version: 6.3, 6.4, 6.5
feature: 検索
topic: コンテンツ管理
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 1%

---


# AEM Assets{#using-smart-translation-search-with-aem-assets}でのスマート翻訳検索の使用

スマート翻訳検索を使用すると、アセットとページの両方のAEMコンテンツで、言語をまたいだ検索と検出を自動的に実行でき、50以上の言語をサポートし、手動でのコンテンツ翻訳の必要性を軽減できます。

>[!VIDEO](https://video.tv.adobe.com/v/21297/?quality=9&learn=on)

AEM Smart Translation Searchを使用すると、英語以外の用語を使用してAEMでコンテンツの検索を実行し、同等の英語の用語を持つAEMのアセットを照合できます。

スマート翻訳検索は、英語でアセットに適用されるAEMスマートタグにとって完全な補完です。

このビデオでは、[AEM Smart Translation Search](smart-translation-search-technical-video-setup.md)が設定されていることを前提としています。

## スマート翻訳検索の仕組み{#how-smart-translation-search-works}

![スマート翻訳検索のフロー図](assets/smart-translation-search-flow.png)

1. AEMユーザーが全文検索を実行し、ローカライズされた検索語句(例： スペイン語で&#39;man&#39;、&#39;hombre&#39;を指す)。
2. Apache Oak Machine Translation OSGiバンドルが提供するスマート翻訳検索が関与し、提供された検索用語が登録済みの言語パックを使用して翻訳できるかどうかを評価します。
3. 手順#2の翻訳済みの用語はすべて収集され、内部的にクエリが拡張され、検索用語として含められます。 関連する一致を見つけるAEM検索インデックスに対して正常に評価される場合、この拡張検索用語セット。
4. 元の用語(「hombre」)または翻訳された用語(「man」)に一致する検索結果が収集され、検索結果としてユーザに返されます。

## その他のリソース{#additional-resources}

* [AEM Assetsでのスマート翻訳検索の設定](smart-translation-search-technical-video-setup.md)
* [Apache Joshua言語パック](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)