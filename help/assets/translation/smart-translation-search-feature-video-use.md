---
title: AEM Assetsでのスマート翻訳検索の使用
description: スマート翻訳検索を使用すると、アセットとページの両方のAEMコンテンツをまたいで、言語をまたいだ検索と検出を自動的に実行でき、50 以上の言語をサポートし、手動でのコンテンツ翻訳の必要性を減らすことができます。
version: 6.4, 6.5
feature: Search
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-09-03T00:00:00Z
thumbnail: 21297.jpg
exl-id: 4f35e3f7-ae29-4f93-bba9-48c60b800238
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 0%

---

# AEM Assetsでのスマート翻訳検索の使用{#using-smart-translation-search-with-aem-assets}

スマート翻訳検索を使用すると、アセットとページの両方のAEMコンテンツをまたいで、言語をまたいだ検索と検出を自動的に実行でき、50 以上の言語をサポートし、手動でのコンテンツ翻訳の必要性を減らすことができます。

>[!VIDEO](https://video.tv.adobe.com/v/21297?quality=12&learn=on)

AEM Smart Translation Search を使用すると、英語以外の用語を使用してAEMでコンテンツの検索を実行し、同等の英語の用語を持つAEM内のアセットを照合できます。

スマート翻訳検索は、AEMのスマートタグを英語で使用したアセットに適用する完璧な補完機能です。

このビデオでは、 [AEM Smart Translation Search](smart-translation-search-technical-video-setup.md) が設定されている。

## スマート翻訳検索の仕組み {#how-smart-translation-search-works}

![スマート翻訳検索のフロー図](assets/smart-translation-search-flow.png)

1. AEMユーザーが全文検索を実行し、ローカライズされた検索語句 ( 例： スペイン語で&#39;man&#39;, &#39;hombre&#39;を指す )。
2. Apache Oak Machine Translation OSGi バンドルが提供するスマート翻訳検索が関与し、提供された検索用語が登録済みの言語パックを使用して翻訳できるかどうかを評価します。
3. ステップ#2の翻訳済みの用語がすべて収集され、内部的にクエリが拡張され、検索用語として含められます。 関連する一致を見つけるAEM検索インデックスに対して正常に評価される場合、この拡張検索用語のセット。
4. 元の用語 (「hombre」) または翻訳された用語 (「man」) に一致する検索結果が収集され、検索結果としてユーザに返されます。

## その他のリソース{#additional-resources}

* [AEM Assetsでのスマート翻訳検索の設定](smart-translation-search-technical-video-setup.md)
* [Apache Joshua 言語パック](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
