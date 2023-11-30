---
title: 画像、スウォッチ、スピンおよび混在メディアセット
description: Dynamic Media Classicの最も役に立つ強力な機能の 1 つは、画像、スウォッチ、スピン、混在メディアセットなどのリッチメディアセットの作成をサポートしている点です。 各リッチメディアセットの概要と、Dynamic Media Classicで各タイプを作成する方法について説明します。 次に、アップロード時にリッチメディアセットを作成するプロセスを自動化するバッチセットプリセットについて詳しく説明します。
feature: Dynamic Media Classic, Image Sets, Mixed Media Sets, Spin Sets
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: 45c86ff2-d991-46a7-a8d1-25c9fec142d9
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1433'
ht-degree: 3%

---

# 画像、スウォッチ、スピンおよび混在メディアセット {#media-sets}

動的なサイズ変更やズームのために 1 つの画像だけでなく、Dynamic Media Classicのコレクションを設定することで、より豊富なオンラインエクスペリエンスを実現します。 この節では、Dynamic Media Classicで次のリッチメディアセットを作成する方法について説明します。

- 画像セット
- スウォッチセット
- スピンセット
- 混在メディアセット

また、バッチセットプリセットを使用して、アップロードを使用してセット作成を自動化する方法についても説明します。

## セットに関して常に知りたかったすべて

基本的な動的サイズ設定とズームの横に、セットはおそらく最も広く使用されているDynamic Media Classicのサブ製品です。 セットとは、基本的に、実際の画像を含まず、他の画像やビデオとの関係のセットで構成される「仮想」アセットです。 セットの主な魅力は、それらが「既製」のミニアプリケーションであることです。 つまり、設定された各ビューアには独自のロジックとインターフェイスが含まれるので、サイト上で呼び出すだけで済みます。 また、追跡が必要なのはセットごとに 1 つのアセット ID だけです。すべてのメンバーアセットと関係を自分で管理する必要はありません。

セットを作成すると、そのセットは別のアセットとして管理され、URL から提供する前に公開用にマークして公開する必要があります。 すべてのメンバーアセットも公開する必要があります。

### セットのタイプ

Dynamic Media Classicで作成できる 4 種類のセット（画像、スウォッチ、スピン、混在メディアセット）について説明します。

## 画像セット

これは最も一般的なタイプのセットです。 通常は、同じアイテムの代替ビューに使用します。 この画像は、関連する画像のサムネールをクリックして、ビューアに読み込む複数の画像で構成されます。

![画像](assets/media-sets/image-set-1.jpg)

_画像セットの例_

上記の画像セットの URL は次のように表示されます。

![画像](assets/media-sets/image-set-url-1.png)

