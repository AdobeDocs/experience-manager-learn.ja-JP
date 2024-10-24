---
title: クリック可能な画像コンポーネントの作成
description: AEM Forms as a Cloud Service でクリック可能な画像コンポーネントを作成します。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15968
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: c451472f-d282-4662-9852-8a3e73c5c853
source-git-commit: b4df652fcda0af5d01077b97aa7fa17cfe2abf4b
workflow-type: ht
source-wordcount: '138'
ht-degree: 100%

---

# クリック可能な画像の概要

Forms でクリック可能な画像を使用すると、より魅力的で直感的、視覚的にアピールするユーザーエクスペリエンスを作成できます。この記事では、特にデザインの柔軟性、パフォーマンス、ユーザーエクスペリエンスの面でいくつかのメリットがあるので、クリック可能な画像に SVG を使用しました。
SVG は、Adobe Illustrator または無料のオンラインツールを使用して作成できます。ユースケースの説明に、simplemaps の[米国マップ](https://simplemaps.com/resources/svg-us)を使用しました。

## クリック可能な米国マップを使用するユースケース

クリック可能な米国のマップにより、ユーザーは州別のフォーム送信を確認できます。ユーザーが州をクリックすると、その州からの送信がリストされ、特定の送信を開くオプションが表示されます。
