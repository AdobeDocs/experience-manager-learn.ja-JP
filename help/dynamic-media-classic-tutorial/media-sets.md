---
title: 画像、スウォッチ、スピン、混在メディアセット
description: Dynamic Media Classicの最も役に立つ強力な機能の1つは、画像、スウォッチ、スピン、混在メディアセットなどのリッチメディアセットの作成に対するサポートです。 各リッチメディアセットの概要と、Dynamic Media Classicでの各タイプの作成方法について説明します。 次に、アップロード時にリッチメディアセットを作成するプロセスを自動化するバッチセットプリセットについて詳しく説明します。
sub-product: dynamic-media
feature: Dynamic Media Classic、画像セット、混在メディアセット、スピンセット
doc-type: tutorial
topics: sets, development, authoring, configuring
audience: all
activity: use
topic: コンテンツ管理
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1468'
ht-degree: 2%

---


# 画像、スウォッチ、スピンおよび混在メディアセット{#media-sets}

動的なサイズ変更とズームのために1つの画像以外にも対応できるDynamic Media Classicのセットコレクションを使用すると、より豊富なオンラインエクスペリエンスを実現できます。 このチュートリアルの節では、Dynamic Media Classicで次のリッチメディアセットを作成する方法について説明します。

- 画像セット
- スウォッチセット
- スピンセット
- 混在メディアセット

また、バッチセットプリセットを使用して、アップロードを通じてセット作成を自動化する方法についても説明します。

## セットに関して常に知りたかったすべて

基本的な動的サイズ設定とズームの隣に、セットはおそらく最も広く使用されているDynamic Media Classicサブ製品です。 セットとは、基本的に、実際の画像を含まないが、他の画像やビデオとの関係のセットで構成される「仮想」アセットです。 セットの主な魅力は、それらが「すぐに使える」ミニアプリケーションであることです。 つまり、設定された各ビューアには独自のロジックとインターフェイスが含まれているので、サイト上で呼び出すだけで済みます。 また、メンバーアセットと関係をすべて自分で管理する必要はなく、セットごとに1つのアセットIDのみを追跡する必要があります。

セットを作成する場合、そのセットは別のアセットとして管理され、URLから提供するには公開用にマークして公開する必要があります。 すべてのメンバーアセットも公開する必要があります。

### セットのタイプ

Dynamic Media Classicで作成できる4種類のセットについて説明します。画像、スウォッチ、スピン、混在メディアセット

## 画像セット

これは最も一般的なタイプのセットです。 通常は、同じ項目の別のビューに使用します。 ビューアに読み込む複数の画像で構成され、その画像の関連するサムネールをクリックします。

![画像](assets/media-sets/image-set-1.jpg)

_画像セットの例_

上記の画像セットのURLは、次のように表示されます。

![画像](assets/media-sets/image-set-url-1.png)

