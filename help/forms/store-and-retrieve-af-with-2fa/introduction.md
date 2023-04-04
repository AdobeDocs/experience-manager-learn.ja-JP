---
title: MySQL データベースからの添付ファイル付きフォームデータの保存と取得
description: 添付ファイルを含むフォームデータの保存と取得に関する手順について説明するマルチパートチュートリアル
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6593
thumbnail: 327122.jpg
topic: Development
role: Developer
level: Experienced
exl-id: b278652f-6c09-4abc-b92e-20bfaf2e791a
last-substantial-update: 2020-11-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 2%

---

# 2FA を使用したアダプティブフォームデータの保存と取得

このチュートリアルでは、2FA を使用して添付ファイルを含むアダプティブフォームデータを保存および取得する手順について説明します。 このチュートリアルでは、MySQL データベースを使用してアダプティブフォームデータを保存しました。 データベース固有のドライバーをAEMにデプロイしている限り、任意のデータベースを使用してデータを保存できます。 この使用例を達成するには、次の手順を大まかに実行する必要があります。

* GuideBridge API を使用してアダプティブフォームデータにアクセスする

* サーブレットへのPOST呼び出しを実行します。 このサーブレットは、データベースのデータと CRX リポジトリのフォーム添付ファイルを保存します。 データベースに格納されたデータは、GUID に関連付けられます。

* 保存されたデータをアダプティブフォームに入力する場合は、GUID に関連付けられているデータを取得し、 **request.setAttribute** メソッド。

## 使用例のデモ

>[!VIDEO](https://video.tv.adobe.com/v/327122?quality=12&learn=on)

## 前提条件

このコンテンツのオーディエンスは、次の領域で何らかの経験を持つことが予想されます。

* アダプティブフォーム
* フォームデータモデル
* OSGi サービス/コンポーネント
* AEMクライアントライブラリ
