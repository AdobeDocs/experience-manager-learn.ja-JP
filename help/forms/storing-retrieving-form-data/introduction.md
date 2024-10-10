---
title: フォームデータの MySQL データベースへの保存と MySQL データベースからの取得：概要
description: フォームデータの保存と取得に関わる手順について説明するマルチパートチュートリアル
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 95795102-4278-4556-8e0f-1b8a359ab093
last-substantial-update: 2019-07-07T00:00:00Z
duration: 236
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '165'
ht-degree: 100%

---

# アダプティブフォームデータの MySQL データベースへの保存と MySQL データベースからの取得

このチュートリアルでは、アダプティブフォームデータをデータベースに保存する手順とデータベースから取得する手順について説明します。このチュートリアルでは、MySQL データベースを使用してアダプティブフォームデータを保存しました。 データベース固有のドライバーを AEM にデプロイしている限り、任意のデータベースを使用してデータを保存できます。 ユースケースを実現するには、大まかには、次の手順を実行する必要があります。

* GuideBridge API を使用して、アダプティブフォームデータにアクセスします。

* サーブレットへの POST 呼び出しを実行します。 このサーブレットは、データをデータベースに保存します。保存されたデータは GUID に関連付けられています。

* 保存されたデータをアダプティブ フォームに入力する場合は、GUID に関連付けられているデータを取得し、**request.setAttribute** メソッドを使用してアダプティブフォームに入力します。

## ユースケースのデモ

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=12&learn=on)


