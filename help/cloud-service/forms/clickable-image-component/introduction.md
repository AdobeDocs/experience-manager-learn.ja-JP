---
title: クリック可能な画像コンポーネントの作成
description: AEM Forms Cloud Serviceでクリック可能な画像コンポーネントを作成します。
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15968
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: c451472f-d282-4662-9852-8a3e73c5c853
source-git-commit: 1fa202910e6c9d21532c6027647c5f3909931e0e
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 4%

---

# クリック可能な画像の概要

Formsでクリック可能な画像を使用すると、より魅力的で直感的な、視覚的なユーザーエクスペリエンスを作成できます。 そこで、クリック画像にSVGを適用したところ、デザインの自由度や性能、ユーザーエクスペリエンスの面で優れていることが分かりました。
SVGは、Adobe Illustratorまたは無料のオンラインツールを使用して作成できます。 ユースケースのデモに [USA Map from](https://simplemaps.com/resources/svg-us)simplemaps を使用しました。

## クリック可能な USA マップを使用するユースケース

米国のクリック可能なマップを使用すると、ユーザーは状態固有のフォーム送信を調べることができます。 ユーザーが状態をクリックすると、その状態からの送信が一覧表示され、特定の送信を開くオプションが表示されます。
