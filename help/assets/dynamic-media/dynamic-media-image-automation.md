---
title: 透明性およびコンテンツの自動バッチ処理のための Dynamic Media
description: AEMの Dynamic Media を使用して、バーチャルレンディションの作成、透明性の管理、スケーラブルなコンテンツ再利用のための画像処理の自動化を行う方法について説明します。
feature: Image Profiles, Viewer Presets
topic: Content Management
role: User
level: Beginner, Intermediate, Experienced
doc-type: Feature Video
duration: 560
last-substantial-update: 2025-05-28T00:00:00Z
jira: KT-18197
exl-id: 13a09ad3-bf55-4524-bf43-f1cdad368034
source-git-commit: 1061a13089e5891ee375c959a9361079c0f8ce20
workflow-type: ht
source-wordcount: '262'
ht-degree: 100%

---

# 透明性およびコンテンツの自動バッチ処理のための Dynamic Media

AEMの Dynamic Media を使用して、バーチャルレンディションの作成、透明性の管理、スケーラブルなコンテンツ再利用のための画像処理の自動化を行う方法について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3459589/?learn=on&enablevpops)


## Dynamic Media アセットの例

以下は、ビデオで使用されている Dynamic Media アセットの例と その URL です。

>[!BEGINTABS]

>[!TAB 画像の透明度の例]

ビデオで使用されている Dynamic Media Image Server のサンプル URL は次のとおりです。

| プレビュー | 説明 | Dynamic Media URL |
|-----------|------------------|---------|
| ![デフォルト](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?bgc=255,255,255){width="250"} | デフォルト | [リンク](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?bgc=255,255,255) |
| ![シームレスな背景画像レイヤーとの合成](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&layer=1&src=backdrop5-Camera&size=8500,8500&layer=2&src=AdobeStock_322150086%20trans){width="250"} | シームレスな背景画像レイヤーとの合成 | [リンク](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&layer=1&src=backdrop5-Camera&size=8500,8500&layer=2&src=AdobeStock_322150086%20trans) |
| ![背景（赤）](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&layer=1&color=200,50,50&size=8500,8500&layer=2&src=AdobeStock_322150086%20trans){width="250"} | 背景（赤） | [リンク](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20trans?&layer=1&color=200,50,50&size=8500,8500&layer=2&src=AdobeStock_322150086%20trans) |
| ![楕円形のパスにクリップ](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&bgc=255,255,255){width="250"} | 楕円形のパスにクリップ | [リンク](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&bgc=255,255,255) |


>[!TAB 画像パスの例]

ビデオで使用されている Dynamic Media Image Server のサンプル URL は次のとおりです。

| プレビュー | 説明 | Dynamic Media URL |
|-----------|------------------|---------|
| ![幅 80 ピクセルに正規化（透明度なし）](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?wid=800){width="250"} | 幅 80 ピクセルに正規化されました（透明度なし）{width="250"} | [リンク](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?wid=800) |
| ![パスに沿って切り抜き](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?cropPathE=Path%201&wid=800){width="250"} | パスに沿ってトリミング | [リンク](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?cropPathE=Path%201&wid=800) |
| ![パスに沿って切り抜く](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&wid=800){width="250"} | パスに沿って切り抜く | [リンク](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&wid=800) |
| ![パスに沿って切り抜き、パスに沿ってトリミングする](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&cropPathE=Path%201&wid=800){width="250"} | パスに沿って切り抜き、パスに沿ってトリミングする | [リンク](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=Path%201&cropPathE=Path%201&wid=800) |
| ![別のパスに沿って切り抜く](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&wid=800){width="250"} | 別のパスに沿って切り抜く | [リンク](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086%20paths?clipPathE=round&wid=800) |
| ![別のパスに沿って切り抜き、背景を赤くする](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086fullpaths?cropPathE=round&clipPathE=round&bgc=200,50,50&wid=800){width="250"} | 別のパスに沿って切り抜き、背景を赤くする | [リンク](https://smartimaging.scene7.com/is/image/DynamicMediaNA/AdobeStock_322150086fullpaths?cropPathE=round&clipPathE=round&bgc=200,50,50&wid=800) |

>[!ENDTABS]


## Dynamic Media Image Server API

* [Dynamic Media 画像サービング／画像レンダリング API](https://experienceleague.adobe.com/ja/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/http-protocol-reference/c-http-protocol-reference)
* [Dynamic Media スナップショットのプレビュー](https://snapshot.scene7.com/)
