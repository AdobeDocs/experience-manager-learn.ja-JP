---
title: AEM Dynamic Media でのカラーマネジメントについて
description: このビデオでは、Dynamic Media のカラーマネジメントと、この機能を使用して AEM Assets でカラー補正プレビュー機能を使用できるようにする方法について説明します。
feature: Image Profiles, Video Profiles
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
doc-type: Feature Video
exl-id: a733532b-db64-43f6-bc43-f7d422d5071a
duration: 274
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 100%

---

# AEM Dynamic Media でのカラーマネジメントについて{#understanding-color-management-with-aem-dynamic-media}

このビデオでは、Dynamic Media のカラーマネジメントと、この機能を使用して AEM Assets でカラー補正プレビュー機能を使用できるようにする方法について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/16792?quality=12&learn=on)

>[!NOTE]
>
>この機能を使用するには、AEM で [Dynamic Mediaを有効にします](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=ja)。

この機能は、AEM 6.1 および 6.2 バージョンの機能パックとして使用できます。

## カラーマネジメント設定ノード用の XML テンプレート {#xml-template-for-the-color-management-configuration-node}

次に、カラーマネジメント設定ノードの XML テンプレートを示します。 この XML テンプレートは、AEM 開発プロジェクトにコピーし、プロジェクトに適した内容で設定できます。

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!--
    XML Node definition for: /etc/dam/imageserver/configuration/jcr:content/settings

 Adobe Docs

 * Image Server Configuration: https://docs.adobe.com/docs/en/aem/6-2/administer/content/dynamic-media/config-dynamic.html#Configuring%20Dynamic%20Media%20Image%20Settings

* Default Color Profile Configuration: https://docs.adobe.com/docs/en/aem/6-1/administer/content/dynamic-media/config-dynamic.html#Configuring%20the%20default%20color%20profiles

    iccprofileXXX values:
        Node name of color profile found at: /etc/dam/imageserver/profiles

    iccblackpointcompensation values:
        true | false

    iccdither values:
        true | false

    iccrenderintent values:
        0 for perceptual
        1 for relative colorimetric
        2 for saturation
        3 for absolute colorimetric

-->

<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
    xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
    jcr:primaryType="nt:unstructured"

        bkgcolor="FFFFFF"
        defaultpix="300,300"
        defaultthumbpix="100,100"
        expiration="{Long}36000000"
        jpegquality="80"
        maxpix="2000,2000"
        resmode="SHARP2"
        resolution="72"
        thumbnailtime="[1%,11%,21%,31%,41%,51%,61%,71%,81%,91%]"
        iccprofilergb=""
        iccprofilecmyk=""
        iccprofilegray=""
        iccprofilesrcrgb=""
        iccprofilesrccmyk=""
        iccprofilesrcgray=""
        iccblackpointcompensation="{Boolean}true"
        iccdither="{Boolean}false"
        iccrenderintent="{Long}0"
/>
```

### デフォルトの Adobe カラープロファイルのリストを次に示します。 {#list-of-default-adobe-color-profiles-are-listed-below}

| 名前 | カラースペース | 説明 |
| ------------------- | ---------- | ------------------------------------- |
| AdobeRGB | RGB | Adobe RGB (1998) |
| AppleRGB | RGB | Apple RGB |
| CIERGB | RGB | CIE RGB |
| CoatedFogra27 | CMYK | Coated FOGRA27（ISO 12647-2:2004） |
| CoatedFogra39 | CMYK | Coated FOGRA39（ISO 12647-2:2004） |
| CoatedGraCol | CMYK | Coated GRACoL 2006（ISO 12647-2:2004） |
| ColorMatchRGB | RGB | ColorMatch RGB |
| EuropeISOCoated | CMYK | Europe ISO Coated FOGRA27 |
| EuroscaleCoated | CMYK | Euroscale Coated v2 |
| EuroscaleUncoated | CMYK | Euroscale Uncoated v2 |
| JapanColorCoated | CMYK | Japan Color 2001 Coated |
| JapanColorNewspaper | CMYK | Japan Color 2002 Newspaper |
| JapanColorUncoated | CMYK | Japan Color 2001 Uncoated |
| JapanColorWebCoated | CMYK | Japan Color 2003 Web Coated |
| JapanWebCoated | CMYK | Japan Web Coated（Ad） |
| NewsprintSNAP2007 | CMYK | US Newsprint（SNAP 2007） |
| NTSC | RGB | NTSC（1953） |
| PAL | RGB | PAL／SECAM |
| ProPhoto | RGB | ProPhoto RGB |
| PS4Default | CMYK | Photoshop 4 Default CMYK |
| PS5Default | CMYK | Photoshop 5 Default CMYK |
| SheetfedCoated | CMYK | U.S. Sheetfed Coated v2 |
| SheetfedUncoated | CMYK | U.S. Sheetfed Uncoated v2 |
| SMPTE | RGB | SMPTE-C |
| sRGB | RGB sRGB | IEC61966-2.1 |
| UncoatedFogra29 | CMYK | Uncoated FOGRA29 (ISO 12647-2:2004) |
| WebCoated | CMYK | U.S. Web Coated (SWOP) v2 |
| WebCoatedFogra28 | CMYK | Web Coated FOGRA28 (ISO 12647-2:2004) |
| WebCoatedGrade3 | CMYK | Web Coated SWOP 2006 Grade 3 Paper |
| WebCoatedGrade5 | CMYK | Web Coated SWOP 2006 Grade 5 Paper |
| WebUncoated | CMYK | U.S. Web Uncoated v2 |
| WideGamutRGB | RGB | Wide Gamut RGB |

## その他のリソース{#additional-resources}

* [Dynamic Media カラーマネジメントの設定](https://helpx.adobe.com/jp/experience-manager/6-5/assets/using/config-dynamic.html#ConfiguringDynamicMediaColorManagement)
