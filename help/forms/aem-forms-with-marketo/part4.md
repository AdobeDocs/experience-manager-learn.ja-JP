---
title: AEM Formsとマーケット（パート4）
seo-title: AEM Formsとマーケット（パート4）
description: AEM Formsフォームデータモデルを使用したAEM Formsとマーケティングツールの統合に関するチュートリアルです。
seo-description: AEM Formsフォームデータモデルを使用したAEM Formsとマーケティングツールの統合に関するチュートリアルです。
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 0%

---


# フォームデータモデルを使用したアダプティブフォームの作成

次の手順は、アダプティブフォームを作成し、前の手順で作成したフォームデータモデルを基にしてアダプティブフォームを作成することです。
ユーザーはリードIDを入力し、Marketorサービスをタブ順序でタブアウトして、リードby idが呼び出されます。 次に、サービス業の結果が、適応Formsの適切なフィールドにマップされます。

1. アダプティブフォームを作成し、「空白のフォームテンプレート」を基にして、前の手順で作成したフォームデータモデルに関連付けます。
1. フォームを編集モードで開く
1. TextFieldコンポーネントとPanelコンポーネントをアダプティブフォームにドラッグ&amp;ドロップします。 TextFieldコンポーネントのタイトルを「Enter Lead Id」に設定し、名前を「LeadId」に設定します。
1. 2つのTextFieldコンポーネントをパネルコンポーネントにドラッグ&amp;ドロップします
1. 2つのTextfieldコンポーネントの名前とタイトルをFirstNameとLastNameに設定します
1. パネルコンポーネントを繰り返し可能なコンポーネントに設定するには、最小値を1、最大値を —1に設定します。 これは、Marketoサービスが一連のリードオブジェクトを返すので必須です。結果を表示するには、繰り返し可能なコンポーネントが必要です。 ただし、この場合、IDでリードオブジェクトを検索するので、1つのリードオブジェクトしか取り戻されません。
1. 以下の画像に示すように、「LeadId」フィールドにルールを作成します
1. フォームにプレビューを入力し、「LeadID」フィールドに有効なリードIDを入力し、タブを閉じます。 「名」および「姓」フィールドには、サービス呼び出しの結果が入力されます。

次のスクリーンショットは、ルールエディターの設定を説明しています

![ruleeditor](assets/ruleeditor.jfif)
