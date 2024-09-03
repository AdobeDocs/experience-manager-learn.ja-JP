---
title: クリック可能な画像コンポーネントの作成
description: AEM Forms Cloud Serviceでのクリック可能な画像コンポーネントの作成
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15968
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
exl-id: c451472f-d282-4662-9852-8a3e73c5c853
source-git-commit: ba744f95f8d1f0b982cd5430860f0cb0945a4cda
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 5%

---

# はじめに

Formsでクリック可能な画像を使用すると、より魅力的で直感的な、視覚的なユーザーエクスペリエンスを作成できます。 そこで、クリック画像にSVGを適用したところ、デザインの自由度や性能、ユーザーエクスペリエンスの面で優れていることが分かりました。
SVGは、Adobe Illustratorまたは無料のオンラインツールを使用して作成できます。 ユースケースのデモに [USA Map from](https://simplemaps.com/resources/svg-us)simplemaps を使用しました。

## クリック可能な USA マップを使用するユースケース

米国のクリック可能なマップを使用すると、ユーザーは状態固有のフォーム送信を調べることができます。 ユーザーが状態をクリックすると、その状態からの送信が一覧表示され、特定の送信を開くオプションが表示されます。
