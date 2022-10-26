---
title: Dynamic MediaとAEM Assetsの概要
description: このビデオシリーズでは、Adobe Experience Manager Dynamic Mediaをコンテンツサービスとして使用して、メディアコンテンツの管理方法とアクセス方法の概要を説明します。 Dynamic Mediaでは、Experience Manager Assets独自の機能である、動的なデジタルエクスペリエンスを管理および公開できます。 アドビのフレームワークとコンポーネントスイートを使用すると、マーケターはあらゆるデバイスをまたいでインタラクティブなマルチメディアエクスペリエンスをカスタマイズし、提供できます。
feature: Smart Crop, Video Profiles, Image Profiles, Viewer Presets, 360 VR Video, Image Sets, Spin Sets
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 59462cb4-d379-4e58-b786-ff8dbae6191c
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '871'
ht-degree: 0%

---

# AEM AssetsでのDynamic Mediaの使用 {#understanding-aem-dynamic-media}

このマルチパートビデオシリーズでは、Adobe Experience Manager Dynamic Mediaをコンテンツサービングサービスとして使用してメディアコンテンツを管理し、アクセスする方法の概要を説明します。 Dynamic Mediaでは、Experience Manager Assets独自の機能である、動的なデジタルエクスペリエンスを管理および公開できます。 アドビのフレームワークとコンポーネントスイートを使用すると、マーケターはあらゆるデバイスをまたいでインタラクティブなマルチメディアエクスペリエンスをカスタマイズし、提供できます。

## Dynamic Mediaの概要

