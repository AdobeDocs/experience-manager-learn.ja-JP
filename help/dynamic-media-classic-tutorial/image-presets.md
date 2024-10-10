---
title: 画像プリセット
description: Dynamic Media Classic の画像プリセットには、特定のサイズ、形式、画質およびシャープニングで画像を作成するために必要なすべての設定が含まれています。画像プリセットは、動的サイズ設定の主要なコンポーネントです。 Dynamic Media Classic で URL を見ると、画像プリセットが使用されているかどうかを簡単に確認できます。画像プリセットと、それが役に立つ理由およびその作成方法について説明します。
feature: Dynamic Media Classic, Image Presets
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: e472db7c-ac3f-4f66-85af-5a4c68ba609e
duration: 127
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '648'
ht-degree: 100%

---

# 画像プリセット {#image-presets}

画像プリセットは基本的に、特定のサイズ、形式、画質およびシャープニングで画像を作成するために必要なすべての設定を含むレシピです。画像プリセットは、動的サイズ設定の主要なコンポーネントです。

Dynamic Media Classic のほぼすべての顧客の URL を見ると、おそらく画像プリセットが使用されていることがわかります。URL の末尾にある $name$ を探してください（name は任意の単語に置換されます）。

画像プリセットは URL を短縮するので、リクエストごとに複数の画像サービング命令を書き出す代わりに、単一の画像プリセットを書き出すことができます。例えば、次の 2 つの URL ではシャープニングされた同じ 300 x 300 JPEG 画像を生成しますが、2 番目の URL では画像プリセットを使用します。

![画像](assets/image-presets/image-preset-2.png)

画像プリセットの真の価値は、会社の管理者がその画像プリセットの定義を更新し、その形式を使用するすべての画像に影響を与えることができることです。Web コードを変更する必要はありません。URL のキャッシュがクリアされると、画像プリセットに対する変更の結果が表示されます。

>[!IMPORTANT]
>
>画像のサイズを変更するときは、画像が歪まないように、縦横比（画像の幅と高さの比率）を常に比例させる必要があります。

画像プリセットには、名前の両側にドル記号（$）があり、その後に疑問符（?）の区切り文字が続きます。

>[!TIP]
>
>サイトの一意の画像サイズごとに 1 つの画像プリセットを作成します。例えば、製品の詳細ページに 350 X 350 画像、参照／検索ページに 120 X 120 画像、クロスセル／お勧めアイテムに 90 X 90 画像が必要な場合、画像が 500 でも 500,000 あっても関係なく、3 つの画像プリセットが必要です。

- 詳しくは、[画像プリセット](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html?lang=ja)を参照してください。
- 方法について詳しくは、[画像プリセットの作成](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sizing/setting-image-presets.html?lang=ja#creating-an-image-preset)を参照してください。

## 画像プリセットとシャープニング

画像プリセットは通常、画像のサイズを変更します。元のサイズから画像のサイズを変更する場合はいつでも、シャープニングを追加する必要があります。これは、サイズを変更すると、多くのピクセルが結合して小さなスペースにブレンドし、画像が柔らかくぼやけて見えるからです。シャープニングにより、画像のエッジとコントラストの高い領域のコントラストが高まります。

Dynamic Media Classic にアップロードする高解像度の画像は、ズームインしてフルサイズで表示する場合、シャープニングする必要はないと考えられます。ただし、サイズが小さい場合は、通常、ある程度のシャープニングが必要です。

>[!TIP]
>
>画像のサイズを変更する際は、常にシャープを適用します。つまり、すべての画像プリセット（および後で説明するビューアプリセット）にシャープニングを追加する必要があります。
>
>画像の表示が良くない場合は、シャープニングする必要があるか、最初から画質が低かった可能性があります。

どの程度のシャープニングを追加するかは、完全に主観的なものです。より柔らかい画像が好きな人物もいれば、非常にシャープな画像が好きな人物もいます。画像に対してシャープニングフィルターを組み合わせて実行すると、画像を簡単に強調できます。ただし、度を超えて画像を過剰なシャープにすることも簡単です。

次の図に、3 つのレベルのシャープニングを示します。右から左へ、シャープニングなし、適切なシャープニング、過剰なシャープニングとなっています。

![画像](assets/image-presets/image-presets-1.jpg)

Dynamic Media Classic では、シンプルシャープニング、リサンプルモードおよびアンシャープマスクの 3 種類のシャープニングが可能です。

詳しくは、[Dynamic Media Classic シャープニングオプション](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/master-files/sharpening-image.html?lang=ja#sharpening_an_image)を参照してください。

## その他のリソース

[画像プリセットガイド](https://www.adobe.com/content/dam/www/us/en/experience-manager/pdfs/dynamic-media-image-preset-guide.pdf)。画質と読み込み速度を最適化するために使用する設定。

[画像がすべて（パート 2）：単なるぼかしではありません - 画質と速度](https://theblog.adobe.com/image-is-everything-part-2-its-never-just-a-blur-quality-versus-speed/)。高画質で高速に読み込まれる画像を配信するための画像プリセットの使用について説明しているブログ投稿。
