---
title: MySQL データベースからのフォームデータの格納と取得の概要
description: フォームデータの保存と取得に関する手順について説明するマルチパートチュートリアル
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 95795102-4278-4556-8e0f-1b8a359ab093
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 0%

---

# MySQL データベースからのアダプティブフォームデータの保存と取得

このチュートリアルでは、アダプティブフォームデータをデータベースから保存および取得する手順について説明します。 このチュートリアルでは、MySQL データベースを使用してアダプティブフォームデータを保存しました。 データベース固有のドライバーをAEMにデプロイしている限り、任意のデータベースを使用してデータを保存できます。 この使用例を達成するには、次の手順を大まかに実行する必要があります。

* GuideBridge API を使用してアダプティブフォームデータにアクセスする

* サーブレットへのPOST呼び出しを実行します。 このサーブレットは、データをデータベースに格納します。 保存されたデータは GUID に関連付けられています

* 保存されたデータをアダプティブフォームに入力する場合は、GUID に関連付けられているデータを取得し、 **request.setAttribute** メソッド。

## 使用例のデモ

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=9&learn=on)
