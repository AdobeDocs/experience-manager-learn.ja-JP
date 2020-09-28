---
title: AEM Sitesでソーシャルメディアシェアを使用する
description: ソーシャルメディア共有コンポーネントの設定と使用に関するトピックを参照してください。
feature: core-components
topics: integrations
audience: developer, implementer
doc-type: technical video
activity: setup
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 9%

---


# ソーシャルメディア共有を使用する {#using-social-media-sharing-in-aem-sites}

ソーシャルメディア共有コンポーネントの設定と使用に関するトピックを参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/18897/?quality=9&learn=on)

このビデオでは、 [We.Retail](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/introduction.html)サンプルWebサイトを使用して、ソーシャルメディア共有コンポーネント(AEMコアコンポーネントの一部 [](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail) )の次の機能について説明します。

* 0:00 — ソーシャルメディア共有コンポーネントの追加と設定
* 1:00 - Facebookでの共有
* 3:10 - Pinterestとの共有
* 6:25 — 製品ページでのソーシャルメディア共有コンポーネントの使用

## Externalizerの設定 {#externalizer-setup}

![Day CQ Link Externalizer](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[AEM Publishにアクセスするために使用されるpublickアクセス可能なドメインにpublish runmodeをマップするには、AEM externalizer](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/externalizer.html) をAEM AuthorとAEM Publishの両方で設定する必要があります。

このビデオでは、www.example.com `/etc/hosts` を偽ってlocalhostに解決し、 *基本的なAEMディスパッチャー設定を使用してwww.example.comをAEM Publishの前面に配置します*[](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/getting-started/dispatcher-install.html) 。

## サポート資料 {#supporting-materials}

* [AEMコアコンポーネントのダウンロード](https://github.com/adobe/aem-core-wcm-components/releases)
* [Web.Retailのダウンロード](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Dispatcher のインストール](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/getting-started/dispatcher-install.html)
