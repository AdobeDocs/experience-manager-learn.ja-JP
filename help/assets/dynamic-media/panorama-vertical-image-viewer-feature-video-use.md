---
title: AEM AssetsDynamic Mediaとのパノラマ&垂直画像ビューアの使用
description: AEM 6.4でのDynamic Mediaビューアの機能強化には、パノラマ画像ビューア、パノラマバーチャルリアリティ画像ビューア、および垂直画像ビューアが追加されました。 パノラマビューアでは、特別な開発を行うことなく、魅力的で没入型のルーム、プロパティ、場所、または景観を楽しめます。
sub-product: dynamic-media
feature: Video Profiles, Video Profiles, 360 VR Video
version: 6.4, 6.5
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 4%

---


# パノラマと縦置き画像ビューアのAEM AssetsDynamic Mediaでの使用{#using-panorama-and-vertical-image-viewer-with-aem-assets-dynamic-media}

AEM 6.4でのDynamic Mediaビューアの機能強化には、パノラマ画像ビューア、パノラマバーチャルリアリティ画像ビューア、および垂直画像ビューアが追加されました。 パノラマビューアでは、特別な開発を行うことなく、魅力的で没入型のルーム、プロパティ、場所、または景観を楽しめます。

>[!VIDEO](https://video.tv.adobe.com/v/24156/?quality=9&learn=on)

>[!NOTE]
>
>ビデオでは、AEMインスタンスがDynamic MediaS7モードで実行されていることを前提としています。 [Dynamic MediaでAEMを設定する手順は、こちらを参照してください。](https://helpx.adobe.com/jp/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)

## パノラマVRビューアおよびパノラマVRビューア

画像は、その縦横比（キーワード）に基づいてパノラマと見なされます。 初期設定では、縦横比が2の画像はパノラマ画像と見なされます。 上記の条件を満たす場合、パノラマ画像ビューアのプリセットが画像プレビューで使用できるようになります。 パノラマ画像の縦横比基準は、会社のDMS7設定で変更できます。変更するには、/conf/global/settings/cloudconfigs/dmscene7/jcr:contentで重複プロパティs7PanoramicARを指定します。 キーワードは、アセットのメタデータノードのdc:keywordプロパティに保存されます。 キーワードに次のいずれかの組み合わせが含まれる場合：

* 等角形、
* 球状+パノラマ
* 球状+パノラマ、

縦横比に関係なく、パノラマ画像アセットと見なされます。

## 垂直画像ビューア

横置きスウォッチの場合は、ユーザーのデスクトップ画面のサイズに応じて、ユーザーがページをスクロールダウンするまでスウォッチが表示されないことがあります。 垂直方向の画像ビューアを使用し、垂直方向のスウォッチを配置すると、画面のサイズに関係なく、スウォッチが表示されます。 また、メイン画像のサイズも最大化します。 水平スウォッチの場合、ページ上にスペースを確保して、見える確率が高く、メイン画像のサイズが小さくなるようにする必要がありました。 縦置きレイアウトの場合は、このスペースの割り当てを気にする必要がないので、メイン画像のサイズを最大化できます。

<table> 
 <tbody>
  <tr>
   <td> </td>
   <td>パノラマおよびVRビューア</td>
   <td>垂直画像ビューア</td>
  </tr>
  <tr>
   <td>Dynamic Media実行モード</td>
   <td>Dynamic MediaScene7モードのみ</td>
   <td>DMS7とDynamic Media</td>
  </tr>
  <tr>
   <td>使用例</td>
   <td><p>パノラマビューアとバーチャルリアリティビューアを使用すると、ユーザーの興味を引く体験ができます。 ユーザは予約を行う前でもホテルの部屋をチェックアウトしたり、予約の予定を立てずに貸し出し物件をチェックアウトすることができる。 ユーザーは場所をチェックアウトし、さらに多くの可能性を見ることができます。 ここでの主な焦点は、顧客がWebサイトを訪問し、最終的にコンバージョン率を増やす際に、より良いエクスペリエンスを提供することです。</p> <p> </p> </td> 
   <td><p>縦置き版画像ビューアを使用すると、商品の画像を最大限に表示して、消費者に商品を最適に表現できるので、コンバージョン率を高め、収益を最小限に抑えることができます。</p> <p> </p> </td>
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
