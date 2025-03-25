---
title: JSON スキーマとデータを使用した AEM Forms [パート 1]
description: 複数のパートで構成されているチュートリアルでは、JSON スキーマを使用したアダプティブフォームの作成と、送信されたデータのクエリに関する手順を説明します。
feature: Adaptive Forms
doc-type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: c588bdca-b8a8-4de2-97e0-ba08b195699f
duration: 50
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 100%

---

# JSON スキーマに基づいてアダプティブフォームを作成する


AEM Forms 6.3 リリースでは、JSON スキーマに基づいてアダプティブフォームを作成する機能が導入されました。JSON スキーマを使用してアダプティブフォームを作成する方法について詳しくは、この[記事](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/template-editor.html?lang=ja)を参照してください。

JSON スキーマに基づくアダプティブフォームを作成したら、次の手順では、送信されたデータをデータベースに保存します。この目的で、様々なデータベースベンダーが導入した新しい JSON データタイプを使用します。 この記事では、MySql 8 データベースを使用して送信データを保存します。

この記事では、MySql 8 データベースが使用されました。MySQL では、[JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html) という新しいデータタイプが導入されました。これにより、JSON オブジェクトの保存とクエリが容易になります。送信されたデータは、データベースの JSON タイプの列に保存されます。

以下のスクリーンショットは、JSON データタイプに保存された送信済みフォームデータを示しています。 「formdata」列の型は JSON です。 また、formname 列のデータに関連付けられたフォームの名前を格納しました

>[!NOTE]
>
>JSON スキーマファイルの名前が適切に設定されていることを確認してください。 例えば、&lt;name>schema.json の形式で名前を付ける必要があります。そのため、スキーマファイルは mortgage.schema.json または credit.schema.json にすることができます。


![datastored](assets/datastored.gif)


[アダプティブフォームの作成に使用できるサンプル JSON スキーマ。](assets/samplejsonschemas.zip)zip ファイルをダウンロードして展開し、JSON スキーマを取得します。
