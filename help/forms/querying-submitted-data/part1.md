---
title: JSON スキーマとデータを使用したAEM Forms[ 第 1 部 ]
seo-title: AEM Forms with JSON Schema and Data[Part1]
description: マルチパートチュートリアルで、JSON スキーマを使用したアダプティブフォームの作成と、送信されたデータのクエリに関する手順を説明します。
seo-description: Multi-Part tutorial to walk you through the steps involved in creating Adaptive Form with JSON schema and querying the submitted data.
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: c588bdca-b8a8-4de2-97e0-ba08b195699f
source-git-commit: 25a1a40f42d37443db9edc0e09b1691b1c19e848
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 0%

---

# JSON スキーマに基づくアダプティブフォームの作成


AEM Forms 6.3 リリースでは、JSON スキーマに基づいてアダプティブFormsを作成する機能が導入されました。 JSON スキーマを使用してアダプティブFormsを作成する方法について詳しくは、このドキュメントで説明します [記事](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/adaptive-form-json-schema-form-model.html).

JSON スキーマに基づくアダプティブフォームを作成したら、次の手順では、送信されたデータをデータベースに保存します。 この目的で、様々なデータベースベンダーが導入した新しい JSON データタイプを使用します。 この記事の目的で、MySql 8 データベースを使用して送信データを保存します。

この記事では、MySql 8 データベースを使用していました。 MySQL では、という新しいデータ型が導入されました。 [JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html). これにより、JSON オブジェクトの保存とクエリが容易になります。 送信されたデータは、データベースの JSON タイプの列に保存されます。

以下のスクリーンショットは、JSON データタイプに保存された送信済みフォームデータを示しています。 「formdata」列の型は JSON です。 また、列のフォーム名に、データに関連付けられたフォームの名前を格納しました

>[!NOTE]
>
>json スキーマファイルの名前が適切に設定されていることを確認してください。 例えば、次の形式で名前を付ける必要があります &lt;name>schema.json. そのため、スキーマファイルは mortgage.schema.json または credit.schema.json にすることができます。


![datastored](assets/datastored.gif)


[アダプティブFormsの作成に使用できるサンプル JSON スキーマ。](assets/samplejsonschemas.zip)。zip ファイルをダウンロードして展開し、JSON スキーマを取得します。
