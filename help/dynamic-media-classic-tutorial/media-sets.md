---
title: 画像、スウォッチ、スピンおよび混在メディアセット
description: Dynamic Media Classic の最も役に立つ強力な機能の 1 つは、画像、スウォッチ、スピン、混在メディアセットなどのリッチメディアセットの作成をサポートしている点です。各リッチメディアセットの概要と、Dynamic Media Classic で各タイプを作成する方法について説明します。次に、アップロード時にリッチメディアセットを作成するプロセスを自動化するバッチセットプリセットについて詳しく説明します。
feature: Dynamic Media Classic, Image Sets, Mixed Media Sets, Spin Sets
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: 45c86ff2-d991-46a7-a8d1-25c9fec142d9
duration: 347
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '1359'
ht-degree: 100%

---

# 画像、スウォッチ、スピンおよび混在メディアセット {#media-sets}

動的なサイズ変更やズームのために 1 つの画像だけでなく、Dynamic Media Classic のコレクションを設定することで、より豊富なオンラインエクスペリエンスを実現します。このセクションでは、Dynamic Media Classic で次のリッチメディアセットを作成する方法について説明します。

- 画像セット
- スウォッチセット
- スピンセット
- 混在メディアセット

また、バッチセットプリセットを使用して、アップロードを使用してセット作成を自動化する方法についても説明します。

## セットに関して常に知りたかったすべて

基本的な動的サイズ設定とズームの次に、おそらく最も広く使用されている Dynamic Media Classic のサブ製品はセットです。セットとは、基本的に、実際の画像を含まず、他の画像やビデオとの関係のセットで構成される「仮想」アセットです。セットの主な魅力は、それらが「既製」のミニアプリケーションであることです。つまり、設定された各ビューアーには独自のロジックとインターフェイスが含まれるので、サイト上で呼び出すだけで済みます。また、追跡が必要なのはセットごとに 1 つのアセット ID だけです。すべてのメンバーアセットと関係を自分で管理する必要はありません。

セットを作成する際、そのセットは別のアセットとして管理され、URL から提供する前に公開用にマークして公開する必要があります。すべてのメンバーアセットも公開する必要があります。

### セットの種類

Dynamic Media Classic で作成できる 4 種類のセット（画像、スウォッチ、スピン、混在メディアセット）について説明します。

## 画像セット

これは最も一般的なタイプのセットです。通常は、同じアイテムの代替ビューに使用します。この画像は、関連する画像のサムネールをクリックして、ビューアーに読み込む複数の画像で構成されます。

![画像](assets/media-sets/image-set-1.jpg)

_画像セットの例_

上記の画像セットの URL は次のように表示されます。

![画像](assets/media-sets/image-set-url-1.png)