- [画像セットのクイックスタート](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sets/quick-start-image-sets.html)で画像セットの詳細を説明します。
- [画像セット](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sets/creating-image-set.html#creating-an-image-set)を作成する方法を説明します。

### スウォッチセット

このタイプのセットは、通常、同じ項目のカラー表示に使用されます。 画像とカラースウォッチのペアで構成されます。

スウォッチセットと画像セットの主な違いは、スウォッチセットではクリック可能なスウォッチとして異なる画像が使用されるのに対して、画像セットでは元の画像の小型でクリック可能なサムネールバージョンが使用される点です。

スウォッチセットでは、画像の色は変わりません（一般的な誤解です）。 画像セットと同様に、画像が入れ替えられるだけです。 ミニスウォッチ画像は、Photoshopを使用して作成したり、各色を個別に撮影したり、Dynamic Media Classicの切り抜きツールを使用して色付きの画像の1つからスウォッチを作成したりできます。

![画像](assets/media-sets/image-set-2.jpg)

_スウォッチセットの例_

上記のスウォッチセットのURLは、次のように表示されます。

![画像](assets/media-sets/image-set_url.png)

- [スウォッチセットのクイックスタート](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/swatch-sets/quick-start-swatch-sets.html)でスウォッチセットについて詳しく説明します。
- [スウォッチセット](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/swatch-sets/creating-swatch-set.html#creating-a-swatch-set)の作成方法を説明します。

### スピンセット

このセットは、通常、項目の360度のビューを表示するために使用されます。 スウォッチセットと同様に、スピンセットも3Dマジックを使用しません。実際の作業は、あらゆる面から画像の多くの写真を作成することです。 ビューアでは、ストップモーションアニメーションのように画像を切り替えるだけです。

スピンセットは、1つの軸に沿って1方向にスピンすることも、2Dスピンセットとして交互に作成した場合は複数の軸にスピンすることもできます。 例えば、すべての車輪が地面にある間に車を回転させ、後ろの車輪でも「反転」させて回転させることができます。 2Dスピンセットを適切にセットアップする場合は、各軸の1行あたりの画像数を同じにする必要があります。 つまり、2軸で回転する場合、1回の角度スピンの2倍の画像が必要です。

![画像](assets/media-sets/image-set-3.png)

_スピンセットの例_

上記のスピンセットのURLは、次のように表示されます。

![画像](assets/media-sets/spin-set.png)

- [スピンセットのクイックスタート](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/spin-sets/quick-start-spin-sets.html)を使用したスピンセットの詳細を説明します。
- [スピンセット](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/spin-sets/creating-spin-set.html#creating-a-spin-set)の作成方法を説明します。

## 混在メディアセット

これは組み合わせセットです。 以前のセットを組み合わせ、ビデオを1つのビューアに追加できます。 このワークフローでは、最初に任意のコンポーネントセットを作成し、それらを混在メディアセットにまとめます。

![画像](assets/media-sets/image-set-4.png)

_混在メディアセットの例_

上記の混在メディアセットのURLは、次のようになります。

![画像](assets/media-sets/image-set-url-1.png)

- [混在メディアセットのクイックスタート](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/mixed-media-sets/quick-start-mixed-media-sets.html)を使用した混在メディアセットの詳細を説明します。

- [混在メディアセット](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/mixed-media-sets/creating-mixed-media-set.html#creating-a-mixed-media-set)の作成方法を説明します。

Webサイト上でズーム、セットまたはビデオ用の画像を表示するには、Dynamic Media Classicの「ビューア」でその画像を呼び出します。 Dynamic Media Classicには、スウォッチセット、スピンセット、ビデオなどのリッチメディアアセットのビューアが含まれています。

[AEM AssetsおよびDynamic Media Classicのビューア](https://docs.adobe.com/content/help/ja-JP/dynamic-media-developer-resources/library/viewers-aem-assets-dmc/c-html5-s7-aem-asset-viewers.translate.html)について詳しく説明します。

## バッチセットプリセット

これまでは、Dynamic Media Classicビルド機能を使用してセットを手動でビルドする方法について説明してきました。 ただし、標準化された命名規則がある限り、バッチセットプリセットを使用して画像セットとスピンセットの作成を自動化できます。

各プリセットは、定義された命名規則に一致する画像を使用してセットを構築する方法を定義する、一意の名前と自己完結した命令のセットです。 プリセットでは、まず、1つのセットにグループ化するアセットの命名規則を定義します。 次に、バッチセットプリセットを作成して、これらの画像を参照できます。

プリセットは自分で作成できますが（ **設定/アプリケーション設定/バッチセットプリセット**&#x200B;の下にあります）、ベストプラクティスとして、コンサルティングチームまたはテクニカルサポートに設定してもらう必要があります。 理由は次のとおりです。

- バッチセットプリセットは、複雑に設定することができます。正規表現を使用します。開発者以外は、この構文に慣れていないか、混乱を招く可能性があります。
- 作成後は、デフォルトでオンになります。 「元に戻す」機能はありません。 何千もの画像のアップロードを開始し、プリセットが正しく設定されていない場合、数百、数千の壊れたセットが発生し、手動で検索して削除する必要があります。

以前は、バッチセットプリセットへの組み込みが非常に簡単な、単純な命名規則が推奨されていました。 ただし、プリセットは非常に柔軟なので、複雑な命名方法を使用できます。 つまり、セットに属する画像は、共通の名前（多くの場合、SKU番号または製品ID）で結び付ける必要があります。 Dynamic Media Classicでは、すべての画像に対してデフォルトの命名規則を使用するか、異なる命名規則を使用して複数のプリセットを作成できます。

バッチセットプリセットはアップロード時にのみ適用されます。画像がアップロードされた後は実行できません。 したがって、すべての画像の読み込みを開始する前に、命名規則を計画し、プリセットを作成することが重要です。

プリセットを作成したら、会社管理者は、アクティブか非アクティブかを選択できます。 「アクティブ」は、アップロードページの「**ジョブオプション**」の下に表示されるのに対して、非アクティブなプリセットは非表示のままになります。

[バッチセットプリセット](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/application-setup.html#creating-a-batch-set-preset)の作成方法を説明します。

### アップロード時のバッチセットプリセットの使用

次に、作成後のアップロード時のバッチセットプリセットの使用方法を示します。

1. 「**アップロード**」をクリックし、「**デスクトップから**」または「**FTP経由**」を選択します。
2. 「**ジョブオプション**」をクリックします。
3. 「**バッチセットプリセット**」オプションを開き、アップロードで使用するプリセットのチェックボックスをオンまたはオフにします。
4. アップロードが完了したら、フォルダー内で完了したセットを探します。

[バッチセットプリセット](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/application-setup.html#batch-set-presets)の詳細を説明します。
