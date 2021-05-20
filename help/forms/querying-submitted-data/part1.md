---
title: AEM FormsとJSONスキーマおよびデータ[パート1]
seo-title: JSONスキーマとデータを使用したAEM Forms[Part1]
description: マルチパートチュートリアルでは、JSONスキーマを使用したアダプティブフォームの作成と送信済みデータのクエリに関する手順について説明します。
seo-description: マルチパートチュートリアルでは、JSONスキーマを使用したアダプティブフォームの作成と送信済みデータのクエリに関する手順について説明します。
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
source-wordcount: '297'
ht-degree: 3%

---


# JSONスキーマに基づくアダプティブフォームの作成


AEM Forms 6.3リリースでは、JSONスキーマに基づいてアダプティブFormsを作成する機能が導入されました。 JSONスキーマを使用してアダプティブFormsを作成する方法の詳細については、この[記事](https://helpx.adobe.com/jp/experience-manager/6-3/forms/using/adaptive-form-json-schema-form-model.html)で説明します。

JSONスキーマに基づくアダプティブフォームを作成したら、次の手順では、送信済みデータをデータベースに保存します。 この目的で、様々なデータベースベンダーが導入した新しいJSONデータ型を使用します。 この記事の目的で、MySQL 8データベースを使用して送信データを保存します。

この記事では、MySql 8データベースを使用していました。 MySQLでは、[JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html)という新しいデータ型が導入されました。 これにより、JSONオブジェクトの保存とクエリが容易になります。 送信されたデータは、データベース内のJSONタイプの列に格納されます。

次のスクリーンショットは、JSONデータタイプに保存された送信済みフォームデータを示しています。 「formdata」列の型はJSONです。 また、データに関連付けられたフォームの名前を列のフォーム名に格納します

>[!NOTE]
>
>jsonスキーマファイルが適切に名前付けされていることを確認してください。 例えば、&lt;name>schema.jsonという形式で名前を付ける必要があります。 したがって、スキーマファイルはmortgage.schema.jsonまたはcredit.schema.jsonにすることができます。


![datastored](assets/datastored.gif)


[アダプティブFormsの作成に使用できるJSONスキーマのサンプル。](assets/samplejsonschemas.zip)」を選択します。zipファイルをダウンロードして解凍し、JSONスキーマを取得します。

