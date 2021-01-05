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
ht-degree: 7%

---


# コンポーネントアイコンのカスタマイズ{#developing-component-icons-in-aem-sites}

コンポーネントアイコンを使用すると、アイコンや意味のある省略形を持つコンポーネントをすばやく特定できます。 作成者は、Webエクスペリエンスを構築するのに必要なコンポーネントを以前に比べて迅速に見つけることができます。

>[!VIDEO](https://video.tv.adobe.com/v/16778/?quality=9&learn=on)

コンポーネントブラウザに一貫したグレーのテーマが表示され、次のテーマが表示されます。

* **[!UICONTROL コンポーネントグループ]**
* **[!UICONTROL コンポーネントのタイトル]**
* **[!UICONTROL コンポーネント説明]**
* **[!UICONTROL コンポーネントアイコン]**
   * コンポーネントタイトルの最初の2文字&#x200B;*（デフォルト）*
   * カスタムPNG画像&#x200B;*（開発者が設定）*
   * カスタムSVG画像&#x200B;*（開発者が設定）*
   * CoralUIアイコン&#x200B;*（開発者が設定）*

## コンポーネントアイコンの設定オプション{#component-icon-configuration-options}

### 略語{#abbreviations}

デフォルトでは、コンポーネントタイトルの最初の2文字(**[cq:Component]@jcr:title**)が省略形として使用されます。 例えば、**[cq:Component]@jcr:title=Articleリスト**&#x200B;の場合、省略形は「**Ar**」と表示されます。

省略形は、**[cq:Component]@abbreviation**&#x200B;プロパティを使用してカスタマイズできます。 この値には2文字を超える文字を使用できますが、視覚的な障害を回避するため、省略形を2文字に制限することをお勧めします。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### CoralUIアイコン{#coralui-icons}

AEMが提供するCoralUIアイコンは、コンポーネントのアイコンに使用できます。 CoralUIアイコンを設定するには、**[cq:Component]@cq:icon**&#x200B;プロパティを目的のCoralUIアイコンのHTMLアイコン属性値に設定します（[CoralUIドキュメント](https://helpx.adobe.com/jp/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)に列挙されます）。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### PNG画像{#png-images}

PNG画像は、コンポーネントのアイコンに使用できます。 PNG画像をコンポーネントアイコンとして設定するには、**[cq:Component]**&#x200B;の下に&#x200B;**cq:icon.png**&#x200B;という名前の&#x200B;**nt:file**&#x200B;として目的の画像を追加します。

PNGは、透明な背景、または背景色を&#x200B;**#707070**&#x200B;に設定する必要があります。

PNG画像は&#x200B;**20px x 20px**&#x200B;に縮小されます。 ただし、Retinaディスプレイを収容するには、**40px** by **40px**&#x200B;をお勧めします。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### SVG画像{#svg-images}

SVG画像（ベクトルベース）は、コンポーネントのアイコンに使用できます。 SVG画像をコンポーネントアイコンとして設定するには、**[cq:Component]**&#x200B;の下に&#x200B;**nt:file****cq:icon.svg**&#x200B;という名前で追加します。

SVG画像の背景色は&#x200B;**#707070**&#x200B;に、サイズは&#x200B;**20px by 20pxに設定する必要があります。**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## その他のリソース {#additional-resources}

* [使用可能なCoralUIアイコン](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
