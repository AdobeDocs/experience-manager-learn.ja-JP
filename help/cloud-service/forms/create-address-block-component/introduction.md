---
title: 住所コンポーネントの作成
description: AEM Forms Cloud Serviceでの新しいアドレスコアコンポーネントの作成
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15752
exl-id: 21b6640e-5cfa-4902-9660-a2b1c91b285d
source-git-commit: a12b1778413079646814cb25567abfc26a429340
workflow-type: tm+mt
source-wordcount: '160'
ht-degree: 50%

---

# AEM Formsでの新しいコアコンポーネントの作成

Adobe Experience Manager（AEM）のコンポーネントとは、ページやフォームの作成に使用される構築ブロックを意味します。 作成者がコンテンツを作成および管理するためのシンプルで強力な方法が提供されると同時に、開発者はカスタムコンポーネントの作成に必要な柔軟性と拡張性を活用できます。 これらは、web サイトやフォームの開発時間を短縮し、メンテナンスコストを削減するように設計されており、特定のニーズに合わせて柔軟かつ容易にカスタマイズできます。

このチュートリアルでは、住所ブロックコンポーネントを作成します。 住所ブロックコンポーネントには、住所、市区町村、都道府県、郵便番号を取得するためのフィールドがあります。

![ 最終アドレス ](assets/final-address-component.png)

## 前提条件

* AEM Forms Cloud Serviceインスタンスへのアクセス
* AEM Forms モジュールを使用したフォームの開発経験
* AEM/AEM Forms（Git、IntelliJ など）の開発環境のセットアップ経験

## 次の手順

[開発環境の設定](./set-up.md)