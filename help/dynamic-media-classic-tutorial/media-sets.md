---
title: 画像、スウォッチ、スピン、混在メディアセット
description: Dynamic Mediaクラシックの最も役に立つ強力な機能の1つは、画像、スウォッチ、スピン、混在メディアセットなどのリッチメディアセットの作成をサポートしている点です。 各リッチメディアセットの概要と、各タイプを作成する方法をDynamic Mediaクラシックで説明します。 次に、アップロード時にリッチメディアセットを作成するプロセスを自動化するバッチセットプリセットについて詳しく説明します。
sub-product: dynamic-media
feature: Dynamic Media Classic, Image Sets, Mix Media Sets, Spin Sets
doc-type: tutorial
topics: sets, development, authoring, configuring
audience: all
activity: use
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1468'
ht-degree: 2%

---


# 画像、スウォッチ、スピン、混在メディアセット{#media-sets}

動的なサイズ変更とズームのために、1枚の画像以外にも多数の画像を配置できるDynamic Mediaクラシックのセットコレクションを使用すると、オンラインでのより豊富な操作が可能になります。 チュートリアルのこの節では、Dynamic Mediaクラシックで次のリッチメディアセットを作成する方法について説明します。

- 画像セット
- スウォッチセット
- スピンセット
- 混在メディアセット

また、バッチセットプリセットを使用して、アップロードを介してセットを作成する方法についても説明します。

## セットに関して常に知りたかったすべて

基本的な動的サイズ変更とズームの隣に、セットがおそらく最も広く使用されているDynamic Mediaクラシックの副製品です。 セットとは、基本的には「仮想」アセットで、実際の画像は含まれず、他の画像やビデオとの関係のセットで構成されます。 セットの主な魅力は、セットが「すぐに使える」ミニアプリケーションであることです。 つまり、各セットビューアには独自のロジックとインターフェイスが含まれているので、サイトで呼び出すだけで済みます。 また、メンバーアセットと関係をすべて自分で管理する必要がなく、セットごとに1つのアセットIDの追跡のみが必要になります。

セットを作成する場合、そのセットは、URLから供給する前に公開用にマークし、公開する必要がある個別のアセットとして管理されます。 すべてのメンバアセットも公開する必要があります。

### セットのタイプ

次に、Dynamic Mediaクラシックで作成できる4種類のセットについて説明します。画像、スウォッチ、スピン、混在メディアセット

## 画像セット

これは、最も一般的なセットのタイプです。 通常、同じ品目の代替表示に使用します。 ビューアに読み込む複数の画像で構成されます。この画像の関連するサムネールをクリックすると、その画像がビューアに読み込まれます。

![画像](assets/media-sets/image-set-1.jpg)

_画像セットの例_

上記の画像セットのURLは、次のように表示されます。

![画像](assets/media-sets/image-set-url-1.png)

