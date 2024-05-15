---
title: AEM Assets でのスマート翻訳検索の使用
description: スマート翻訳検索を使用すると、あらゆる AEM コンテンツ（アセットとページの両方）を対象にクロス言語検索および検出を自動的に実行できます。50 以上の言語をサポートしており、手動コンテンツ翻訳の必要性を減らすことができます。
version: 6.4, 6.5
feature: Search
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-09-03T00:00:00Z
thumbnail: 21297.jpg
doc-type: Feature Video
exl-id: 4f35e3f7-ae29-4f93-bba9-48c60b800238
duration: 189
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 100%

---

# AEM Assets でのスマート翻訳検索の使用{#using-smart-translation-search-with-aem-assets}

スマート翻訳検索を使用すると、あらゆる AEM コンテンツ（アセットとページの両方）を対象にクロス言語検索および検出を自動的に実行できます。50 以上の言語をサポートしており、手動コンテンツ翻訳の必要性を減らすことができます。

>[!VIDEO](https://video.tv.adobe.com/v/21297?quality=12&learn=on)

AEM スマート翻訳検索を使用すると、英語以外の用語を使用して AEM のコンテンツの検索を実行し、それに関する同等の英語用語を持っている AEM 内のアセットを照合できます。

スマート翻訳検索は、英語のアセットに適用される AEM スマートタグを完璧に補完する機能です。

このビデオでは、 [AEM スマート翻訳検索](smart-translation-search-technical-video-setup.md) が設定されていることを前提としています。

## スマート翻訳検索の仕組み {#how-smart-translation-search-works}

![スマート翻訳検索のフロー図](assets/smart-translation-search-flow.png)

1. AEM ユーザーが全文検索を実行し、ローカライズされた検索語句（例： スペイン語で「man」を意味する「hombre」）を指定します。
2. Apache Oak Machine Translation OSGi バンドルが提供するスマート翻訳検索を使用し、指定された検索用語を登録済みの言語パックを使用して翻訳できるかどうかを評価します。
3. 手順 2 の翻訳済み用語がすべて収集されて、それらを検索用語として含めるようにクエリが内部的に拡張されます。この拡張された検索用語セットを通常は AEM の検索インデックスに照らして評価し、関連性の高い一致を見つけます。
4. 元の用語（「hombre」）または翻訳された用語（「man」）に一致する検索結果が収集され、検索結果としてユーザーに返されます。

## その他のリソース{#additional-resources}

* [AEM Assets でのスマート翻訳検索の設定](smart-translation-search-technical-video-setup.md)
* [Apache Joshua 言語パック](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
