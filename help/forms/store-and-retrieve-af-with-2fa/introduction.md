---
title: MySQLデータベースからの添付ファイル付きフォームデータの格納と取得
description: 添付ファイルを含むフォームデータの格納と取得に関する手順について説明するマルチパートチュートリアル
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6593
thumbnail: 327122.jpg
topic: 開発
role: デベロッパー
level: 経験豊富な
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 4%

---


# 2FAを使用したアダプティブフォームデータの保存と取得

このチュートリアルでは、2FAを使用した添付ファイル付きアダプティブフォームデータの保存と取得に関する手順について説明します。 このチュートリアルでは、MySQLデータベースを使用してアダプティブフォームデータを保存していました。 AEMにデータベース固有のドライバーが展開されている場合は、任意のデータベースを使用してデータを保存できます。 この使用例を達成するには、高レベルで次の手順が必要です。

* GuideBridge APIを使用してアダプティブフォームデータにアクセスする

* サーブレットへのPOST呼び出しを行います。 このサーブレットは、データをデータベースに、フォーム添付をCRXリポジトリに保存します。 データベースに格納されたデータは、GUIDに関連付けられています。

* アダプティブフォームに保存されたデータを入力する場合は、GUIDに関連付けられたデータを取得し、**request.setAttribute**&#x200B;メソッドを使用してアダプティブフォームに入力します。

## 使用事例のデモ

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)

## 前提条件

このコンテンツのオーディエンスは、次の領域での経験が必要です。

* アダプティブフォーム
* フォームデータモデル
* OSGiサービス/コンポーネント
* AEMクライアントライブラリ
