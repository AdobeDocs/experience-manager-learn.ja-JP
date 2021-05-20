---
title: Adobe Experience Manager Sitesのコンポーネントアイコンのカスタマイズ
description: コンポーネントアイコンを使用すると、作成者は、アイコンや意味のある省略形でコンポーネントをすばやく識別できます。 作成者は、Webエクスペリエンスを構築するのに必要なコンポーネントを、以前よりも迅速に見つけることができるようになりました。
topics: components
audience: administrator, developer
doc-type: technical video
activity: develop
version: 6.3, 6.4, 6.5
feature: コアコンポーネント
topic: 開発
role: Business Practitioner
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 8%

---


# コンポーネントアイコンのカスタマイズ{#developing-component-icons-in-aem-sites}

コンポーネントアイコンを使用すると、作成者は、アイコンや意味のある省略形でコンポーネントをすばやく識別できます。 作成者は、Webエクスペリエンスを構築するのに必要なコンポーネントを、以前よりも迅速に見つけることができるようになりました。

>[!VIDEO](https://video.tv.adobe.com/v/16778/?quality=9&learn=on)

コンポーネントブラウザーが一貫したグレーのテーマで表示され、次の項目が表示されます。

* **[!UICONTROL コンポーネントグループ]**
* **[!UICONTROL コンポーネントのタイトル]**
* **[!UICONTROL コンポーネント説明]**
* **[!UICONTROL コンポーネントアイコン]**
   * コンポーネントタイトルの最初の2文字&#x200B;*（デフォルト）*
   * カスタムPNG画像&#x200B;*（開発者が設定）*
   * カスタムSVG画像&#x200B;*（開発者が設定）*
   * CoralUIアイコン&#x200B;*（開発者が設定）*

## コンポーネントアイコン設定オプション{#component-icon-configuration-options}

### 略語 {#abbreviations}

デフォルトでは、コンポーネントタイトルの最初の2文字(**[cq:Component]@jcr:title**)が省略形として使用されます。 例えば、 **[cq:Component]@jcr:title=Article List**&#x200B;の場合、省略形は「**Ar**」と表示されます。

省略形は、 **[cq:Component]@abbreviation**&#x200B;プロパティを使用してカスタマイズできます。 この値は2文字を超える場合がありますが、視覚的な障害を避けるために、省略形を2文字に制限することをお勧めします。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### CoralUIアイコン{#coralui-icons}

AEMが提供するCoralUIアイコンは、コンポーネントアイコンに使用できます。 CoralUIアイコンを設定するには、**[cq:Component]@cq:icon**&#x200B;プロパティを目的のCoralUIアイコンのHTMLアイコン属性値に設定します（[CoralUIのドキュメント](https://helpx.adobe.com/jp/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)に列挙されています）。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### PNG画像{#png-images}

コンポーネントアイコンにはPNG画像を使用できます。 PNG画像をコンポーネントアイコンとして設定するには、**[cq:Component]**&#x200B;の下に&#x200B;**nt:file**&#x200B;という名前の&#x200B;**cq:icon.png**&#x200B;を付けて、目的の画像を追加します。

PNGは、透明な背景にするか、背景色を&#x200B;**#707070**&#x200B;に設定する必要があります。

PNG画像は、**20pxに合わせて20px**&#x200B;拡大/縮小されます。 ただし、Retinaディスプレイ&#x200B;**40px**&#x200B;を&#x200B;**40px**&#x200B;に収めるのが望ましい場合があります。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### SVG画像{#svg-images}

SVG画像（ベクトルベース）はコンポーネントアイコンに使用できます。 SVG画像をコンポーネントアイコンとして設定するには、**[cq:Component]**&#x200B;の下に&#x200B;**nt:file**&#x200B;として&#x200B;**cq:icon.svg**&#x200B;を追加します。

SVG画像の背景色は&#x200B;**#707070**&#x200B;に、サイズは&#x200B;**20px x 20pxに設定する必要があります。**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## その他のリソース {#additional-resources}

* [使用可能なCoralUIアイコン](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
