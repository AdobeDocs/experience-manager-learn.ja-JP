---
title: 画像プリセット
description: 画像プリセット Dynamic Media Classicでは、特定のサイズ、形式、画質およびシャープニングで画像を作成するために必要なすべての設定が含まれています。 画像プリセットは、動的サイズ設定の主要なコンポーネントです。 Dynamic Media Classicで URL を見ると、画像プリセットが使用中かどうかを簡単に確認できます。 画像プリセット、その有用性の理由、画像プリセットの作成方法について説明します。
feature: Dynamic Media Classic, Image Presets
doc-type: tutorial
topics: development, authoring, configuring
audience: all
activity: use
topic: Content Management
role: User
level: Beginner
exl-id: e472db7c-ac3f-4f66-85af-5a4c68ba609e
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '670'
ht-degree: 1%

---

# 画像プリセット {#image-presets}

画像プリセットは基本的に、特定のサイズ、形式、品質およびシャープニングで画像を作成するのに必要なすべての設定を含むレシピです。 画像プリセットは、動的サイズ設定の主要なコンポーネントです。

ほぼすべてのDynamic Media Classicの顧客の URL を見ると、おそらく使用中の画像プリセットが表示されます。 URL の末尾にある$name$を探します（名前の代わりに使用される任意の単語または単語を含む）。

画像プリセットは URL を短縮するので、要求ごとに複数の画像サービング命令を書き出す代わりに、1 つの画像プリセットを書き出すことができます。 例えば、これら 2 つの URL では同じ 300 x 300JPEG画像がシャープニング付きで生成されますが、2 番目の URL では画像プリセットが使用されます。

![画像](assets/image-presets/image-preset-2.png)

画像プリセットの真の値は、会社の管理者が画像プリセットの定義を更新し、その形式を使用するすべての画像に影響を与えることです。Web コードは変更されません。 URL のキャッシュがクリアされた後に、画像プリセットに対する変更の結果が表示されます。

>[!IMPORTANT]
>
>画像のサイズを変更する場合、画像がゆがまれないように、縦横比、画像の幅と高さの比率を常に比例させる必要があります。

画像プリセットの名前の両側にドル記号 ($) が付き、疑問符 (?) に従っています。 区切り文字.

>[!TIP]
>
>サイト上の一意の画像サイズごとに 1 つの画像プリセットを作成します。 例えば、商品の詳細ページに 350 X 350 の画像、参照/検索ページに 120 X 120 の画像、クロス販売/特集アイテムに 90 X 90 の画像が必要な場合、500 画像か 500,000 画像かに関わらず、3 つの画像プリセットが必要です。

- 詳細情報： [画像プリセット](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html).
- 方法を学ぶ [画像プリセットの作成](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html#creating-an-image-preset).

## 画像プリセットとシャープ

画像プリセットは通常、画像のサイズを変更し、元のサイズから画像のサイズを変更する場合は、シャープを追加する必要があります。 サイズを変更すると、多くのピクセルが結合されて小さなスペースにブレンドされ、画像が柔らかくぼやけて見えるからです。 シャープにより、画像内のエッジのコントラストと高いコントラスト領域のコントラストが増加します。

Dynamic Media Classicにアップロードする高解像度画像では、ズームインしたときにフルサイズで表示する場合、シャープは必要ありません。 ただし、小さいサイズでは、通常、一部のシャープニングが望ましいです。

>[!TIP]
>
>画像のサイズを変更する際は、常にシャープを適用します。 つまり、すべての画像プリセット（およびビューアプリセット）にシャープを追加する必要があります。詳しくは、後で説明します。
>
>画像の表示が良くない場合は、シャープが必要な場合や、最初は画質が悪かった場合があります。

追加するシャープの量は完全に主観的です。 柔らかい画像が好きな人もいれば、とても鋭い画像が好きな人もいます。 画像に対してシャープフィルターを組み合わせて実行すると、画像を簡単に拡張できます。 ただし、画像をオーバーボーディングして過剰にシャープにすることも簡単です。

次の図は、3 つのレベルのシャープを示しています。 右から左へはシャープはありませんが、適切な量だけで、過剰です。

![画像](assets/image-presets/image-presets-1.jpg)

Dynamic Media Classicでは、次の 3 種類のシャープニングが可能です。シンプルシャープ、再サンプルモードおよびアンシャープマスク

詳細情報： [Dynamic Media Classic Sharpening Options](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/sharpening-image.html#sharpening_an_image).

## その他のリソース

[画像プリセットガイド](https://www.adobe.com/content/dam/www/us/en/experience-manager/pdfs/dynamic-media-image-preset-guide.pdf). 画質と読み込み速度を最適化するために使用する設定。

[画像はすべて第 2 部：ぼかしに過ぎず、品質と速度](https://theblog.adobe.com/image-is-everything-part-2-its-never-just-a-blur-quality-versus-speed/). 高品質で高速読み込みが可能な画像の配信に関する画像プリセットの使用について説明するブログ投稿です。
