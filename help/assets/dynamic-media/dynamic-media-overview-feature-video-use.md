---
title: Dynamic Media と AEM Assets の連携の概要
description: このビデオシリーズでは、Adobe Experience Manager Dynamic Media をコンテンツ提供サービスとして使用してメディアコンテンツを管理およびアクセスする方法の概要を説明します。Dynamic Media では、動的なデジタルエクスペリエンスを管理および公開できます。これは Experience Manager Assets に固有の機能です。アドビのフレームワークとコンポーネントスイートを使用すると、マーケターは、インタラクティブなマルチメディアエクスペリエンスをカスタマイズして、あらゆるデバイスに提供できます。
feature: Smart Crop, Video Profiles, Image Profiles, Viewer Presets, 360 VR Video, Image Sets, Spin Sets
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 59462cb4-d379-4e58-b786-ff8dbae6191c
duration: 2559
source-git-commit: 0400242f6a99bc5209a8b483469d5fd88eac077e
workflow-type: ht
source-wordcount: '794'
ht-degree: 100%

---

# AEM Assets と Dynamic Media の連携 {#understanding-aem-dynamic-media}

このマルチパートビデオシリーズでは、Adobe Experience Manager Dynamic Media をコンテンツ提供サービスとして使用してメディアコンテンツを管理およびアクセスする方法の概要を説明します。Dynamic Media では、動的なデジタルエクスペリエンスを管理および公開できます。これは Experience Manager Assets に固有の機能です。アドビのフレームワークとコンポーネントスイートを使用すると、マーケターは、インタラクティブなマルチメディアエクスペリエンスをカスタマイズして、あらゆるデバイスに提供できます。

## Dynamic Media の概要

