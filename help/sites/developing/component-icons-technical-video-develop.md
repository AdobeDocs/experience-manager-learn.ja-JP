---
title: Adobe Experience Manager Sitesでのコンポーネントアイコンのカスタマイズ
description: コンポーネントアイコンを使用すると、作成者はアイコンや意味のある略語でコンポーネントをすばやく識別できます。 作成者は、Web エクスペリエンスを構築するのに必要なコンポーネントを、以前よりも迅速に見つけることができるようになりました。
topics: components
audience: administrator, developer
doc-type: technical video
activity: develop
version: 6.4, 6.5
feature: Core Components
topic: Development
role: User
level: Intermediate
exl-id: 37dc26aa-0773-4749-8c8b-4544bd4d5e5f
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '375'
ht-degree: 8%

---

# コンポーネントアイコンのカスタマイズ {#developing-component-icons-in-aem-sites}

コンポーネントアイコンを使用すると、作成者はアイコンや意味のある略語でコンポーネントをすばやく識別できます。 作成者は、Web エクスペリエンスを構築するのに必要なコンポーネントを、以前よりも迅速に見つけることができるようになりました。

>[!VIDEO](https://video.tv.adobe.com/v/16778/?quality=9&learn=on)

コンポーネントブラウザーが一貫したグレーのテーマで表示され、次の項目が表示されるようになりました。

* **[!UICONTROL コンポーネントグループ]**
* **[!UICONTROL コンポーネントのタイトル]**
* **[!UICONTROL コンポーネント説明]**
* **[!UICONTROL コンポーネントアイコン]**
   * コンポーネントタイトルの最初の 2 文字 *（デフォルト）*
   * カスタム PNG 画像 *（開発者が設定）*
   * カスタムSVG画像 *（開発者が設定）*
   * CoralUI アイコン *（開発者が設定）*

## コンポーネントアイコンの設定オプション {#component-icon-configuration-options}

### 略語 {#abbreviations}

デフォルトでは、コンポーネントタイトルの最初の 2 文字 (**[cq:Component]@jcr:title**) は省略形として使用されます。 例えば、 **[cq:Component]@jcr:title=記事リスト** 省略形は、「**Ar**&quot;.

省略形は、 **[cq:Component]@abbreviation** プロパティ。 この値は 2 文字を超える文字を受け取ることができますが、視覚的な障害を避けるために、省略形を 2 文字に制限することをお勧めします。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### CoralUI アイコン {#coralui-icons}

AEMが提供する CoralUI アイコンは、コンポーネントアイコンに使用できます。 CoralUI アイコンを設定するには、 **[cq:Component]@cq:icon** プロパティを CoralUI アイコンのHTMLアイコン属性値に追加します ( [CoralUI ドキュメント](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html).

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### PNG 画像 {#png-images}

PNG 画像はコンポーネントアイコンに使用できます。 PNG 画像をコンポーネントアイコンとして設定するには、目的の画像を **nt:file** 名前付き **cq:icon.png** の下に **[cq:Component]**.

PNG は透明の背景にするか、背景色をに設定する必要があります。 **#707070**.

PNG 画像は次のサイズに合わせて拡大縮小されます **20px by 20px**. ただし、Retina ディスプレイに対応するため **40px** 作成者 **40px** 好ましいのは

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### SVG画像 {#svg-images}

SVG画像（ベクトルベース）は、コンポーネントアイコンに使用できます。 SVG画像をコンポーネントアイコンとして設定するには、目的のSVGを **nt:file** 名前付き **cq:icon.svg** の下に **[cq:Component]**.

SVG画像の背景色は次のように設定する必要があります **#707070** そして **20px x 20px.**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## その他のリソース {#additional-resources}

* [使用可能な CoralUI アイコン](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
