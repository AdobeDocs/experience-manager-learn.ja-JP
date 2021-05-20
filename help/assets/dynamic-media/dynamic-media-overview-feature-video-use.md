---
title: Dynamic MediaとAEM Assetsの概要
description: このビデオシリーズでは、Adobe Experience Manager Dynamic Mediaをコンテンツサービングサービスとして使用して、メディアコンテンツの管理方法とアクセス方法の概要を説明します。 Dynamic Mediaでは、動的なデジタルエクスペリエンスを管理および公開できます。これは、Experience Managerアセットに特有の機能です。 アドビのフレームワークとコンポーネントスイートを使用すると、マーケターはすべてのデバイスでインタラクティブなマルチメディアエクスペリエンスをカスタマイズし、提供できます。
sub-product: dynamic-media
feature: スマート切り抜き、ビデオプロファイル、イメージプロファイル、ビューアプリセット、360 VRビデオ、画像セット、スピンセット
version: 6.3, 6.4, 6.5
topic: コンテンツ管理
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '888'
ht-degree: 1%

---


# AEM AssetsでのDynamic Mediaの使用{#understanding-aem-dynamic-media}

このマルチパートビデオシリーズでは、Adobe Experience Manager Dynamic Mediaをコンテンツサービングサービスとして使用して、メディアコンテンツの管理方法とアクセス方法の概要を説明します。 Dynamic Mediaでは、動的なデジタルエクスペリエンスを管理および公開できます。これは、Experience Managerアセットに特有の機能です。 アドビのフレームワークとコンポーネントスイートを使用すると、マーケターはすべてのデバイスでインタラクティブなマルチメディアエクスペリエンスをカスタマイズし、提供できます。

## Dynamic Mediaの概要

