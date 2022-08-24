---
title: AEM Sitesでのページの違いに対する開発
description: このビデオでは、AEM Sites のページの違いの機能にカスタムスタイルを提供する方法を説明します。
feature: Authoring
topics: development
audience: developer
doc-type: technical video
activity: develop
version: 6.4, 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 7d600b16-bbb3-4f21-ae33-4df59b1bb39d
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 6%

---

# ページ差のための開発 {#developing-for-page-difference}

このビデオでは、AEM Sites のページの違いの機能にカスタムスタイルを提供する方法を説明します。

## ページの差異スタイルのカスタマイズ {#customizing-page-difference-styles}

>[!VIDEO](https://video.tv.adobe.com/v/18871/?quality=9&learn=on)

>[!NOTE]
>
>このビデオでは、カスタム CSS を we.Retail クライアントライブラリに追加します。このライブラリでは、これらの変更をカスタマイズ者のAEM Sitesプロジェクトに加える必要があります。以下のコード例： `my-project`.

AEMのページの違いは、 `/libs/cq/gui/components/common/admin/diffservice/clientlibs/diffservice/css/htmldiff.css`.

クライアントライブラリカテゴリを使用するのではなく、CSS のこの直接読み込みのため、カスタムスタイルの別の挿入ポイントを見つける必要があります。このカスタム挿入ポイントは、プロジェクトのオーサリング clientlib です。

これにより、これらのカスタムスタイルの上書きをテナント固有にすることができます。

### オーサリングクライアントライブラリの準備 {#prepare-the-authoring-clientlib}

の存在を確認する `authoring` プロジェクトの clientlib( ) `/apps/my-project/clientlib/authoring.`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
        jcr:primaryType="cq:ClientLibraryFolder"
        categories="[my-project.authoring]"/>
```

### カスタム CSS の提供 {#provide-the-custom-css}

プロジェクトの `authoring` clientlib a `css.txt` これは、上書きスタイルを提供する less ファイルを指しています。 [低](https://lesscss.org/) は、この例で利用されるクラスラッピングを含む多くの便利な機能により推奨されます。

```shell
base=./css

htmldiff.less
```

を作成します。 `less` 次の場所にあるスタイルオーバーライドを含むファイル `/apps/my-project/clientlibs/authoring/css/htmldiff.less`を使用し、必要に応じてオーバーリングスタイルを指定します。

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

オーサリング clientlibs カテゴリをプロジェクトのベースページの `/apps/my-project/components/structure/page/customheaderlibs.html` 直前 `</head>` タグを使用して、スタイルが読み込まれるようにします。

これらのスタイルは、次に限定する必要があります。 [!UICONTROL 編集] および [!UICONTROL プレビュー] WCM モード。

```xml
<head>
  ...
  <sly data-sly-test="${wcmmode.preview || wcmmode.edit}" 
       data-sly-call="${clientLib.css @ categories='my-project.authoring'}"/>
</head>
```

上記のスタイルが適用された差分ページの最終結果は、次のようになります (HTMLが追加され、コンポーネントが変更されました )。

![ページの差異](assets/page-diff.png)

## その他のリソース {#additional-resources}

* [we.Retail サンプルサイトのダウンロード](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [AEMクライアントライブラリの使用](https://helpx.adobe.com/jp/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [CSS ドキュメントの削減](https://lesscss.org/)