- 画像セットの詳細を説明します。 [画像セットのクイックスタート](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sets/quick-start-image-sets.html).
- 方法を学ぶ [画像セットの作成](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/image-sets/creating-image-set.html#creating-an-image-set).

### スウォッチセット

このタイプのセットは、通常、同じ項目のカラー表示に使用されます。 画像とカラースウォッチのペアで構成されます。

スウォッチと画像セットの主な違いは、スウォッチセットでは別の画像をクリック可能なスウォッチとして使用するのに対して、画像セットでは元の画像のミニチュアでクリック可能なサムネールバージョンを使用する点です。

スウォッチセットでは、画像の色付けは行われません（一般的な誤解です）。 画像セットと同様に、画像はスワップされるだけです。 ミニスウォッチ画像は、Photoshopを使用して作成したり、各色を個別に撮影したり、Dynamic Media Classicの切り抜きツールを使用して色付きの画像の 1 つからスウォッチを作成したりできます。

![画像](assets/media-sets/image-set-2.jpg)

_スウォッチセットの例_

上記のスウォッチセットの URL は次のように表示されます。

![画像](assets/media-sets/image-set_url.png)

- スウォッチセットの詳細については、 [スウォッチセットのクイックスタート](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/swatch-sets/quick-start-swatch-sets.html).
- 方法を学ぶ [スウォッチセットの作成](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/swatch-sets/creating-swatch-set.html#creating-a-swatch-set).

### スピンセット

このセットは、通常、項目の 360 度ビューを表示するために使用されます。 スウォッチセットと同様に、スピンセットも 3D マジックを使用しません。実際の作業は、あらゆる面から画像の多くの写真を作成することです。 ビューアでは、ストップモーションアニメーションのように画像を切り替えることができます。

スピンセットは、1 つの軸に沿って 1 方向にスピンすることも、2D スピンセットとして交互に作成した場合は複数の軸にスピンすることもできます。 例えば、すべての車輪が地面にある間に車を回転させ、後ろの車輪で上に「反転」したり、回転させたりすることができます。 2D スピンセットを適切に設定するには、各軸の 1 行あたりの画像数を同じにする必要があります。 つまり、2 軸で回転する場合、1 つの角度のスピンの 2 倍の画像が必要です。

![画像](assets/media-sets/image-set-3.png)

_スピンセットの例_

上記のスピンセットの URL は、次のように表示されます。

![画像](assets/media-sets/spin-set.png)

- を使用したスピンセットの詳細 [スピンセットのクイックスタート](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/spin-sets/quick-start-spin-sets.html).
- 方法を学ぶ [スピンセットの作成](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/spin-sets/creating-spin-set.html#creating-a-spin-set).

## 混在メディアセット

これは組み合わせセットです。 これにより、前の任意のセットを組み合わせ、ビデオを 1 つのビューアに追加できます。 このワークフローでは、最初に任意のコンポーネントセットを作成し、それらを 1 つにまとめて混在メディアセットに組み立てます。

![画像](assets/media-sets/image-set-4.png)

_混在メディアセットの例_

上記の混在メディアセットの URL は次のように表示されます。

![画像](assets/media-sets/image-set-url-1.png)

- を使用した混在メディアセットの詳細 [混在メディアセットのクイックスタート](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/mixed-media-sets/quick-start-mixed-media-sets.html).

- 方法を学ぶ [混在メディアセットの作成](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/mixed-media-sets/creating-mixed-media-set.html#creating-a-mixed-media-set).

Web サイト上にズーム、セットまたはビデオ用の画像を表示するには、その画像をDynamic Media Classicの「ビューア」で呼び出します。 Dynamic Media Classicには、スウォッチセット、スピンセット、ビデオなどのリッチメディアアセットのビューアが含まれています。

詳細情報： [AEM AssetsとDynamic Media Classicのビューア](https://experienceleague.adobe.com/docs/dynamic-media-developer-resources/library/viewers-aem-assets-dmc/c-html5-s7-aem-asset-viewers.html?lang=ja).

## バッチセットプリセット

これまでは、Dynamic Media Classic Build 機能を使用してセットを手動でビルドする方法について説明してきました。 ただし、標準化された命名規則がある限り、バッチセットプリセットを使用して画像セットとスピンセットの作成を自動化することはできます。

各プリセットは、定義された命名規則に一致する画像を使用してセットを構築する方法を定義する、一意の名前と自己完結型の命令のセットです。 プリセットでは、まず、1 つのセットにグループ化するアセットの命名規則を定義します。 その後、バッチセットプリセットを作成して、これらの画像を参照できます。

プリセットは自分で作成することもできますが（これらはの下にあります）、 **設定/アプリケーション設定/バッチセットプリセット** ) を使用する場合は、ベストプラクティスとして、コンサルティングチームまたはテクニカルサポートに設定してもらう必要があります。 理由は次のとおりです。

- バッチセットプリセットは、設定が複雑になる場合があります。正規表現を使用します。開発者以外は、この構文に慣れていないか、混乱を招く可能性があります。
- 作成されると、デフォルトでオンになります。 「元に戻す」機能はありません。 何千もの画像のアップロードを開始し、プリセットが正しく設定されていない場合、数百、数千の壊れたセットが発生し、手動で検索および削除する必要が生じる場合があります。

以前は、バッチセットプリセットの作成が非常に簡単な、単純な命名規則を提案していました。 ただし、プリセットは非常に柔軟なので、複雑な命名方法を扱うことができます。 つまり、1 つのセットに属する画像は、共通の名前（多くの場合、SKU 番号または製品 ID）で結び付ける必要があります。 Dynamic Media Classicでは、すべての画像をプリセットに使用するためのデフォルトの命名規則を指定するか、異なる命名規則を持つ複数のプリセットを作成できます。

バッチセットプリセットはアップロード時にのみ適用され、画像のアップロード後は実行できません。 したがって、すべての画像の読み込みを開始する前に、命名規則を計画し、作成したプリセットを取得することが重要です。

プリセットを作成した後、会社管理者は、プリセットをアクティブにするか非アクティブにするかを選択できます。 「アクティブ」は、これらがの下のアップロードページに表示されることを意味します。 **ジョブオプション**&#x200B;非アクティブなプリセットは非表示のままです。

方法を学ぶ [バッチセットプリセットの作成](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html#creating-a-batch-set-preset).

### アップロード時のバッチセットプリセットの使用

次に、作成後のアップロード時のバッチセットプリセットの使用方法を示します。

1. クリック **アップロード** を選択し、次のいずれかを選択します。 **デスクトップから** または **FTP 経由**.
2. クリック **ジョブオプション**.
3. を開きます。 **バッチセットプリセット** 「 」オプションを選択し、プリセットをオンまたはオフにして、アップロードで使用します。
4. アップロードが完了したら、フォルダー内で完了したセットを確認します。

詳細情報： [バッチセットプリセット](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html#batch-set-presets).
