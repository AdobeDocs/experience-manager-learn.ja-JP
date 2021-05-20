---
title: MySQLデータベースからのフォームデータの格納と取得
description: フォームデータの保存と取得に関する手順について説明するマルチパートチュートリアル
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: 開発
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '167'
ht-degree: 1%

---


# MySQLデータベースからのアダプティブフォームデータの保存と取得

このチュートリアルでは、アダプティブフォームデータをデータベースから保存して取得する手順について説明します。 このチュートリアルでは、MySQLデータベースを使用してアダプティブフォームデータを保存しました。 データベース固有のドライバーをAEMにデプロイしている限り、任意のデータベースを使用してデータを保存できます。 この使用例を実現するには、大まかに次の手順を実行する必要があります。

* GuideBridge APIを使用してアダプティブフォームデータにアクセスする

* サーブレットへのPOST呼び出しを実行します。 このサーブレットは、データをデータベースに格納します。 保存されたデータはGUIDに関連付けられています

* 保存されたデータをアダプティブフォームに入力する場合は、GUIDに関連付けられたデータを取得し、**request.setAttribute**&#x200B;メソッドを使用してアダプティブフォームに入力します。

## 使用例のデモ

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=9&learn=on)
