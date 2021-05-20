---
title: AEM FormsとMarketo（パート4）
seo-title: AEM FormsとMarketo（パート4）
description: AEM Formsフォームデータモデルを使用したAEM FormsとMarketoの統合に関するチュートリアルです。
seo-description: AEM Formsフォームデータモデルを使用したAEM FormsとMarketoの統合に関するチュートリアルです。
feature: アダプティブForms、フォームデータモデル
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
topic: 開発
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 0%

---


# フォームデータモデルを使用したアダプティブフォームの作成

次の手順では、アダプティブフォームを作成し、前の手順で作成したフォームデータモデルをベースにします。
ユーザーはリードIDを入力し、Marketoサービスをタブで移動して、IDによるリードを取得します。 次に、サービス操作の結果が、アダプティブFormsの適切なフィールドにマッピングされます。

1. アダプティブフォームを作成し、「空のフォームテンプレート」をベースに、前の手順で作成したフォームデータモデルに関連付けます。
1. フォームを編集モードで開く
1. TextFieldコンポーネントとパネルコンポーネントをアダプティブフォームにドラッグ&amp;ドロップします。 TextFieldコンポーネントのタイトルを「Enter Lead Id」に設定し、名前を「LeadId」に設定します。
1. 2つのTextFieldコンポーネントをパネルコンポーネントにドラッグ&amp;ドロップします
1. 2つのテキストフィールドコンポーネントの名前とタイトルをFirstNameおよびLastNameに設定します。
1. パネルコンポーネントを繰り返し可能なコンポーネントに設定するには、「最小」を1に、「最大」を —1に設定します。 Marketoサービスがリードオブジェクトの配列を返すので、結果を表示するには繰り返し可能なコンポーネントが必要です。 ただし、この場合、IDでリードオブジェクトを検索するので、リードオブジェクトは1つしか返されません。
1. 下の図に示すように、「LeadId」フィールドにルールを作成します
1. フォームをプレビューし、有効なリードIDを「LeadID」フィールドに入力してタブを閉じます。 「名」フィールドと「姓」フィールドには、サービス呼び出しの結果が入力されます。

次のスクリーンショットは、ルールエディターの設定を説明しています

![ruleeditor](assets/ruleeditor.jfif)
