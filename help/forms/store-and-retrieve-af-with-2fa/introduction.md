---
title: 添付ファイル付きフォームデータの MySQL データベースへの保存と MySQL データベースからの取得
description: 添付ファイルを含んだフォームデータの保存と取得に関する手順について説明するマルチパートチュートリアル
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6593
thumbnail: 327122.jpg
topic: Development
role: Developer
level: Experienced
exl-id: b278652f-6c09-4abc-b92e-20bfaf2e791a
last-substantial-update: 2020-11-07T00:00:00Z
duration: 148
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 100%

---

# 2FA を使用したアダプティブフォームデータの保存と取得

このチュートリアルでは、添付ファイルを含んだアダプティブフォームデータを 2FA を使用して保存および取得する手順について説明します。 このチュートリアルでは、MySQL データベースを使用してアダプティブフォームデータを保存しました。 データベース固有のドライバーを AEM にデプロイしている限り、任意のデータベースを使用してデータを保存できます。 ユースケースを実現するには、大まかには、次の手順を実行する必要があります。

* GuideBridge API を使用して、アダプティブフォームデータにアクセスします。

* サーブレットへの POST 呼び出しを実行します。 このサーブレットは、データをデータベースに保存し、フォーム添付ファイルを CRX リポジトリに保存します。 データベースに格納されたデータは、GUID に関連付けられます。

* 保存されたデータをアダプティブ フォームに入力する場合は、GUID に関連付けられているデータを取得し、**request.setAttribute** メソッドを使用してアダプティブフォームに入力します。

## ユースケースのデモ

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)

## 前提条件

このコンテンツの対象読者には、次の分野の何らかの経験があることが想定されています。

* アダプティブフォーム
* フォームデータモデル
* OSGi サービス／コンポーネント
* AEM クライアントライブラリ


## 次の手順

[データソースの設定](./configure-data-source.md)