>[!VIDEO](https://video.tv.adobe.com/v/27144/?quality=9&learn=on)

>[!NOTE]
>
>ここで示される機能は、Dynamic Media DMS7 実行モードで使用できます。現在サポートされている実行モードでは、DMS7 が置き換えられた DMHybrid 実行モードとは限りません。

このビデオでは、Adobe Experience Manager Dynamic Mediaをコンテンツサービングサービスとして使用して、メディアコンテンツを管理し、アクセスする方法について説明します。 Dynamic Mediaは、1 つのマスターアセット手法に基づいて動作します。画像アセットやビデオアセットは、必要な消耗品バリエーションや派生レンディションの無制限のセットを満たすように要求できます。 含む：

* 単一マスターアセットから URL への製品の成果物の説明
* 画像処理オプション
* コンテンツビューアオプション
* 画像およびレスポンシブビューアへの URL のコピー
* URL へのスマート切り抜きのバリエーション

## Dynamic MediaとAEM Sitesおよびその他の CMS

>[!VIDEO](https://video.tv.adobe.com/v/27145/?quality=9&learn=on)

>[!NOTE]
>
>ここで示される機能は、Dynamic Media DMS7 実行モードで使用できます。現在サポートされている実行モードでは、DMS7 が置き換えられた DMHybrid 実行モードとは限りません。 このビデオでは、第 1 部ビデオ (Dynamic Mediaの概要 ) で説明している概念を参照します。

このビデオでは、Adobe Experience Manager Dynamic Mediaでのメディアコンテンツの管理方法と、AEM Sitesでのコンポーネントの使用を簡単にし、レスポンシブページの幅に基づいて最適化するために、簡単かつ自動的に切り抜きます。 インタラクティブな画像バナーを簡単に作成し、任意のコンテンツ管理システムで使用するコピー URL を生成できます。

* AEM Sites Dynamic Mediaコンポーネントの柔軟性
* 画像プリセットを使用したローカルダウンロード
* インタラクティブバナーの作成と公開

## Dynamic Mediaを使用して混在メディアコレクションとカスタムビューアを作成する

>[!VIDEO](https://video.tv.adobe.com/v/27146/?quality=9&learn=on)

>[!NOTE]
>
>ここで示される機能は、Dynamic Media DMS7 実行モードで使用できます。現在サポートされている実行モードでは、DMS7 が置き換えられた DMHybrid 実行モードとは限りません。 このビデオでは、第 1 部ビデオ (Dynamic Mediaの概要 ) で説明している概念を参照します。

このビデオでは、スピンセット、ビデオ、製品画像のコレクションなど、メディアアセットの混在メディアビューアコレクションを簡単に作成する方法について説明します。 混在メディアセットにコンテンツを追加し、最終的なコピー URL またはAEM Sitesコンポーネントから選択できるカスタマイズ済みビューアを作成します。

* 適切な製品の写真からスピンセットを作成
* Dynamic Media Video のマスタービデオをアップロードしてエンコード
* スピンセット、ビデオ、写真から混在メディアセットを作成
* カスタム混在メディアビューアを編集して使用する

## Dynamic Media画像プリセット

>[!VIDEO](https://video.tv.adobe.com/v/27320/?quality=9&learn=on)

このビデオでは、画像プリセットの作成方法と画像プリセットとは何かを説明します。画像プリセットは、URL で画像が要求されたときにその画像に対して操作する一連の Image Server 引数の URL 短縮サービスです。 画像プリセットを拡張および編集するための貴重なテクニックを学びます。

* 画像プリセット短縮サービス明示的な Image Server コマンドのコレクションを非表示にする
* 新しくサイズ変更された画像をパディングなしで準拠するには、1 ピクセルの寸法（幅または高さ）のみを使用します
* 常にシャープを使用する
* 画像プリセットのサイズを変更するコマンドを追加する URL 修飾子フィールド

## Dynamic Media Advanced URL 修飾子

>[!VIDEO](https://video.tv.adobe.com/v/27319/?quality=9&learn=on)

このビデオでは、画像のサイズ変更を超えて、ソースファイル自体の機能（クリッピングパスと切り抜き、テキストに組み込まれる背景の透明度）を活用する方法について説明します。これは、Dynamic Media の URL 修飾子を使用した変数です。

* Dynamic Media修飾子フィールドでの URL 修飾子の使用
* 透明度を持つ画像の背景色の変更
* 画像のパスのクリッピング
* 画像のパスに切り抜き
* Photoshopファイルからのテキストテンプレートの作成

## Dynamic MediaJPEGファイルサイズの制御（キロバイト）

>[!VIDEO](https://video.tv.adobe.com/v/27404/?quality=9&learn=on)


>[!NOTE]
>
>画像画質は逆圧縮のパーセンテージで測定されます。100 %画質は最も低い圧縮率で、高画質の画像が得られますが、比較的大きなファイルサイズが得られます。 JPEG 圧縮は非可逆圧縮方式で、圧縮設定によって画質とファイルサイズが決まります。

jpeg 画像の画質と結果のファイルサイズ（キロバイト単位）のバランスを取り、2 つのコマンドを使用して jpeg 圧縮設定を調整し、ページ読み込み速度を向上させます。 QLT は、jpeg 圧縮品質設定を調整して画質を定義します。 JPEGサイズコマンドを使用すると、圧縮を使用して達成する必要のあるファイルサイズを指定できます。

## CC クローズドキャプションをDynamic Mediaビデオに追加

>[!VIDEO](https://video.tv.adobe.com/v/28074/?quality=9&learn=on)

クローズドキャプションをDynamic Mediaビデオに簡単に追加するには、コピー URL を追加して、追加のクローズドキャプションファイルドキュメント（任意のビデオの CC 情報を含む web.VTT サイドカーファイル）を指定します。

## AEM Dynamic Media での画像シャープニングの使用

このビデオでは、画像の忠実性を維持するために画像のシャープニングが重要な理由と、詳細設定を使用して完全な画像を作成する方法を説明します。

>[!VIDEO](https://demos-pub.assetsadobe.com/etc/dam/viewers/s7viewers/html5/VideoViewer.html?asset=%2Fcontent%2Fdam%2Fdm-public-facing-upgrade-portal-video%2F04_DynamicImagery_AdvancedSettings_071917_BH.mp4&amp;config=/etc/dam/presets/viewer/Video_social&amp;serverUrl=https%3A%2F%2Fadobedemo62-h.assetsadobe.com%2Fis%2Fimage%2F&amp;contenturl=%2F&amp;config2=/etc/dam/presets/analytics&amp;videoserverurl=https://gateway-na.assetsadobe.com/DMGateway/public/demoCo&amp;posterimage=/content/dam/dm-public-facing-upgrade-portal-video/04_DynamicImagery_AdvancedSettings_071917_BH.mp4)
