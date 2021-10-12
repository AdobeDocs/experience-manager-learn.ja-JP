---
title: AEM FormsとMarketo（パート 4）
description: AEM Formsフォームデータモデルを使用したAEM FormsとMarketoの統合に関するチュートリアルです。
feature: Adaptive Forms, Form Data Model
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 6b44e6b2-15f7-45b2-8d21-d47f122c809d
source-git-commit: 020852f16de0cdb1e17e19ad989dabf37b7f61f5
workflow-type: tm+mt
source-wordcount: '317'
ht-degree: 1%

---

# フォームデータモデルを使用したアダプティブフォームの作成

次の手順では、アダプティブフォームを作成し、前の手順で作成したフォームデータモデルをベースにします。
ユーザーはリード ID を入力し、Tab キーを押しながらMarketoサービスをタブアウトして、ID 別にリードを取得します。 次に、サービス操作の結果が、アダプティブFormsの適切なフィールドにマッピングされます。

1. アダプティブフォームを作成し、「空のフォームテンプレート」をベースにして、前の手順で作成したフォームデータモデルに関連付けます。
1. フォームを編集モードで開く
1. TextField コンポーネントと Panel コンポーネントをアダプティブフォームにドラッグ&amp;ドロップします。 TextField コンポーネントのタイトルを「リード ID を入力」に設定し、名前を「リード ID」に設定します。
1. 2 つの TextField コンポーネントをパネルコンポーネントにドラッグ&amp;ドロップします
1. 2 つのテキストフィールドコンポーネントの名前とタイトルを FirstName および LastName に設定します。
1. パネルコンポーネントを繰り返し可能なコンポーネントに設定するには、「最小」を 1 に、「最大」を —1 に設定します。 Marketoサービスがリードオブジェクトの配列を返すので、結果を表示するには繰り返し可能なコンポーネントが必要です。 ただし、この場合、ID でリードオブジェクトを検索するので、1 つのリードオブジェクトのみが返されます。
1. 下の図に示すように、「LeadId」フィールドにルールを作成します
1. フォームをプレビューし、「LeadID」フィールドに有効なリード ID を入力して、「Tab Out」にします。 「名」フィールドと「姓」フィールドには、サービス呼び出しの結果が入力されます。

次のスクリーンショットは、ルールエディターの設定を説明しています

![ruleeditor](assets/ruleeditor.jfif)

## デバッグ

この記事で提供されるバンドルを使用している場合は、次のクラスに対して [ デバッグログ ](http://localhost:4502/system/console/slinglog) を有効にすることができます。

+ `com.marketoandforms.core.impl.MarketoServiceImpl`
+ `com.marketoandforms.core.MarketoConfigurationService`
