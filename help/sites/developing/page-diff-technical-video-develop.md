---
title: AEM Sites でのページ差異に対応した開発
description: このビデオでは、AEM Sites のページ差異の機能にカスタムスタイルを提供する方法を説明します。
feature: Authoring
version: 6.4, 6.5
topic: Development
role: Developer
level: Beginner
doc-type: Technical Video
exl-id: 7d600b16-bbb3-4f21-ae33-4df59b1bb39d
duration: 281
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '281'
ht-degree: 100%

---

# ページの差異に対応した開発 {#developing-for-page-difference}

このビデオでは、AEM Sites のページ差異の機能にカスタムスタイルを提供する方法を説明します。

## ページ差異のスタイルのカスタマイズ {#customizing-page-difference-styles}

>[!VIDEO](https://video.tv.adobe.com/v/18871?quality=12&learn=on)

>[!NOTE]
>
>このビデオでは、カスタム CSS を we.Retail クライアントライブラリに追加しますが、この変更は、カスタマイズする人の AEM Sites プロジェクトに加える必要があります。以下のコードの例では `my-project` です。

AEM のページ差異は、`/libs/cq/gui/components/common/admin/diffservice/clientlibs/diffservice/css/htmldiff.css` の直接読み込みを介して OOTB CSS を取得します。

クライアントライブラリのカテゴリを使用するのではなく、CSS を直接読み込むため、カスタムスタイルの注入ポイントを別に見つける必要があります。このカスタム注入ポイントは、プロジェクトのオーサリング clientlib です。

これには、カスタムスタイルの上書きをテナントごとに設定にできる利点があります。

### オーサリング clientlib の準備 {#prepare-the-authoring-clientlib}

プロジェクトの `authoring` clientlib が `/apps/my-project/clientlib/authoring.` に存在することを確認します。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
        jcr:primaryType="cq:ClientLibraryFolder"
        categories="[my-project.authoring]"/>
```

### カスタム CSS の提供 {#provide-the-custom-css}

プロジェクトの `authoring` clientlib に、優先スタイルを提供する less ファイルを指す `css.txt` を追加します。[less](https://lesscss.org/) は、この例で利用されているクラスラッピングなど、多くの便利な機能があるので推奨されます。

```shell
base=./css

htmldiff.less
```

スタイルの上書きを含む `less` ファイルを `/apps/my-project/clientlibs/authoring/css/htmldiff.less` で作成し、上書きするスタイルを必要に応じて提供します。

```css
/* Wrap with body to gives these rules more specificity than the OOTB */
body {

    /* .html-XXXX are the styles that wrap text that has been added or removed */

    .html-added {
        background-color: transparent;
     color: initial;
        text-decoration: none;
     border-bottom: solid 2px limegreen;
    }

    .html-removed {
        background-color: transparent;
     color: initial;
        text-decoration: none;
     border-bottom: solid 2px red;
    }

    /* .cq-component-XXXX require !important as the class these are overriding uses it. */

    .cq-component-changed {
        border: 2px dashed #B9DAFF !important;
        border-radius: 8px;
    }
    
    .cq-component-moved {
        border: 2px solid #B9DAFF !important;
        border-radius: 8px;
    }

    .cq-component-added {
        border: 2px solid #CCEBB8 !important;
        border-radius: 8px;
    }

    .cq-component-removed {
        border: 2px solid #FFCCCC !important;
        border-radius: 8px;
    }
}
```

### ページコンポーネントを介してオーサリング clientlib CSS を含める {#include-the-authoring-clientlib-css-via-the-page-component}

スタイルが確実に読み込まれるように、プロジェクトのベースページの `/apps/my-project/components/structure/page/customheaderlibs.html` で `</head>` タグの直前にオーサリング clientlibs カテゴリを含めます。

これらのスタイルは、[!UICONTROL 編集]と[!UICONTROL プレビュー]の WCM モードに限定する必要があります。

```xml
<head>
  ...
  <sly data-sly-test="${wcmmode.preview || wcmmode.edit}" 
       data-sly-call="${clientLib.css @ categories='my-project.authoring'}"/>
</head>
```

上記のスタイルを適用した差分ページの最終結果は、次のようになります。HTML が追加され、コンポーネントが変更されています。

![ページ差異](assets/page-diff.png)

## その他のリソース {#additional-resources}

* [we.Retail サンプルサイトのダウンロード](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [AEM クライアントライブラリの使用](https://helpx.adobe.com/jp/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Less CSS ドキュメント](https://lesscss.org/)
