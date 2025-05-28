---
title: 透明性およびコンテンツ自動処理のバッチ処理のための Dynamic Media
description: AEMの Dynamic Media を使用して、バーチャルレンディションの作成、透明性の管理、スケーラブルなコンテンツ再利用のための画像処理の自動化を行う方法について説明します。
feature: Image Profiles, Viewer Presets
topic: Content Management
role: User
level: Beginner, Intermediate, Experienced
doc-type: Feature Video
duration: 560
last-substantial-update: 2025-05-28T00:00:00Z
jira: KT-18197
source-git-commit: a509b7c41e6adb05e6c3b791ac2e99f635973322
workflow-type: tm+mt
source-wordcount: '262'
ht-degree: 6%

---


# 透明性およびコンテンツ自動処理のバッチ処理のための Dynamic Media

AEMの Dynamic Media を使用して、バーチャルレンディションの作成、透明性の管理、スケーラブルなコンテンツ再利用のための画像処理の自動化を行う方法について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3459589/?learn=on&enablevpops)


## Dynamic Media アセットの例

次に、ビデオで使用されている Dynamic Media アセットの例と URL を示します。

>[!BEGINTABS]

>[!TAB  画像の透明度の例 ]

ビデオで使用されている Dynamic Media Image Server のサンプル URL は次のとおりです。

| プレビュー | 説明 | Dynamic Media URL |
|-----------|------------------|---------|
| ![デフォルト](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?bgc=255,255,255){width="250"} | デフォルト | [リンク](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?bgc=255,255,255) |
| ![ シームレスな背景画像レイヤーと合成 ](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;src=backdrop5-Camera&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans){width="250"} | シームレスな背景画像レイヤーと合成 | [リンク](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;src=backdrop5-Camera&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans) |
| ![ 背景（赤 ](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;color=200,50,50&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans){width="250"} | 背景（赤） | [リンク](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&amp;layer=1&amp;color=200,50,50&amp;size=8500,8500&amp;layer=2&amp;src=AdobeStock_322150086%20trans) |
| ![ 楕円形のパスにクリップ ](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;bgc=255,255,255){width="250"} | 楕円パスにクリップ | [リンク](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;bgc=255,255,255) |


>[!TAB  画像パスの例 ]

ビデオで使用されている Dynamic Media Image Server のサンプル URL は次のとおりです。

| プレビュー | 説明 | Dynamic Media URL |
|-----------|------------------|---------|
| ![ 幅 80 ピクセルに正規化（透明度なし） ](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?wid=800){width="250"} | 幅 80 ピクセルに正規化されました（透明度なし） {width="250"} | [リンク](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?wid=800) |
| ![ パスに切り抜き ](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?cropPathE=Path%201&amp;wid=800){width="250"} | パスに切り抜き | [リンク](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?cropPathE=Path%201&amp;wid=800) |
| ![ パスにクリップ ](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;wid=800){width="250"} | パスにクリップ | [リンク](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;wid=800) |
| ![ パスにクリップし、パスに切り抜き ](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;cropPathE=Path%201&amp;wid=800){width="250"} | パスにクリップ、パスに切り抜き | [リンク](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&amp;cropPathE=Path%201&amp;wid=800) |
| ![ 別のパスにクリップ ](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;wid=800){width="250"} | 別のパスにクリップ | [リンク](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&amp;wid=800) |
| ![ 別のパスにクリップし、背景を赤くする ](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086fullpaths?cropPathE=round&amp;clipPathE=round&amp;bgc=200,50,50&amp;wid=800){width="250"} | 別のパスにクリップして、背景を赤くする | [リンク](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086fullpaths?cropPathE=round&amp;clipPathE=round&amp;bgc=200,50,50&amp;wid=800) |

>[!ENDTABS]


## Dynamic Media Image Server API

* [Dynamic Media 画像サービングおよび画像レンダリング API](https://experienceleague.adobe.com/en/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/http-protocol-reference/c-http-protocol-reference)
* [Dynamic Media スナップショットのプレビュー ](https://snapshot.scene7.com/)
