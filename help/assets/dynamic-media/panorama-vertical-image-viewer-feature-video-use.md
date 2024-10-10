---
title: AEM Assets Dynamic Media でのパノラマ画像ビューアおよび縦長画像ビューアの使用
description: AEM 6.4 の Dynamic Media ビューアの機能強化には、パノラマ画像ビューア、パノラマバーチャルリアリティ画像ビューア、縦長画像ビューアの追加が含まれています。 パノラマビューアを使用すると、部屋、物件、場所、風景の魅力的で没入感のあるエクスペリエンスを、カスタム開発を行わずに簡単に提供できます。
feature: Video Profiles, Video Profiles, 360 VR Video
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Feature Video
exl-id: 6b2f7533-8ce0-4134-b1ae-b3c5d15a05e6
duration: 535
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '518'
ht-degree: 100%

---

# AEM Assets Dynamic Media でのパノラマ画像ビューアおよび縦長画像ビューアの使用{#using-panorama-and-vertical-image-viewer-with-aem-assets-dynamic-media}

AEM 6.4 の Dynamic Media ビューアの機能強化には、パノラマ画像ビューア、パノラマバーチャルリアリティ画像ビューア、縦長画像ビューアの追加が含まれています。 パノラマビューアを使用すると、部屋、物件、場所、風景の魅力的で没入感のあるエクスペリエンスを、カスタム開発を行わずに簡単に提供できます。

>[!VIDEO](https://video.tv.adobe.com/v/24156?quality=12&learn=on)

>[!NOTE]
>
>ビデオでは、AEM インスタンスが Dynamic Media S7 モードで動作していることを前提としています。[Dynamic Media での AEM の設定手順は、こちらを参照してください。](https://helpx.adobe.com/jp/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## パノラマビューアおよびパノラマ VR ビューア

画像は、縦横比またはキーワードに基づいてパノラマ画像と見なされます。デフォルトでは、縦横比が 2 の画像はパノラマ画像と見なされます。上記の条件を満たす場合、画像のプレビューでパノラマ画像ビューアプリセットが使用できるようになります。 パノラマ画像の縦横比の基準は、会社の DMS7 設定で、 /conf/global/settings/cloudconfigs/dmscene7/jcr:content にある double プロパティ s7PanoramicAR を指定することで変更できます。キーワードは、アセットの metadata ノードの dc:keyword プロパティに保存されます。キーワードに次の組み合わせのいずれかが含まれる場合：

* 等角形
* 球状 + パノラミック
* 球状 + パノラマ

縦横比に関係なく、パノラマ画像アセットと見なされます。

## 縦長画像ビューア

横長のスウォッチの場合、消費者のデスクトップ画面サイズによっては、ユーザーがページを下にスクロールするまでスウォッチが表示されないことがあります。縦長画像ビューアを使用し、縦長のスウォッチを配置すると、画面サイズに関係なく、スウォッチが確実に表示されます。また、メイン画像のサイズも最大化されます。横長のスウォッチの場合、スウォッチが表示される可能性が高くなるようにページ上にスペースを確保する必要があり、メイン画像のサイズが小さくなることがありました。縦長レイアウトの場合は、このスペースの割り当てについて心配する必要がないので、メイン画像のサイズを最大化できます。

<table> 
 <tbody>
  <tr>
   <td> </td>
   <td>パノラマおよび VR ビューア</td>
   <td>縦長画像ビューア</td>
  </tr>
  <tr>
   <td>Dynamic Media 実行モード</td>
   <td>Dynamic Media Scene7 モードのみ</td>
   <td>DMS7 および Dynamic Media</td>
  </tr>
  <tr>
   <td>ユースケース</td>
   <td><p>パノラマビューアとバーチャルリアリティビューアは、ユーザーにより魅力的なエクスペリエンスを提供します。予約前であってもホテルの部屋を確認したり、予約する必要がなくレンタル物件を確認したりできます。ユーザーは場所や他の多くの可能性を確認できます。ここでの主な焦点は、消費者が Web サイトを訪問した際により優れたエクスペリエンスを提供し、最終的にコンバージョン率を高めることです。</p> <p> </p> </td> 
   <td><p>縦長画像ビューアは、製品画像の視聴エクスペリエンスを最大化して、消費者に製品の最適な表示を提供するのに役立つことで、コンバージョンを促進し、返品を最小限に抑えます。</p> <p> </p> </td>
  </tr>
  <tr>
   <td>使用可 </td>
   <td>OOTB</td>
   <td>OOTB</td>
  </tr>
 </tbody>
</table>

[Scene7 モードでの Dynamic Media の設定](https://helpx.adobe.com/jp/experience-manager/6-5/assets/using/config-dms7.html)

[ハイブリッドモードでの Dynamic Media の設定](https://helpx.adobe.com/jp/experience-manager/6-5/assets/using/config-dynamic.html)

>[!NOTE]
>
>パノラマビューアはパノラマ画像で機能し、通常の画像での使用は意図されていません。
