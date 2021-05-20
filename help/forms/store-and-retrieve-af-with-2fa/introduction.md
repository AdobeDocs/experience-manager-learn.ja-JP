---
title: MySQLデータベースから添付ファイル付きのフォームデータを保存および取得する
description: 添付ファイルを含むフォームデータの保存と取得に関する手順について説明するマルチパートチュートリアル
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6593
thumbnail: 327122.jpg
topic: 開発
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 4%

---


# 2FAを使用したアダプティブフォームデータの保存と取得

このチュートリアルでは、2FAを使用して添付ファイルを含むアダプティブフォームデータを保存し、取得する手順について説明します。 このチュートリアルでは、MySQLデータベースを使用してアダプティブフォームデータを保存しました。 データベース固有のドライバーをAEMにデプロイしている限り、任意のデータベースを使用してデータを保存できます。 この使用例を実現するには、大まかに次の手順を実行する必要があります。

* GuideBridge APIを使用してアダプティブフォームデータにアクセスする

* サーブレットへのPOST呼び出しを実行します。 このサーブレットは、データをデータベースに、フォーム添付ファイルをCRXリポジトリに格納します。 データベースに格納されたデータは、GUIDに関連付けられます。

* 保存されたデータをアダプティブフォームに入力する場合は、GUIDに関連付けられたデータを取得し、**request.setAttribute**&#x200B;メソッドを使用してアダプティブフォームに入力します。

## 使用例のデモ

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=9&learn=on)

## 前提条件

このコンテンツのオーディエンスは、次の領域で経験が必要です。

* アダプティブフォーム
* フォームデータモデル
* OSGiサービス/コンポーネント
* AEMクライアントライブラリ
