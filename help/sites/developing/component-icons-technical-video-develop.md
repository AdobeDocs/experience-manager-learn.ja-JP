---
title: Adobe Experience Manager Sites でのコンポーネントアイコンのカスタマイズ
description: コンポーネントアイコンを使用すると、オーサーはアイコンや意味のある略語でコンポーネントをすばやく識別できます。オーサーは、web エクスペリエンスを構築するのに必要なコンポーネントを、以前よりも迅速に見つけることができるようになりました。
version: Experience Manager 6.4, Experience Manager 6.5
feature: Core Components
topic: Development
role: User
level: Intermediate
doc-type: Technical Video
exl-id: 37dc26aa-0773-4749-8c8b-4544bd4d5e5f
duration: 379
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 100%

---

# コンポーネントアイコンのカスタマイズ {#developing-component-icons-in-aem-sites}

コンポーネントアイコンを使用すると、オーサーはアイコンや意味のある略語でコンポーネントをすばやく識別できます。オーサーは、web エクスペリエンスを構築するのに必要なコンポーネントを、以前よりも迅速に見つけることができるようになりました。

>[!VIDEO](https://video.tv.adobe.com/v/16778?quality=12&learn=on)

コンポーネントブラウザーが一貫したグレーのテーマで表示され、次の項目が表示されるようになりました。

* **[!UICONTROL コンポーネントグループ]**
* **[!UICONTROL コンポーネントのタイトル]**
* **[!UICONTROL コンポーネント説明]**
* **[!UICONTROL コンポーネントアイコン]**
   * コンポーネントタイトルの最初の 2 文字&#x200B;*（デフォルト）*
   * カスタム PNG 画像&#x200B;*（開発者が設定）*
   * カスタム SVG 画像&#x200B;*（開発者が設定）*
   * CoralUI アイコン&#x200B;*（開発者が設定）*

## コンポーネントアイコンの設定オプション {#component-icon-configuration-options}

### 略語 {#abbreviations}

デフォルトでは、コンポーネントタイトルの最初の 2 文字（**[cq:Component]@jcr:title**）が略語として使用されます。例えば、**[cq:Component]@jcr:title=Article List** の場合、略語は「**Ar**」と表示されます。

略語は、**[cq:Component]@abbreviation** プロパティでカスタマイズできます。この値は 2 文字より長くすることもできますが、視覚的な障害を避けるために、略語は 2 文字に制限することをお勧めします。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### CoralUI アイコン {#coralui-icons}

AEM が提供する CoralUI アイコンは、コンポーネントアイコンに使用できます。CoralUI アイコンを設定するには、**[cq:Component]@cq:icon** プロパティを目的の CoralUI アイコンの HTML アイコン属性値に設定します（[CoralUI ドキュメント](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)に列挙されています）。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### PNG 画像 {#png-images}

PNG 画像は、コンポーネントアイコンに使用できます。PNG 画像をコンポーネントアイコンとして設定するには、目的の画像を **cq:icon.png** という名前の **nt:file** として **[cq:Component]** に追加します。

PNG の背景は透明にするか、背景色を **#707070** に設定する必要があります。

PNG 画像は、**20ピクセル x 20ピクセル**&#x200B;にサイズが調整されます。ただし、Retina ディスプレイに対応するには、**40ピクセル** x **40ピクセル**&#x200B;が望ましい場合があります。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### SVG 画像 {#svg-images}

SVG 画像（ベクトルベース）は、コンポーネントアイコンに使用できます。SVG 画像をコンポーネントアイコンとして設定するには、目的の SVG を **nt:file** という名前の **cq:icon.svg** として **[cq:Component]** に追加します。

SVG 画像の背景色は **#707070** に設定し、サイズは **20ピクセル x 20ピクセル**&#x200B;にする必要があります。

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## その他のリソース {#additional-resources}

* [使用可能な CoralUI アイコン](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
