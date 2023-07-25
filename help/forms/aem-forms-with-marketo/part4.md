---
title: AEM Forms と Marketo（第 4 部）
description: AEM Forms フォームデータモデルを使用した AEM Forms と Marketo の統合に関するチュートリアル
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="統合" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 6b44e6b2-15f7-45b2-8d21-d47f122c809d
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '332'
ht-degree: 8%

---

# フォームデータモデルを使用したアダプティブフォームの作成

次の手順では、アダプティブフォームを作成し、前の手順で作成したフォームデータモデルに基づいてアダプティブフォームを作成します。
ユーザーがリード ID を入力し、Marketoサービスをタブでタブアウトして、ID でリードを取得すると、呼び出されます。 次に、サービス操作の結果が、アダプティブFormsの適切なフィールドにマッピングされます。

1. アダプティブフォームを作成し、「空のフォームテンプレート」をベースにして、前の手順で作成したフォームデータモデルに関連付けます。
1. フォームを編集モードで開く
1. TextField コンポーネントとパネルコンポーネントをアダプティブフォームにドラッグ&amp;ドロップします。 テキストフィールドコンポーネントのタイトルを「リード ID を入力」に設定し、名前を「リード ID」に設定します。
1. 2 つの TextField コンポーネントをパネルコンポーネントにドラッグ&amp;ドロップします
1. 2 つのテキストフィールドコンポーネントの「名前」と「タイトル」を「名」および「姓」に設定します。
1. パネルコンポーネントを繰り返し可能なコンポーネントに設定するには、「最小」を 1 に、「最大」を —1 に設定します。 Marketoサービスがリードオブジェクトの配列を返すので、結果を表示するには繰り返し可能なコンポーネントが必要です。 ただし、この場合、ID でリードオブジェクトを検索するので、リードオブジェクトは 1 つだけ返されます。
1. 下の画像に示すように、「LeadId」フィールドにルールを作成します
1. フォームをプレビューし、「LeadID」フィールドに有効なリード ID を入力して、「Tab out」をクリックします。 「名」フィールドと「姓」フィールドには、サービス呼び出しの結果が入力されます。

次のスクリーンショットでは、ルールエディターの設定について説明しています

![ruleeditor](assets/ruleeditor.jfif)

## デバッグ

この記事で提供されるバンドルを使用している場合は、 [デバッグログ](http://localhost:4502/system/console/slinglog) 次のクラスの場合：

+ `com.marketoandforms.core.impl.MarketoServiceImpl`
+ `com.marketoandforms.core.MarketoConfigurationService`

## これで完了です

AEM Formsフォームデータモデルを使用して、AEM FormsをMarketoと正常に統合しました。