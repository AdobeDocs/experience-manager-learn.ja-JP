---
title: AEM Assets Dynamic Mediaでのパノラマおよび縦長画像ビューアの使用
description: AEM 6.4 のDynamic Mediaビューアの機能強化には、パノラマ画像ビューア、パノラマ仮想現実画像ビューア、縦長画像ビューアが追加されています。 パノラマビューアを使用すると、カスタム開発をおこなわずに、部屋、物件、場所、風景の魅力的で没入感のあるエクスペリエンスを簡単に提供できます。
feature: Video Profiles, Video Profiles, 360 VR Video
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 6b2f7533-8ce0-4134-b1ae-b3c5d15a05e6
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '533'
ht-degree: 4%

---

# AEM Assets Dynamic Mediaでのパノラマおよび縦長画像ビューアの使用{#using-panorama-and-vertical-image-viewer-with-aem-assets-dynamic-media}

AEM 6.4 のDynamic Mediaビューアの機能強化には、パノラマ画像ビューア、パノラマ仮想現実画像ビューア、縦長画像ビューアが追加されています。 パノラマビューアを使用すると、カスタム開発をおこなわずに、部屋、物件、場所、風景の魅力的で没入感のあるエクスペリエンスを簡単に提供できます。

>[!VIDEO](https://video.tv.adobe.com/v/24156/?quality=9&learn=on)

>[!NOTE]
>
>ビデオは、AEMインスタンスがDynamic Media S7 モードで動作していることを前提としています。 [Dynamic MediaでのAEMの設定手順は、こちらを参照してください。](https://helpx.adobe.com/jp/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## パノラマおよびパノラマ VR ビューア

画像は、縦横比またはキーワードに基づいてパノラマ画像と見なされます。 デフォルトでは、縦横比が 2 の画像はパノラマ画像と見なされます。 上記の条件を満たす場合、パノラマ画像ビューアプリセットを画像のプレビューで使用できるようになります。 パノラマ画像の縦横比の基準は、会社の DMS7 設定で、 /conf/global/settings/cloudconfigs/dmscene7/jcr:content にある double プロパティ s7PanoramicAR を指定することで変更できます。 キーワードは、アセットの metadata ノードの dc:keyword プロパティに保存されます。 キーワードに次の組み合わせのいずれかが含まれる場合：

* 等角形
* 球状+パノラマ
* 球状+パノラマ

縦横比に関係なく、パノラマ画像アセットと見なされます。

## 縦長画像ビューア

水平スウォッチの場合、ユーザーのデスクトップ画面サイズに応じて、ユーザーがページを下にスクロールするまでスウォッチが表示されないことがあります。 垂直方向の画像ビューアを使用し、垂直方向のスウォッチを配置すると、画面サイズに関係なく、スウォッチが確実に表示されます。 また、メイン画像のサイズも最大化します。 横向きスウォッチの場合、スウォッチが表示される可能性が高く、メイン画像のサイズが小さくなるように、ページ上のスペースを確保する必要がありました。 垂直レイアウトの場合は、このスペースの割り当てについて心配する必要がないので、メイン画像のサイズを最大化できます。

<table> 
 <tbody>
  <tr>
   <td> </td>
   <td>パノラマおよび VR ビューア</td>
   <td>縦長画像ビューア</td>
  </tr>
  <tr>
   <td>Dynamic Media実行モード</td>
   <td>Dynamic Media Scene7モードのみ</td>
   <td>DMS7 とDynamic Media</td>
  </tr>
  <tr>
   <td>ユースケース</td>
   <td><p>パノラマビューアとバーチャルリアリティビューアは、より魅力的なエクスペリエンスを提供します。 予約をする前でも、ホテルの部屋をチェックアウトしたり、予約をする必要がなくレンタル物件をチェックアウトすることができる。 ユーザーは、場所や他の多くの可能性をチェックアウトできます。 ここでの主な焦点は、消費者が Web サイトを訪問した際のより優れたエクスペリエンスを提供し、最終的にコンバージョン率を高めることです。</p> <p> </p> </td> 
   <td><p>垂直画像ビューアは、製品画像の表示エクスペリエンスを最大化して、消費者に製品の最適な表現を提供するのに役立ち、コンバージョンを促進し、再来訪を最小限に抑えます。</p> <p> </p> </td>
  </tr>
  <tr>
   <td>使用可 </td>
   <td>OOTB</td>
   <td>OOTB</td>
  </tr>
 </tbody>
</table>

[Scene7モードでのDynamic Mediaの設定](https://helpx.adobe.com/jp/experience-manager/6-5/assets/using/config-dms7.html)

[ハイブリッドモードでのDynamic Mediaの設定](https://helpx.adobe.com/jp/experience-manager/6-5/assets/using/config-dynamic.html)

>[!NOTE]
>
>パノラマビューアはパノラマ画像で機能し、通常の画像では使用しません。
