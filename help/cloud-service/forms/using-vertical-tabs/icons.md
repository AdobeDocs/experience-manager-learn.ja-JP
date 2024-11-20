---
title: カスタムアイコンの追加
description: 垂直タブへのカスタムアイコンの追加
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
thumbnail: 331891.jpg
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16418
exl-id: 20e44be0-5490-4414-9183-bb2d2a80bdf0
source-git-commit: faa859897b6b9fbb0acff02000611de216ddda3e
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 95%

---

# カスタムアイコンの追加

タブにカスタムアイコンを追加すると、いくつかの点でユーザーエクスペリエンスと視覚的な魅力が向上します。

* ユーザビリティの向上：アイコンを使用すると、各タブの目的をすばやく伝えることができるので、ユーザーは探しているものを一目で簡単に見つけることができます。アイコンなどの視覚的なヒントは、ユーザーがより直感的に操作するのに役立ちます。

* 視覚的な階層とフォーカス：アイコンを使用すると、タブ間の区切りがより明確になり、視覚的な階層が改善されます。これは、重要なタブを目立たせ、ユーザーの注意を効果的に導くのに役立ちます。
この記事に従うと、次に示すようにアイコンを配置できます

![アイコン](assets/icons.png)

## 前提条件

この記事に従うには、Git、Cloud Manager を使用した AEM プロジェクトの作成とデプロイ、AEM Cloud Manager でのフロントエンドパイプラインの設定、および少しの CSS に精通している必要があります。上記のトピックに精通していない場合は、[テーマを使用したコアコンポーネントのスタイル設定](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/using-themes-in-core-components#rename-env-file-theme-folder)の記事を参照してください。

## テーマへのアイコンの追加

Visual Studio Code または任意の他のエディターでテーマプロジェクトを開きます。
任意のアイコンを画像フォルダーに追加します。
赤色でマークされたアイコンは、新しく追加されたアイコンです。
![new-icons](assets/newicons.png)

## アイコンを保存する icon-map の作成

_variable.scss ファイルに icon-map を作成します。 SCSS マップ $icon-map はキーと値のペアのコレクションで、各キーはアイコン名（ホーム、ファミリーなど）を表し、各値はそのアイコンに関連付けられた画像ファイルへのパスです。

![variable-scss](assets/variable_scss.png)

```css
$icon-map: (
    home: "./resources/images/home.png",
    family: "./resources/images/icons8-family-80.png",
    pdf: "./resources/images/pdf.png",
    income: "./resources/images/income.png",
    assets: "./resources/images/assets.png",
    cars: "./resources/images/cars.png"
);
```

## mixin の追加

_mixin.scss に次のコードを追加します

```css
@mixin add-icon-to-vertical-tab($image-url) {
  display: inline-flex;
  align-self: center;
  &::before {
    content: "";
    display:inline-block;
    background: url($image-url) left center / cover no-repeat;
    margin-right: 8px; /* Space between icon and text */
    height:40px;
    width:40px;
    vertical-align:middle;
    
  }
  
}
```

add-icon-to-vertical-tab mixin は、垂直タブ上のテキストの横にカスタムアイコンを追加するように設計されています。画像をアイコンとしてタブに簡単に含め、テキストの横に配置してスタイルを設定し、一貫性と整列を確保できます。

Mixin の分類。Mixin の各パートの機能は次のとおりです。

パラメーター:

* $image-url：タブテキストの横に表示するアイコンまたは画像の URL。このパラメーターを渡すと、必要に応じて様々なアイコンを異なるタブに追加できるので、Mixin が汎用になります。

* 適用されるスタイル：

   * display: inline-flex：これにより、要素が Flex コンテナになり、ネストされたコンテンツ（アイコンやテキストなど）が水平方向に揃えられます。
   * align-self: center：要素がコンテナ内で垂直方向に中央に配置されるようにします。
   * 疑似要素（::before）：
   * content: &quot;&quot;：疑似要素 ::before を初期化します。この疑似要素は、アイコンを背景画像として表示するために使用されます。
   * display: inline-block：疑似要素を inline-block に設定することで、テキストのインラインに配置されたアイコンのように動作させることができます。
   * background: url($image-url) left center / cover no-repeat;：$image-url 経由で指定された URL を使用して背景画像を追加します。アイコンを左揃えにし、垂直方向に中央揃えにします。

## _verticaltabs.scss の更新

この記事の目的で、タブアイコンを表示する新しい css クラス（cmp-verticaltabs--marketing）を作成しました。この新しいクラスでは、アイコンを追加してタブ要素を拡張します。css クラスの完全なリストは次のとおりです

```css
.cmp-verticaltabs--marketing
{
  .cmp-verticaltabs
    {
      &__tab 
        {
          cursor:pointer;
            @each $name, $url in $icon-map {
            &[data-icon-name="#{$name}"]
              {
                  @include add-icon-to-vertical-tab($url);
              }
            }
        }
    }
}
```

## verticaltabs コンポーネントの変更

```/apps/core/fd/components/form/verticaltabs/v1/verticaltabs/verticaltabs.html``` から verticaltabs.html ファイルをコピーし、プロジェクトの verticaltabs コンポーネントの下にペーストします。次の画像に示すように、li 役割の下にあるコピーしたファイルに行 ```data-icon-name="${tab.name}"``` を追加します
![data-icon](assets/data-icons.png)
タブ名の値使用して data-icon-name というカスタムデータ属性を設定します。タブ名がアイコンマップの画像名と一致する場合、対応する画像がタブに関連付けられます。



## コードのテスト

更新された verticaltabs コンポーネントをクラウドインスタンスにデプロイします。
フロントエンドパイプラインを使用して、更新されたテーマをデプロイします。
次に示すように、垂直タブコンポーネントのスタイルバリエーションを作成します
![style-variation](assets/verticaltab-style-variation.png)
css クラス _**cmp-verticaltabs--marketing**_ に関連付けられた、マーケティングというスタイルバリエーションを作成しました。
垂直タブコンポーネントを使用してアダプティブフォームを作成します。垂直タブコンポーネントをマーケティングスタイルバリエーションに関連付けます。
垂直タブにいくつかのタブを追加し、アイコンマップで定義されている画像（ホーム、ファミリーなど）に一致するように名前を付けます。
![home-icon](assets/tab-name.png)

フォームをプレビューすると、タブに関連付けられた適切なアイコンが表示されます