>[!VIDEO](https://video.tv.adobe.com/v/27144?quality=12&learn=on)

>[!NOTE]
>
>ここで紹介している機能は、Dynamic Media の DMS7 実行モード（アドビで現在サポートしている実行モード）で利用できます。DMHybrid 実行モードではありません。これは DMS7 に置き換わりました。

このビデオでは、Adobe Experience Manager Dynamic Media をコンテンツ提供サービスとして使用して、メディアコンテンツを管理しアクセスする方法について説明します。Dynamic Media は、単一メインアセットの手法に基づいて動作します。この手法では、リクエスト可能な 1 つの画像アセットやビデオアセットをアップロードして、必要とする消費可能なバリエーションや派生レンディションの無制限のセットを実現します。次のものが含まれます。

* 単一メインアセットから URL への製品成果物の説明
* 画像処理オプション
* コンテンツビューアーオプション
* 画像およびレスポンシブビューアーへのコピー URL
* URL へのスマート切り抜きバリエーション

## AEM Sites での使用

>[!VIDEO](https://video.tv.adobe.com/v/27145?quality=12&learn=on)

>[!NOTE]
>
>ここで紹介している機能は、Dynamic Media の DMS7 実行モード（アドビで現在サポートしている実行モード）で利用できます。DMHybrid 実行モードではありません。これは DMS7 に置き換わりました。このビデオでは、第 1 部のビデオ（Dynamic Media の概要）で説明している概念を参照しています。

このビデオでは、メディアコンテンツを Adobe Experience Manager Dynamic Media で管理す方法と、コンポーネントを使用して AEM Sites でメディアコンテンツを容易に使用し、レスポンシブなページ幅に基づいてシンプルな自動切り抜きで最適化する方法について説明します。インタラクティブな画像バナーを容易に作成し、任意のコンテンツ管理システムで使用するためのコピー URL を生成できます。

* AEM Sites Dynamic Media コンポーネントの柔軟性
* 画像プリセットでのローカルのダウンロード
* インタラクティブバナーの作成と公開

## 混在メディアコレクションの作成

>[!VIDEO](https://video.tv.adobe.com/v/27146?quality=12&learn=on)

>[!NOTE]
>
>ここで紹介している機能は、Dynamic Media の DMS7 実行モード（アドビで現在サポートしている実行モード）で利用できます。DMHybrid 実行モードではありません。これは DMS7 に置き換わりました。このビデオでは、第 1 部のビデオ（Dynamic Media の概要）で説明している概念を参照しています。

このビデオでは、スピンセット、ビデオ、製品画像コレクションなど、メディアアセットの混在メディアビューアコレクションを簡単に作成するプロセスについて説明します。混在メディアセットにコンテンツを追加し、最終的なコピー URL または AEM Sites コンポーネントから選択できるカスタマイズ済みビューアーを作成します。

* 適切な製品写真からのスピンセットの作成
* メインビデオのアップロードと Dynamic Media ビデオ用のエンコード
* スピンセット、ビデオおよび写真からの混在メディアセットの作成
* カスタム混在メディアビューアの編集と使用

## 画像プリセット

>[!VIDEO](https://video.tv.adobe.com/v/27320?quality=12&learn=on)

このビデオでは、画像プリセットの概要と作成方法を説明します。画像プリセットは、URL で画像がリクエストされるたびにその画像に適用される一連の Image Server 引数に対する URL 短縮機能です。画像プリセットを拡張および編集するための貴重なテクニックを学びます。

* 画像プリセット短縮機能で明示的な Image Server コマンドのコレクションを隠蔽
* 1 ピクセルのディメンション（幅または高さ）のみを使用して、サイズ変更された新しい画像をパディングなしで適合化
* 常にシャープを使用
* URL 修飾子フィールドを使用して、画像プリセットのサイズを変更するコマンドを追加

## 高度な URL 修飾子

>[!VIDEO](https://video.tv.adobe.com/v/27319?quality=12&learn=on)

このビデオでは、Dynamic Media の URL 修飾子を使用して、画像のサイズ変更にとどまらず、背景の透明度、組み込みクリッピングパス、切り抜き、変数としてのテキストなどのソースファイル自体の機能を活用する方法を説明します。

* Dynamic Media 修飾子フィールドでの URL 修飾子の使用
* 透明度のある画像の背景色の変更
* 画像パスへのクリッピング
* 画像パスへの切り抜き
* Photoshop ファイルからのテキストテンプレートの作成

## JPEG ファイルサイズ管理

>[!VIDEO](https://video.tv.adobe.com/v/27404?quality=12&learn=on)


>[!NOTE]
>
>画像画質は逆圧縮のパーセンテージで測定されます。100％画質は最も低い圧縮率で、高画質の画像が得られますが、比較的大きなファイルサイズになります。JPEG 圧縮は非可逆圧縮方式で、圧縮設定によって画質とファイルサイズが決まります。

2 つのコマンドを使用して jpeg 圧縮設定を調整することで、jpeg 画像の画質と結果のファイルサイズ（キロバイト単位）のバランスを取って、ページの読み込み速度を向上させます。QLT では、jpeg 圧縮品質設定を調整して画質を定義します。JPEG サイズコマンドを使用すると、圧縮を使用して達成する必要のあるファイルサイズを指定できます。

## クローズドキャプション

>[!VIDEO](https://video.tv.adobe.com/v/28074?quality=12&learn=on)

クローズドキャプションを Dynamic Media ビデオに容易に追加するには、コピー URL を追加して、追加のクローズドキャプションファイルドキュメント（任意のビデオの CC 情報を含む web.VTT サイドカーファイル）を指定します。

## 画像のシャープニング

このビデオでは、画像の忠実性を維持するために画像のシャープニングが重要な理由と、詳細設定を使用して完全な画像を作成する方法を説明します。

>[!VIDEO](https://demos-pub.assetsadobe.com/etc/dam/viewers/s7viewers/html5/VideoViewer.html?asset=%2Fcontent%2Fdam%2Fdm-public-facing-upgrade-portal-video%2F04_DynamicImagery_AdvancedSettings_071917_BH.mp4&amp;config=/etc/dam/presets/viewer/Video_social&amp;serverUrl=https%3A%2F%2Fadobedemo62-h.assetsadobe.com%2Fis%2Fimage%2F&amp;contenturl=%2F&amp;config2=/etc/dam/presets/analytics&amp;videoserverurl=https://gateway-na.assetsadobe.com/DMGateway/public/demoCo&amp;posterimage=/content/dam/dm-public-facing-upgrade-portal-video/04_DynamicImagery_AdvancedSettings_071917_BH.mp4)
