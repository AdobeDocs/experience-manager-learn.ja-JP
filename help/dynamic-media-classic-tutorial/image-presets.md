---
title: 画像プリセット
description: 画像プリセット Dynamic Mediaクラシックには、特定のサイズ、形式、画質およびシャープで画像を作成するのに必要なすべての設定が含まれています。 画像プリセットは、動的サイズ変更の主要コンポーネントです。 Dynamic MediaクラシックでURLを見ると、画像プリセットが使用されているかどうかを簡単に確認できます。 画像プリセットの概要、その用途、画像プリセットの作成方法について説明します。
sub-product: dynamic-media
feature: Dynamic Mediaクラシック、画像プリセット
doc-type: tutorial
topics: development, authoring, configuring
audience: all
activity: use
topic: コンテンツ管理
role: 開業医
level: 初心者
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '715'
ht-degree: 1%

---


# 画像プリセット {#image-presets}

画像プリセットは、基本的には、特定のサイズ、形式、画質およびシャープで画像を作成するのに必要なすべての設定を含むレシピです。 画像プリセットは、動的サイズ変更の主要コンポーネントです。

ほんのDynamic Mediaクラシックのお客様のURLを見ると、おそらく使用中の画像プリセットが表示されます。 URLの末尾で$name$を探します（nameの代わりに使用する単語や単語を含む）。

画像プリセットはURLを短縮するので、要求ごとに複数の画像サービング命令を書き込む代わりに、1つの画像プリセットを書き込むことができます。 例えば、次の2つのURLはシャープの適用時に同じ300 x 300のJPEG画像を生成しますが、2つ目のURLは画像プリセットを使用します。

![画像](assets/image-presets/image-preset-2.png)

画像プリセットの真の値は、会社管理者がWebコードを変更せずに、その画像プリセットの定義を更新し、その形式を使用するすべての画像に影響を与えることです。 URLのキャッシュをクリアすると、画像プリセットに対する変更の結果が表示されます。

>[!IMPORTANT]
>
>画像のサイズを変更する場合、画像の縦横比、および画像の幅と高さの比率は、画像がゆがまないように常に比例します。

画像プリセットの名前の両側にドル記号($)が付き、疑問符(?)の後に続きます。 区切り文字.

>[!TIP]
>
>サイト上の固有の画像サイズごとに1つの画像プリセットを作成します。 例えば、商品詳細ページに350 X 350の画像、ブラウズ/検索ページに120 X 120の画像、クロス販売/特集アイテムに90 X 90の画像が必要な場合、500画像か500,000画像かにかかわらず、3つの画像プリセットが必要です。

- [画像プリセット](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sizing/setting-image-presets.html)についての詳細。
- [画像プリセット](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sizing/setting-image-presets.html#creating-an-image-preset)の作成方法を説明します。

## 画像プリセットとシャープの適用

画像プリセットは通常、画像のサイズを変更します。画像のサイズを元のサイズから変更する場合は、シャープを追加する必要があります。 サイズ変更を行うと、多くのピクセルが合成されて小さなスペースにブレンドされ、画像がぼやけて見えるからです。 シャープを適用すると、画像内のエッジのコントラストが大きくなり、コントラストの大きい領域が強調されます。

Dynamic Mediaクラシックにアップロードする高解像度画像を拡大表示する場合、フルサイズで表示するときは、シャープを適用する必要はありません。 ただし、サイズを小さくする場合は、通常、ある程度のシャープが適しています。

>[!TIP]
>
>画像のサイズを変更する場合は常にシャープを適用 つまり、すべての画像プリセット（およびビューアプリセット）にシャープを追加する必要があります。後で説明します。
>
>画像の表示が良くない場合は、シャープが必要か、最初に画質が悪かった可能性があります。

追加するシャープの適用量は必ずしも主観的です。 柔らかい画像が好きな人もいれば、とてもシャープな画像が好きな人もいます。 画像に対してシャープの適用フィルターを組み合わせて実行すると、画像を簡単に強調できます。 ただし、画像をオーバーボードにしたり、画像にシャープを適用しすぎたりするのも簡単です。

次の図に、3レベルのシャープを示します。 右から左にシャープは適用されず、適切な量だけで、過度に適用されます。

![画像](assets/image-presets/image-presets-1.jpg)

Dynamic Mediaクラシックでは、次の3種類のシャープを適用できます。シンプルなシャープ、再サンプルモード、アンシャープマスク

[Dynamic Mediaクラシックシャープの適用オプション](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/master-files/sharpening-image.html#sharpening_an_image)の詳細を表示します。

## その他のリソース

[画像プリセットガイド](https://www.adobe.com/content/dam/www/us/en/experience-manager/pdfs/dynamic-media-image-preset-guide.pdf)。画質と読み込み速度の最適化に使用する設定です。

[ImageはすべてPart 2:ただのぼかしではありません品質と速度](https://theblog.adobe.com/image-is-everything-part-2-its-never-just-a-blur-quality-versus-speed/)。高品質で高速読み込みが可能な画像プリセットの使用について、ブログの投稿です。

[イメージはすべてウェビナー](https://dynamicmediaseries2019.enterprise.adobeevents.com/)。_Image Is Everything_&#x200B;シリーズの3つのウェビナーすべての録画へのリンクです。 [ウェビナー2では、画像プリセットにつ](https://seminars.adobeconnect.com/p6lqaotpjnd3) いて説明します。
