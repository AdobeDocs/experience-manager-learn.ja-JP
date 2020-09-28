---
title: AEM Sitesのページ差向け開発
description: このビデオでは、AEMサイトのページの違い機能にカスタムスタイルを使用する方法を示します。
feature: page-diff
topics: development
audience: developer
doc-type: technical video
activity: develop
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 6%

---


# ページの差異に対する開発 {#developing-for-page-difference}

このビデオでは、AEMサイトのページの違い機能にカスタムスタイルを使用する方法を示します。

## ページの差異スタイルのカスタマイズ {#customizing-page-difference-styles}

>[!VIDEO](https://video.tv.adobe.com/v/18871/?quality=9&learn=on)

>[!NOTE]
>
>このビデオでは、カスタムCSSをwe.Retailクライアントライブラリに追加します。このライブラリでは、カスタマイザのAEM Sitesプロジェクトに対してこれらの変更を行う必要があります。次のコード例では、 `my-project`.

AEMのページ差は、の直接読み込みを介してOOTB CSSを取得 `/libs/cq/gui/components/common/admin/diffservice/clientlibs/diffservice/css/htmldiff.css`します。

クライアントライブラリカテゴリを使用する代わりにCSSを直接読み込むので、カスタムスタイルの新しい注入ポイントを見つける必要があります。このカスタム注入ポイントは、プロジェクトのオーサリングclientlibです。

これは、これらのカスタムスタイルの上書きをテナント固有にするメリットを持ちます。

### オーサリングclientlibの準備 {#prepare-the-authoring-clientlib}

プロジェクトの `authoring` clientlibが `/apps/my-project/clientlib/authoring.`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
        jcr:primaryType="cq:ClientLibraryFolder"
        categories="[my-project.authoring]"/>
```

### カスタムCSSの指定 {#provide-the-custom-css}

を追加プロジェクトのclientlibに対して、上書きスタイルを提供するLESSファイルを指し示す。 `authoring``css.txt` [便利な機能が多数あるので](https://lesscss.org/) 、LESS（以下の例で使用するクラスラップを含む）が望ましくありません。

```shell
base=./css

htmldiff.less
```

でスタイルの上書きを含む `less` ファイルを作成し、必要に応じてオーバーリングスタイルを指定し `/apps/my-project/clientlibs/authoring/css/htmldiff.less`ます。

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

### ページコンポーネントを介して、オーサリングclientlib CSSを含める {#include-the-authoring-clientlib-css-via-the-page-component}

オーサリングclientlibsカテゴリをプロジェクトのベースページのタグの `/apps/my-project/components/structure/page/customheaderlibs.html` 直前に含め、スタイルが確実に読み込まれるよう `</head>` にします。

これらのスタイルは、 [!UICONTROL 編集] WCMモードと [!UICONTROL プレビュー] WCMモードに制限する必要があります。

```xml
<head>
  ...
  <sly data-sly-test="${wcmmode.preview || wcmmode.edit}" 
       data-sly-call="${clientLib.css @ categories='my-project.authoring'}"/>
</head>
```

上記のスタイルが適用されたdiff&#39;dページの最終結果は、次のようになります（HTMLが追加され、コンポーネントが変更されました）。

![ページの差異](assets/page-diff.png)

## その他のリソース {#additional-resources}

* [we.Retailサンプルサイトのダウンロード](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [AEMクライアントライブラリの使用](https://helpx.adobe.com/jp/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [CSSに関するドキュメントを減らす](https://lesscss.org/)
