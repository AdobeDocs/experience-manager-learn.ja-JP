---
title: Adobe Experience Manager Sitesのコンポーネントアイコンのカスタマイズ
description: コンポーネントアイコンを使用すると、アイコンや意味のある省略形を持つコンポーネントをすばやく特定できます。 作成者は、Webエクスペリエンスを構築するのに必要なコンポーネントを以前に比べて迅速に見つけることができます。
topics: components
audience: administrator, developer
doc-type: technical video
activity: develop
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: c85a59a8bd180d5affe2a5bf5939dabfb2776d73
workflow-type: tm+mt
source-wordcount: '375'
ht-degree: 2%

---


# コンポーネントアイコンのカスタマイズ {#developing-component-icons-in-aem-sites}

コンポーネントアイコンを使用すると、アイコンや意味のある省略形を持つコンポーネントをすばやく特定できます。 作成者は、Webエクスペリエンスを構築するのに必要なコンポーネントを以前に比べて迅速に見つけることができます。

>[!VIDEO](https://video.tv.adobe.com/v/16778/?quality=9&learn=on)

コンポーネントブラウザに一貫したグレーのテーマが表示され、次のテーマが表示されます。

* **[!UICONTROL コンポーネントグループ]**
* **[!UICONTROL コンポーネントのタイトル]**
* **[!UICONTROL コンポーネント説明]**
* **[!UICONTROL コンポーネントアイコン]**
   * コンポーネントタイトルの最初の2文字 *（デフォルト）*
   * カスタムPNG画像 *（開発者が設定）*
   * カスタムSVG画像 *（開発者が設定）*
   * CoralUIアイコン *（開発者が設定）*

## コンポーネントアイコンの設定オプション {#component-icon-configuration-options}

### 省略形 {#abbreviations}

デフォルトでは、コンポーネントタイトルの最初の2文字(**[cq:Component]@jcr:title**)が省略形として使用されます。 例えば、 **[cq:Component]@jcr:title=Articleリストの場合** 、省略形は「**Ar**」と表示されます。

省略形は、 **[cq:Component]@abbreviation** プロパティを使用してカスタマイズできます。 この値には2文字を超える文字を使用できますが、視覚的な障害を回避するため、省略形を2文字に制限することをお勧めします。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### CoralUIアイコン {#coralui-icons}

AEMが提供するCoralUIアイコンは、コンポーネントのアイコンに使用できます。 CoralUIアイコンを設定するには、 **[cq:Component]@cq:icon** プロパティを目的のCoralUIアイコンのHTMLアイコン属性値に設定します( [CoralUIドキュメントに列挙されます](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html))。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### PNG画像 {#png-images}

PNG画像は、コンポーネントのアイコンに使用できます。 PNG画像をコンポーネントアイコンとして設定するには、目的の画像を **cq:Component** の下にcq:icon.png ******[]**&#x200B;という名前のnt:fileとして追加します。

PNGは、透明な背景、または **#707070に設定された背景色にする必要があります**。

PNG画像は20px x 20pxに拡大 **されます**。 ただし、Retinaディスプレイを収容するには、40px **x 40px****** を使用することをお勧めします。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### SVG画像 {#svg-images}

SVG画像（ベクトルベース）は、コンポーネントのアイコンに使用できます。 SVG画像をコンポーネントアイコンとして設定するには、目的のSVGを、 **cq:Component** の下に **cq:icon.svg****[]**&#x200B;という名前のnt:fileとして追加します。

SVG画像の背景色は **#707070** 、サイズは20px x 20pxに設定する必要があり **ます。**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## その他のリソース {#additional-resources}

* [使用可能なCoralUIアイコン](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