>[!VIDEO](https://video.tv.adobe.com/v/27144/?quality=9&learn=on)

>[!NOTE]
>
>ここで示す機能は、Dynamic Media DMS7実行モードで使用できます。現在サポートされている実行モードは、必ずしもDMS7に置き換えられたDMHybrid実行モードとは限りません。

このビデオでは、Adobe Experience Manager Dynamic Mediaをコンテンツサービングサービスとして使用して、メディアコンテンツの管理およびアクセスをおこなう方法について説明します。 Dynamic Mediaは、1つのマスターアセット手法に基づいて動作します。この手法では、必要な消耗品のバリエーションや派生レンディションの無制限のセットを満たすようにリクエストできる画像アセットやビデオアセットをアップロードします。 含む：

* 単一マスターアセットからURLへの製品の成果物の説明
* 画像処理オプション
* コンテンツビューアオプション
* 画像およびレスポンシブビューアへのURLのコピー
* URLへのスマート切り抜きバリエーション

## Dynamic MediaとAEM Sitesおよびその他のCMS

>[!VIDEO](https://video.tv.adobe.com/v/27145/?quality=9&learn=on)

>[!NOTE]
>
>ここで示す機能は、Dynamic Media DMS7実行モードで使用できます。現在サポートされている実行モードは、必ずしもDMS7に置き換えられたDMHybrid実行モードとは限りません。 このビデオでは、第1部ビデオ(Dynamic Mediaの概要)で説明している概念を参照します。

このビデオでは、Adobe Experience Manager Dynamic Mediaでのメディアコンテンツの管理方法を説明し、AEM Sitesで簡単に使用して、シンプルかつ自動的に切り抜いて、レスポンシブページの幅に基づいて最適化する方法を示します。 インタラクティブ画像バナーを簡単に作成し、任意のコンテンツ管理システムで使用するコピーURLを生成できます。

* AEM Sites Dynamic Mediaコンポーネントの柔軟性
* 画像プリセットを使用したローカルでのダウンロード
* インタラクティブバナーの作成と公開

## Dynamic Media：混在メディアコレクションおよびカスタムビューアの作成

>[!VIDEO](https://video.tv.adobe.com/v/27146/?quality=9&learn=on)

>[!NOTE]
>
>ここで示す機能は、Dynamic Media DMS7実行モードで使用できます。現在サポートされている実行モードは、必ずしもDMS7に置き換えられたDMHybrid実行モードとは限りません。 このビデオでは、第1部ビデオ(Dynamic Mediaの概要)で説明している概念を参照します。

このビデオでは、スピンセット、ビデオおよび製品画像のコレクションを含む、メディアアセットの混在メディアビューアコレクションの単純な作成プロセスについて説明します。 混在メディアセットにコンテンツを追加し、最終的なコピーURLまたはAEM Sitesコンポーネントから選択するためのカスタマイズビューアを作成します。

* 適切な製品写真からスピンセットを作成する
* Dynamic Media Videoのマスタービデオをアップロードしてエンコードする
* スピンセット、ビデオ、写真から混在メディアセットを作成
* カスタム混在メディアビューアの編集と使用

## Dynamic Media画像プリセット

>[!VIDEO](https://video.tv.adobe.com/v/27320/?quality=9&learn=on)

このビデオでは、画像プリセットの作成方法と画像プリセットとは何かを説明します。画像プリセットはURLで要求されるたびに画像を操作する一連のImage Server引数のURL短縮サービスです。 画像プリセットを拡張および編集するための貴重なテクニックを学びます。

* 画像プリセット短縮サービスによる明示的なImage Serverコマンドのコレクションの非表示
* 1ピクセルの寸法（幅または高さ）のみを使用して、パディングなしでサイズ変更された新しい画像を準拠する
* 常にシャープを使用する
* 画像プリセットのサイズを変更するコマンドを追加するURL修飾子フィールド

## Dynamic Media Advanced URL修飾子

>[!VIDEO](https://video.tv.adobe.com/v/27319/?quality=9&learn=on)

このビデオでは、画像のサイズ変更を超えて、ソースファイル自体の機能(背景の透明度、クリッピングパスに組み込まれ、トリミングとテキスト（Dynamic MediaのURL修飾子を使用する変数）を活用する方法について説明します。

* Dynamic Media修飾子フィールドでのURL修飾子の使用
* 透明度を持つ画像の背景色の変更
* 画像パスのクリッピング
* 画像パスの切り抜き
* Photoshopファイルからのテキストテンプレートの作成

## Dynamic Media JPEGファイルサイズの制御（キロバイト）

>[!VIDEO](https://video.tv.adobe.com/v/27404/?quality=9&learn=on)


>[!NOTE]
>
>画像画質は逆圧縮の割合で測定されます。100%画質は最も低い圧縮で、高画質ですが、比較的大きなファイルサイズになります。 JPEG圧縮は非可逆圧縮方式で、圧縮設定によって画質とファイルサイズが決まります。

JPEG画像の画質と結果のファイルサイズ（KB単位）のバランスを取り、ページ読み込み速度を向上させます。2つのコマンドを使用してJPEG圧縮設定を調整します。 QLTは、jpeg圧縮画質設定を調整して画質を定義します。 「JPEGサイズ」コマンドを使用すると、圧縮を使用して実行する必要があるファイルサイズを指定できます。

## CCクローズドキャプションのDynamic Mediaビデオへの追加

>[!VIDEO](https://video.tv.adobe.com/v/28074/?quality=9&learn=on)

クローズドキャプションをDynamic Mediaビデオに簡単に追加できます。追加のクローズドキャプションファイルドキュメント（ビデオのCC情報を含むweb.VTTサイドカーファイル）を指すURLをコピーを追加します。

## AEM Dynamic Media での画像シャープニングの使用

このビデオでは、画像の正確性を維持するために画像のシャープニングが重要な理由と、詳細設定を使用して完全な画像を作成する方法を説明します。

>[!VIDEO](https://demos-pub.assetsadobe.com/etc/dam/viewers/s7viewers/html5/VideoViewer.html?asset=%2Fcontent%2Fdam%2Fdm-public-facing-upgrade-portal-video%2F04_DynamicImagery_AdvancedSettings_071917_BH.mp4&amp;config=/etc/dam/presets/viewer/Video_social&amp;serverUrl=https%3A%2F%2Fadobedemo62-h.assetsadobe.com%2Fis%2Fimage%2F&amp;contenturl=%2F&amp;config2=/etc/dam/presets/analytics&amp;videoserverurl=https://gateway-na.assetsadobe.com/DMGateway/public/demoCo&amp;posterimage=/content/dam/dm-public-facing-upgrade-portal-video/04_DynamicImagery_AdvancedSettings_071917_BH.mp4)
