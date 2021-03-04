---
title: JSONスキーマとデータを持つAEM Forms[パート1]
seo-title: JSONスキーマとデータを持つAEM Forms[Part1]
description: マルチパートチュートリアルを参照し、JSONスキーマを使用したアダプティブフォームの作成、送信されたデータのクエリに関する手順を実行してください。
seo-description: マルチパートチュートリアルを参照し、JSONスキーマを使用したアダプティブフォームの作成、送信されたデータのクエリに関する手順を実行してください。
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: 開発
role: デベロッパー
level: 経験豊富な
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '299'
ht-degree: 3%

---


# JSONスキーマに基づくアダプティブフォームの作成


AEM Forms6.3リリースでは、JSONスキーマを基にアダプティブFormsを作成する機能が導入されました。 JSONスキーマを使用したアダプティブFormsの作成に関する詳細は、この[記事](https://helpx.adobe.com/jp/experience-manager/6-3/forms/using/adaptive-form-json-schema-form-model.html)で説明されています。

JSONスキーマに基づいてアダプティブフォームを作成したら、次の手順は、送信されたデータをデータベースに保存することです。 この目的で、様々なデータベースベンダーが導入した新しいJSONデータ型を使用します。 この記事の目的で、MySql 8データベースを使用して送信データを保存します。

この記事ではMySql 8データベースが使用されています。 MySQLには、[JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html)という新しいデータ型が導入されました。 これにより、JSONオブジェクトの保存とクエリが容易になります。 送信されたデータは、データベース内のJSON型の列に格納されます。

以下のスクリーンショットは、送信されたフォームデータをJSONデータタイプに保存したものです。 列「formdata」の型はJSONです。 また、データに関連付けられたフォームの名前を列のformnameに保存しました

>[!NOTE]
>
>jsonスキーマファイルが適切に命名されていることを確認してください。 例えば、&lt;name>スキーマ.jsonの形式で名前を付ける必要があります。 したがって、スキーマファイルはmortgage.スキーマ.jsonまたはcredit.スキーマ.jsonにすることができます。


![datastored](assets/datastored.gif)


[アダプティブFormsの作成に使用できるJSONスキーマのサンプル。](assets/samplejsonschemas.zip)」を選択します。zipファイルをダウンロードして解凍し、JSONスキーマを取得します

