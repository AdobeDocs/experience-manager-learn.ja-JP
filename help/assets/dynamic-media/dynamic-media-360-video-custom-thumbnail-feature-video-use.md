---
title: AEM AssetsでのDynamic Media 360ビデオとカスタムビデオサムネールの使用
description: AEM 6.5のDynamic Mediaビューアの機能強化には、360ビデオレンダリング、360メディアビューア（video360Socialおよびvideo360VR）のサポートと、カスタムビデオサムネールの選択機能が含まれています。
sub-product: dynamic-media
feature: ビデオプロファイル
version: 6.3, 6.4, 6.5
topic: コンテンツ管理
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 7%

---


# AEM AssetsでのDynamic Media 360ビデオとカスタムビデオサムネールの使用

AEM 6.5のDynamic Mediaビューアの機能強化には、360ビデオレンダリング、360メディアビューア（video360Socialおよびvideo360VR）のサポートと、カスタムビデオサムネールの選択機能が含まれています。

>[!VIDEO](https://video.tv.adobe.com/v/26391?quality=9&learn=on)

>[!NOTE]
>
>ビデオは、AEMインスタンスがDynamic Media S7モードで動作していることを前提としています。  [Dynamic MediaでのAEMの設定手順については、こちらを参照してください](https://helpx.adobe.com/jp/experience-manager/6-3/assets/using/config-dynamic-fp-14410.html)。ビデオをアップロードすると、Dynamic Mediaはデフォルトで、そのフッテージの縦横比が2:1の場合、360ビデオとして処理します。 つまり、幅と高さの比率は2:1です。

>[!NOTE]
>
>Dynamic Media 360メディアコンポーネントは、360ビデオのみをサポートしています。

## Dynamic Media 360ビデオ

360度ビデオ（球形ビデオとも呼ばれる）は、全方位カメラやカメラのコレクションを使用して撮影し、全方位のビューを同時に記録するビデオ録画です。 フラットディスプレイでの再生中は、ユーザーが視聴方向を制御し、モバイルデバイスでの再生は通常、組み込みのジャイロスコープ制御を利用します。  1枚の写真の限界を超えて広がります。 マーケターは、360ビデオを利用して、ユーザーの魅力的なエクスペリエンスを提供できます。  始めましょう。 パノラマ画像の縦横比の基準は、 /conf/global/settings/cloudconfigs/dmscene7/jcr:contentでdoubleプロパティs7PanoramicARを指定することで、会社のDMS7設定で変更できます。

## Dynamic Media 360ビデオ

Dynamic Mediaビデオで、ビデオのカスタムサムネールを選択する機能がサポートされるようになりました。 ユーザーは、AEM Assetsから既存のアセットを選択するか、サムネールとしてビデオフレームを選択できます。

## Dynamic 360メディアビューア

<table> 
 <tbody>
   <tr>
      <td> </td>
      <td>**Video360Socialビューア**</td>
      <td>**Video360VRビューア**</td>
   </tr>
   <tr>
      <td>Dynamic Media実行モード</td>
      <td>Dynamic Media Scene7モードのみ</td>
      <td>Dynamic Media Scene7モードのみ<br>
         <br>
      </td>
   </tr>
   <tr>
      <td>使用例</td>
      <td>
         <p>ジャイロスコープをサポートしないWebサイトやデバイスの場合</p>
         <p> </p>
      </td>
      <td>
         <p>ジャイロスコープをサポートするデバイスに仮想現実体験を提供 </p>
      </td>
   </tr>
   <tr>
      <td>オーディオ — ステレオモード</td>
      <td>不可</td>
      <td>可</td>
   </tr>
   <tr>
      <td>ビデオ再生</td>
      <td>はい</td>
      <td>はい</td>
   </tr>
   <tr>
      <td>POVナビゲーション</td>
      <td>
         <ul>
            <li>マウスドラッグ（デスクトップシステムの場合）</li>
            <li>スワイプ（タッチデバイス）</li>
         </ul>
      </td>
      <td>
         <ul>
            <li>マウスとドラッグのオプションは無効です</li>
            <li>組み込みのジャイロスコープを使用</li>
         </ul>
      </td>
   </tr>
   <tr>
      <td>HTML5プレーヤー</td>
      <td>はい</td>
      <td>はい</td>
   </tr>
   <tr>
      <td>ソーシャル共有オプション</td>
      <td>はい</td>
      <td>いいえ</td>
   </tr>
</tbody>
</table>

## その他のリソース{#additional-resources}

[Scene7モードでのDynamic Mediaの設定](https://helpx.adobe.com/jp/experience-manager/6-5/assets/using/config-dms7.html)