- 画像セットに関する詳細は、[画像セットへのクイック開始](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sets/quick-start-image-sets.html)を参照してください。
- [画像セットを作成する方法](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/image-sets/creating-image-set.html#creating-an-image-set)を説明します。

### スウォッチセット

この種類のセットは、通常、同じ項目の色付き表示を表示するのに使用します。 画像とカラースウォッチのペアで構成されます。

スウォッチと画像セットの主な違いは、スウォッチセットではクリック可能なスウォッチとして別の画像が使用されるのに対し、画像セットでは元の画像のミニチュアでクリック可能なサムネールが使用される点です。

スウォッチセットでは、画像のカラー付けは行われません（一般的な誤解）。 画像セットと同様に、画像が入れ替わるだけです。 ミニスウォッチ画像は、Photoshopを使用して作成したもの、各色を個別に撮影したもの、Dynamic Mediaクラシックの切り抜きツールを使用して色付きの画像の1つを切り抜いたものなどがあります。

![画像](assets/media-sets/image-set-2.jpg)

_スウォッチセットの例_

上記のスウォッチセットのURLは、次のように表示されます。

![画像](assets/media-sets/image-set_url.png)

- スウォッチセットに関する詳細は、[スウォッチセットへのクイック開始](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/swatch-sets/quick-start-swatch-sets.html)を参照してください。
- [スウォッチセット](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/swatch-sets/creating-swatch-set.html#creating-a-swatch-set)の作成方法を説明します。

### スピンセット

このセットは、通常、360度の表示の項目を表示するために使用されます。 スウォッチセットと同様に、スピンセットも3Dマジックを使用しません。実際の作業は、あらゆる面から画像の多くの写真を作成することです。 ビューアでは、ストップモーションアニメーションのように、画像を切り替えるだけです。

スピンセットは、1つの軸に沿って1方向にスピンすることも、2Dスピンセットとして交互に作成した場合は複数の軸でスピンすることもできます。 例えば、車はすべての車輪が地面にある間に回転させ、後ろの車輪でも「反転」して回転させることができます。 適切に設定された2Dスピンセットの場合、各軸の1行あたりの画像数は同じにする必要があります。 つまり、2つの軸で回転している場合は、1つの角度スピンの2倍の画像が必要になります。

![画像](assets/media-sets/image-set-3.png)

_スピンセットの例_

上記のスピンセットのURLは、次のように表示されます。

![画像](assets/media-sets/spin-set.png)

- スピンセットに関する詳細は、[スピンセットへのクイック開始](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/spin-sets/quick-start-spin-sets.html)を参照してください。
- [スピンセットを作成する方法](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/spin-sets/creating-spin-set.html#creating-a-spin-set)を説明します。

## 混在メディアセット

これは組み合わせセットです。 これにより、前のセットを組み合わせてビデオを1つのビューアに追加できます。 このワークフローでは、最初に任意のコンポーネントセットを作成し、それらを1つの混在メディアセットにまとめます。

![画像](assets/media-sets/image-set-4.png)

_混在メディアセットの例_

上記の混在メディアセットのURLは、次のように表示されます。

![画像](assets/media-sets/image-set-url-1.png)

- 混在メディアセットの詳細については、[混在メディアセットへのクイック開始](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/mixed-media-sets/quick-start-mixed-media-sets.html)を参照してください。

- [混在メディアセットを作成する方法](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/mixed-media-sets/creating-mixed-media-set.html#creating-a-mixed-media-set)を説明します。

Webサイト上でズーム、セットまたはビデオの画像を表示するには、Dynamic Mediaクラシックの「ビューア」と呼びます。 Dynamic Mediaクラシックには、スウォッチセット、スピンセット、ビデオなどのリッチメディアアセット用のビューアが含まれています。

[AEM AssetsとDynamic Mediaクラシックの閲覧者](https://docs.adobe.com/content/help/ja-JP/dynamic-media-developer-resources/library/viewers-aem-assets-dmc/c-html5-s7-aem-asset-viewers.translate.html)について詳しく知る。

## バッチセットプリセット

これまで、Dynamic Mediaクラシックビルド機能を使用したセットの手動での構築方法について説明してきました。 ただし、命名規則が標準化されている場合は、バッチセットプリセットを使用して画像セットとスピンセットの作成を自動化できます。

各プリセットは、定義された命名規則に一致する画像を使用してセットの構成方法を定義する、固有の名前を持ち自己完結した命令のセットです。 プリセットでは、まず、セットにグループ化するアセットの命名規則を定義します。 次に、バッチセットプリセットを作成して、これらの画像を参照します。

プリセットは自分で作成できますが（**設定/アプリケーション設定/バッチセットプリセット**&#x200B;にあります）、ベストプラクティスとして、コンサルティングチームまたはテクニカルサポートに設定してもらう必要があります。 理由は次のとおりです。

- バッチセットプリセットは複雑に設定する場合があります。通常の式が動作します。開発者以外の場合は、この構文はなじみのないものか、わかりにくい場合があります。
- 作成した後は、デフォルトでオンになります。 「元に戻す」関数はありません。 何千もの画像のアップロード開始でプリセットが正しく設定されていない場合、何百もの壊れたセットが表示され、手動で検索して削除する必要があります。

バッチセットプリセットを組み込むのが非常に簡単な命名規則を以前に提案しました。 ただし、プリセットは非常に柔軟なので、複雑な命名方法を扱うことができます。 つまり、セットに属する画像は、共通の名前（多くの場合はSKU番号や製品ID）で結び付ける必要があります。 Dynamic Mediaクラシックでは、プリセットに使用するすべての画像に対して初期設定の命名規則を指定するか、異なる命名規則を持つ複数のプリセットを作成できます。

バッチセットプリセットは、アップロード時にのみ適用されます。画像がアップロードされた後は実行できません。 したがって、命名規則を計画し、すべての画像の読み込みを開始する前にプリセットを作成することが重要です。

プリセットを作成すると、会社管理者は、プリセットがアクティブか非アクティブかを選択できます。 「アクティブ」は、ユーザーが&#x200B;**ジョブオプション**&#x200B;の下のアップロードページに表示されることを意味し、非アクティブなプリセットは非表示のままになります。

[バッチセットプリセットを作成する方法](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/application-setup.html#creating-a-batch-set-preset)を説明します。

### アップロード時のバッチセットプリセットの使用

次に、作成後のアップロード時にバッチセットプリセットを使用する方法を示します。

1. 「**アップロード**」をクリックし、「**デスクトップから**」または「**FTP経由**」を選択します。
2. 「**ジョブオプション**」をクリックします。
3. 「**バッチセットプリセット**」オプションを開き、プリセットのチェックボックスをオンまたはオフにして、アップロードで使用します。
4. アップロードが完了したら、フォルダー内で完了したセットを探します。

[バッチセットプリセット](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/application-setup.html#batch-set-presets)について詳しく説明します。
