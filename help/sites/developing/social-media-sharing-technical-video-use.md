---
title: AEM Sites でのソーシャルメディア共有の使用
description: ソーシャルメディア共有コンポーネントの設定と使用について説明します。
feature: Core Components
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: Developer
level: Intermediate
doc-type: Technical Video
exl-id: 569069e8-7964-49f1-96ed-7dfa4f8ed96c
duration: 511
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '162'
ht-degree: 100%

---

# ソーシャルメディア共有の使用 {#using-social-media-sharing-in-aem-sites}

ソーシャルメディア共有コンポーネントの設定と使用について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/328273?quality=12&learn=on&captions=jpn)

この動画では、[We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail#weretail) サンプル web サイトを使用して、ソーシャルメディア共有コンポーネント（[AEM コアコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=ja)の一部）の次の機能について説明します。

* 0:00 - ソーシャルメディア共有コンポーネントの追加と設定
* 1:00 - Facebook への共有
* 3:10 - Pinterest への共有
* 6:25 - 製品ページでのソーシャルメディア共有コンポーネントの使用

## Externalizer の設定 {#externalizer-setup}

![Day CQ Link Externalizer](assets/externalizer.png)

[http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl](http://localhost:4502/system/console/configMgr/com.day.cq.commons.impl.ExternalizerImpl)

[AEM externalizer](https://helpx.adobe.com/jp/experience-manager/6-5/sites/developing/using/externalizer.html) は、AEM オーサーと AEM パブリッシュの両方で設定する必要があります。これにより、パブリッシュ実行モードを、AEM パブリッシュへのアクセスに使用される公開アクセス可能なドメインにマッピングします。

この動画では、`/etc/hosts` を使用して *www.example.com* を localhost に解決するように偽装し、[基本的な AEM Dispatcher 設定](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html?lang=ja)を使用して www.example.com が AEM Publish の前に置かれるようにします。

## サポート資料 {#supporting-materials}

* [AEM コアコンポーネントのダウンロード](https://github.com/adobe/aem-core-wcm-components/releases)
* [We.Retail をダウンロード](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Dispatcher のインストール](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/getting-started/dispatcher-install.html?lang=ja)