- 画像セットについて詳しくは、[クイックスタート：画像セット](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sets/quick-start-image-sets.html?lang=ja)を参照してください。
- 方法について詳しくは、[画像プリセットの作成](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sets/creating-image-set.html?lang=ja#creating-an-image-set)を参照してください。

### スウォッチセット

このタイプのセットは、通常、同じ項目のカラー表示に使用されます。画像とカラースウォッチのペアで構成されます。

スウォッチと画像セットの主な違いは、スウォッチセットでは別の画像をクリック可能なスウォッチとして使用するのに対して、画像セットでは元の画像のミニチュアでクリック可能なサムネールバージョンを使用する点です。

スウォッチセットでは、画像の色付けは行われません（一般的な誤解です）。画像セットと同様に、画像はスワップされるだけです。ミニスウォッチ画像は、Photoshop を使用して作成したり、各色を個別に撮影したり、Dynamic Media Classic の切り抜きツールを使用して色付きの画像の 1 つからスウォッチを作成したりできます。

![画像](assets/media-sets/image-set-2.jpg)

_スウォッチセットの例_

上記のスウォッチセットの URL は次のように表示されます。

![画像](assets/media-sets/image-set_url.png)

- スウォッチセットについて詳しくは、[スウォッチセットのクイックスタート](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/swatch-sets/quick-start-swatch-sets.html?lang=ja)を参照してください。
- [スウォッチセットの作成](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/swatch-sets/creating-swatch-set.html?lang=ja#creating-a-swatch-set)について説明します。

### スピンセット

このセットは、通常、項目の 360 度ビューを表示するために使用されます。スウォッチセットと同様に、スピンセットも 3D マジックを使用しません。実際の作業は、画像のあらゆる面から多くの写真を作成することです。ビューアーでは、ストップモーションアニメーションのように画像を切り替えることができます。

スピンセットは、1 つの軸に沿って 1 方向にスピンすることも、2D スピンセットとして交互に作成した場合は複数の軸にスピンすることもできます。例えば、すべての車輪が地面に接している間に車を回転させてから、上方向に「反転」させ、後輪で回転させることもできます。2D スピンセットを適切に設定するには、各軸の 1 行あたりの画像数を同じにする必要があります。つまり、2 軸で回転する場合、1 つの角度のスピンの 2 倍の画像が必要です。

![画像](assets/media-sets/image-set-3.png)

_スピンセットの例_

上記のスピンセットの URL は、次のように表示されることがあります。

![画像](assets/media-sets/spin-set.png)

- スピンセットについて詳しくは、[スピンセットのクイックスタート](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/spin-sets/quick-start-spin-sets.html?lang=ja)を参照してください。
- [スピンセットの作成](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/spin-sets/creating-spin-set.html?lang=ja#creating-a-spin-set)について説明します。

## 混在メディアセット

これは組み合わせセットです。これにより、前の任意のセットを組み合わせ、ビデオを 1 つのビューアーに追加できます。このワークフローでは、最初に任意のコンポーネントセットを作成し、それらを 1 つにまとめて混在メディアセットに組み立てます。

![画像](assets/media-sets/image-set-4.png)

_混在メディアセットの例_

上記の混在メディアセットの URL は次のように表示される場合があります。

![画像](assets/media-sets/image-set-url-1.png)

- 混在メディアセットについて詳しくは、[混在メディアセットのクイックスタート](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/mixed-media-sets/quick-start-mixed-media-sets.html?lang=ja)を参照してください。

- [混在メディアセットの作成](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/mixed-media-sets/creating-mixed-media-set.html?lang=ja#creating-a-mixed-media-set)について説明します。

Web サイト上にズーム、セットまたはビデオ用の画像を表示するには、その画像を Dynamic Media Classic の「ビューアー」で呼び出します。Dynamic Media Classic には、スウォッチセット、スピンセット、ビデオなど数多くのリッチメディアアセットのビューアが含まれています。

詳しくは、[AEM Assets および Dynamic Media Classic 用のビューアー](https://experienceleague.adobe.com/docs/dynamic-media-developer-resources/library/viewers-aem-assets-dmc/c-html5-s7-aem-asset-viewers.html?lang=ja)を参照してください。

## バッチセットプリセット

これまでは、Dynamic Media Classic のビルド機能を使用してセットを手動でビルドする方法について説明してきました。ただし、標準化された命名規則がある限り、バッチセットプリセットを使用して画像セットとスピンセットの作成を自動化することはできます。

各プリセットは、定義された命名規則に一致する画像を使用して、セットの構成方法を定義する、固有の名前を持つ自己完結した命令のセットです。プリセットでは、まず、1 つのセットにグループ化するアセットの命名規則を定義します。その後、バッチセットプリセットを作成して、これらの画像を参照できます。

プリセットを自分で作成することも可能ですが（**設定／アプリケーション設定／バッチセットプリセット**）、ベストプラクティスとしては、コンサルティングチームまたはテクニカルサポートに設定してもらう必要があります。理由は次のとおりです。

- バッチセットプリセットは、設定が複雑になる場合があります。正規表現を使用するため、開発者以外は、この構文に慣れていないか、混乱する可能性があります。
- 作成されると、デフォルトでオンになります。「取り消し」機能はありません。何千もの画像のアップロードを開始し、プリセットが正しく設定されていない場合、数百、数千の壊れたセットが発生し、手動で検索および削除する必要が生じる場合があります。

以前は、バッチセットプリセットに組み込むのが非常に簡単な単純な命名規則を提案していました。ただし、プリセットは非常に柔軟なので、複雑な命名方法を扱うことができます。つまり、1 つのセットに属する画像は、共通の名前（多くの場合、SKU 番号または製品 ID）で結び付ける必要があります。Dynamic Media Classic では、すべての画像をプリセットに使用するためのデフォルトの命名規則を指定するか、異なる命名規則を持つ複数のプリセットを作成できます。

バッチセットプリセットはアップロード時にのみ適用され、画像のアップロード後は実行できません。したがって、すべての画像の読み込みを開始する前に、命名規則を計画し、プリセットを作成しておくことが重要です。

プリセットを作成した後、会社管理者は、プリセットをアクティブにするか非アクティブにするかを選択できます。 「アクティブ」とは、アップロードページの&#x200B;**ジョブオプション**&#x200B;に表示されることです。非アクティブなプリセットは非表示のままになります。

[バッチセットプリセットの作成](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html?lang=ja#creating-a-batch-set-preset)について説明します。

### アップロード時のバッチセットプリセットの使用

次に、作成後のアップロード時のバッチセットプリセットの使用方法を示します。

1. 「**アップロード**」をクリックし、「**デスクトップから**」または「**FTP 経由**」を選択します。
2. 「**ジョブオプション**」をクリックします。
3. 「**バッチセットプリセット**」オプションを開き、プリセットをオンまたはオフにして、アップロードで使用します。
4. アップロードが完了したら、フォルダー内で完了したセットを確認します。

詳しくは、[バッチセットプリセット](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html?lang=ja#batch-set-presets)を参照してください。
