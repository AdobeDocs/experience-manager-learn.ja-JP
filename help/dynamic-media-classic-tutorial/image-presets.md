---
title: 画像プリセット
description: 画像プリセット Dynamic Media Classicでは、特定のサイズ、形式、画質、シャープニングで画像を作成するのに必要なすべての設定が含まれています。 画像プリセットは、動的なサイズ設定の主要なコンポーネントです。 Dynamic Media ClassicのURLを見ると、画像プリセットが使用中かどうかを簡単に確認できます。 画像プリセットの概要、それが非常に役に立つ理由および画像プリセットの作成方法について説明します。
sub-product: dynamic-media
feature: Dynamic Media Classic、画像プリセット
doc-type: tutorial
topics: development, authoring, configuring
audience: all
activity: use
topic: コンテンツ管理
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '712'
ht-degree: 1%

---


# 画像プリセット {#image-presets}

画像プリセットは基本的に、特定のサイズ、形式、画質およびシャープニングで画像を作成するのに必要なすべての設定を含むレシピです。 画像プリセットは、動的なサイズ設定の主要なコンポーネントです。

約Dynamic Media Classicの顧客のURLを見ると、おそらく使用中の画像プリセットが表示されます。 URLの末尾にある$name$を探します（任意の単語または単語がnameに置き換えられます）。

画像プリセットはURLを短縮するので、要求ごとに複数の画像サービングの指示を書き出す代わりに、1つの画像プリセットを書き出すことができます。 例えば、次の2つのURLは、シャープが適用された同じ300 x 300 JPEG画像を生成しますが、2つ目のURLは画像プリセットを使用します。

![画像](assets/image-presets/image-preset-2.png)

画像プリセットの真の値は、会社の管理者が画像プリセットの定義を更新し、その形式を使用するすべての画像に影響を与えることです。Webコードは変更されません。 URLのキャッシュをクリアした後、画像プリセットに対する変更の結果が表示されます。

>[!IMPORTANT]
>
>画像のサイズを変更する場合、画像が歪まないように、縦横比、画像の幅と高さの比率を常に比例させる必要があります。

画像プリセットの名前の両側にドル記号($)が付き、疑問符(?)の後に 区切り文字.

>[!TIP]
>
>サイト上の一意の画像サイズごとに1つの画像プリセットを作成します。 例えば、商品の詳細ページに350 X 350の画像、閲覧/検索ページに120 X 120の画像、クロス販売/特集アイテムに90 X 90の画像が必要な場合、500画像か500,000画像かに関わらず、3つの画像プリセットが必要です。

- [画像プリセット](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sizing/setting-image-presets.html)の詳細をご覧ください。
- [画像プリセットを作成する方法](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sizing/setting-image-presets.html#creating-an-image-preset)を説明します。

## 画像プリセットとシャープ

画像プリセットは通常、画像のサイズを変更し、元のサイズから画像のサイズを変更する際は、シャープを追加する必要があります。 サイズを変更すると、多くのピクセルが結合されて小さなスペースにブレンドされ、画像がぼやけた状態に見えるからです。 シャープにより、画像内のエッジと高いコントラストの領域のコントラストが増加します。

Dynamic Media Classicにアップロードする高解像度画像は、ズームインするときにフルサイズで表示する場合に、シャープニングを必要としないことが予想されます。 ただし、サイズが小さい場合は、通常、一部のシャープが望ましいです。

>[!TIP]
>
>画像のサイズを変更する場合は常にシャープにします。 つまり、すべての画像プリセット（およびビューアプリセット）にシャープを追加する必要があります。詳しくは、後で説明します。
>
>画像が良く見えない場合は、シャープが必要か、または最初に画質が悪かった可能性があります。

追加するシャープの量は完全に主観的です。 柔らかい画像が好きな人もいれば、とてもシャープな画像が好きな人もいます。 画像に対してシャープフィルターを組み合わせて実行することで、画像を簡単に拡張できます。 ただし、画像をオーバーボードしたり、過剰にシャープにしたりするのも簡単です。

次の図は、3つのレベルのシャープを示しています。 右から左へはシャープはなく、ちょうど適切な量、そして多すぎる。

![画像](assets/image-presets/image-presets-1.jpg)

Dynamic Media Classicでは、次の3種類のシャープニングが可能です。シンプルシャープ、再サンプルモードおよびアンシャープマスク

[Dynamic Media Classicのシャープニングオプション](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/master-files/sharpening-image.html#sharpening_an_image)の詳細をご覧ください。

## その他のリソース

[画像プリセットガイド](https://www.adobe.com/content/dam/www/us/en/experience-manager/pdfs/dynamic-media-image-preset-guide.pdf)を参照してください。画質と読み込み速度を最適化するために使用する設定。

[画像はすべてパート2:ただのぼやけではありません。画質と速度](https://theblog.adobe.com/image-is-everything-part-2-its-never-just-a-blur-quality-versus-speed/)。高品質で高速な読み込みをおこなうための画像プリセットの使用について説明するブログ投稿です。

[画像がウェビナーです](https://dynamicmediaseries2019.enterprise.adobeevents.com/)。_Image Is Everything_&#x200B;シリーズの3つのウェビナーの録画へのリンク。 [ウェビナー2](https://seminars.adobeconnect.com/p6lqaotpjnd3) で画像プリセットについて説明しています。
