---
title: AEM Assets Dynamic Mediaでのパノラマビューアと垂直画像ビューアの使用
description: AEM 6.4のDynamic Mediaビューアの機能強化には、パノラマ画像ビューア、パノラマ仮想現実画像ビューア、および垂直画像ビューアが追加されました。 パノラマビューアは、カスタム開発をおこなわずに、部屋、物件、場所、風景などの魅力的で没入感のあるエクスペリエンスを簡単に提供します。
sub-product: dynamic-media
feature: ビデオプロファイル、ビデオプロファイル、360 VRビデオ
version: 6.4, 6.5
topic: コンテンツ管理
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 4%

---


# AEM Assets Dynamic Media{#using-panorama-and-vertical-image-viewer-with-aem-assets-dynamic-media}でのパノラマビューアと垂直画像ビューアの使用

AEM 6.4のDynamic Mediaビューアの機能強化には、パノラマ画像ビューア、パノラマ仮想現実画像ビューア、および垂直画像ビューアが追加されました。 パノラマビューアは、カスタム開発をおこなわずに、部屋、物件、場所、風景などの魅力的で没入感のあるエクスペリエンスを簡単に提供します。

>[!VIDEO](https://video.tv.adobe.com/v/24156/?quality=9&learn=on)

>[!NOTE]
>
>ビデオは、AEMインスタンスがDynamic Media S7モードで動作していることを前提としています。 [Dynamic MediaでのAEMの設定手順は、こちらを参照してください。](https://helpx.adobe.com/jp/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## パノラマおよびパノラマVRビューア

画像は、縦横比またはキーワードに基づいてパノラマと見なされます。 デフォルトでは、縦横比が2の画像はパノラマ画像と見なされます。 上記の条件を満たす場合、パノラマ画像ビューアプリセットを画像プレビューで使用できるようになります。 パノラマ画像の縦横比の基準は、 /conf/global/settings/cloudconfigs/dmscene7/jcr:contentでdoubleプロパティs7PanoramicARを指定することで、会社のDMS7設定で変更できます。 キーワードは、アセットのメタデータノードのdc:keywordプロパティに保存されます。 キーワードに次の組み合わせのいずれかが含まれる場合：

* 等角形
* 球状+パノラマ
* 球+パノラマ

縦横比に関係なく、パノラマ画像アセットと見なされます。

## 垂直画像ビューア

水平スウォッチの場合は、消費者のデスクトップ画面サイズに応じて、ユーザーがページを下にスクロールするまでスウォッチが表示されないことがあります。 垂直画像ビューアを使用して垂直方向にスウォッチを配置すると、画面サイズに関係なく、スウォッチが表示されます。 また、メイン画像のサイズも最大にします。 水平スウォッチの場合、スウォッチが表示される確率が高く、メイン画像のサイズが小さくなるように、ページ上にスペースを確保する必要がありました。 垂直レイアウトの場合は、このスペースの割り当てを気にする必要がないので、メイン画像のサイズを最大化できます。

<table> 
 <tbody>
  <tr>
   <td> </td>
   <td>パノラマビューアおよびVRビューア</td>
   <td>垂直画像ビューア</td>
  </tr>
  <tr>
   <td>Dynamic Media実行モード</td>
   <td>Dynamic Media Scene7モードのみ</td>
   <td>DMS7とDynamic Media</td>
  </tr>
  <tr>
   <td>使用例</td>
   <td><p>パノラマビューアとバーチャルリアリティビューアは、より魅力的なエクスペリエンスを提供します。 ユーザは予約を行う前でもホテルの部屋をチェックアウトしたり、予約をする必要がなくレンタル物件をチェックアウトすることができる。 ユーザーは、場所や他の多くの可能性をチェックアウトできます。 ここでの主な焦点は、消費者がWebサイトを訪問した際のより優れたエクスペリエンスを提供し、最終的にコンバージョン率を高めることです。</p> <p> </p> </td> 
   <td><p>垂直画像ビューアは、製品画像の表示エクスペリエンスを最大限に高め、消費者に製品の最適な表現を提供することで、コンバージョンを促進し、収益を最小限に抑えるのに役立ちます。</p> <p> </p> </td>
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
>パノラマビューアはパノラマ画像で機能し、通常の画像では使用されません。
