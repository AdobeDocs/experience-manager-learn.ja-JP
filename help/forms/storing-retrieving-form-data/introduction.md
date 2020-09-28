---
title: MySQLデータベースからのフォームデータの格納と取得
description: フォームデータの格納と取得に関する手順について説明するマルチパートチュートリアル
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '164'
ht-degree: 0%

---


# MySQLデータベースからのアダプティブフォームデータの格納と取得

このチュートリアルでは、アダプティブフォームデータの保存とデータベースからの取得に関する手順について説明します。 このチュートリアルでは、MySQLデータベースを使用してアダプティブフォームデータを保存していました。 AEMにデータベース固有のドライバーが展開されている場合は、任意のデータベースを使用してデータを保存できます。 この使用例を達成するには、高レベルで次の手順が必要です。

* GuideBridge APIを使用してアダプティブフォームデータにアクセスする

* サーブレットへのPOST呼び出しを行います。 このサーブレットは、データをデータベースに格納します。 保存されたデータはGUIDに関連付けられています

* アダプティブフォームに保存されたデータを入力する場合は、GUIDに関連付けられたデータを取得し、 **request.setAttribute** メソッドを使用してアダプティブフォームに入力します。

## 使用事例のデモ

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=9&learn=on)
