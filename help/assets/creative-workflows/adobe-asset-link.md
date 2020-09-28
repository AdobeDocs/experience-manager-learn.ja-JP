---
title: AEM AssetsでのAdobeアセットリンク拡張の使用
description: 'Adobe Experience Managerアセットは、お気に入りのAdobe Creative Cloudデスクトップアプリケーション内で、デザイナーやクリエイティブユーザーが使用できるようになりました。 Adobe Creative Cloudエンタープライズ向けAdobeアセットリンク拡張機能は、Adobe Photoshop、InDesign、表示のようなCreative Cloudツール内で、AEMアセットの検索と参照、並べ替え、プレビュー、アップロード、チェックアウト、変更、チェックイン、Illustratorのメタデータを拡張します。 '
feature: adobe-asset-link
topics: authoring, collaboration, operations, sharing, metadata, images
audience: all
doc-type: feature video
activity: use
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '1092'
ht-degree: 1%

---


# AEM AssetsでのAdobeアセットリンク拡張の使用{#using-adobe-asset-link-extension-with-aem-assets}

Adobe Experience Managerアセットは、お気に入りのAdobe Creative Cloudデスクトップアプリケーション内で、デザイナーやクリエイティブユーザーが使用できるようになりました。 Adobe Creative Cloudエンタープライズ向けAdobeアセットリンク拡張機能は、Adobe Photoshop、InDesign、表示のようなCreative Cloudツール内で、AEMアセットの検索と参照、並べ替え、プレビュー、アップロード、チェックアウト、変更、チェックイン、Illustratorのメタデータを拡張します。


## Adobeアセットリンク1.1

Adobeアセットリンクv1.1で、AdobeアセットリンクとAEM Assets間のInDesignの直接リンクのサポートが提供されました。 InDesignの直接リンクのサポートにより、Adobeアセットリンクパネルを使用して、デジタルアセットを配置（リンクを配置またはコピーを配置）またはAEM AssetsからデジタルアセットをInDesignにドラッグ&amp;ドロップできるようになりました。 また、For Placement Only ** (FPO)レンディションが導入されています。

>[!VIDEO](https://video.tv.adobe.com/v/28988/?quality=12&learn=on)

>[!NOTE]
>
>Adobe Creative CloudのEnterprise IDまたはFederated IDのみを使用してください。 「Adobeアセットリンク [」にAEMを](https://helpx.adobe.com/enterprise/using/configure-aem-for-aal-prerelease.html)設定していることを確認します。


### Adobeアセットリンク機能

* Adobeアセットリンクは、PS、AI、ID内で機能し、AEM Assetsにあるデジタルアセットへの直接アクセスを提供する拡張機能です
* クリエイティブは、AdobeのIMSEnterprise IDまたはFederated IDを使用してAEMに自動的にログインします
* クリエイティブは、AEM AssetsやCreative Cloudのアセットを検索するだけでなく、AEM Assetsにあるデジタルアセットを閲覧できます。
* クリエイティブは、AEM Assetsに居住する資産のファイルの詳細にアクセスできます。パネル内のサムネール、基本メタデータおよびバージョン
* クリエイティブは、アセットをレイアウトに配置、ダウンロードまたはドラッグ&amp;ドロップできます
* クリエイティブは、アセットをAEM Assetsからチェックアウトし、Creative Cloudアセットのアカウント内で作業(WIP)することで、アセットを変更できます。
* クリエイティブは、アセットの変更が完了した後、そのアセットをAEM Assetsに再度チェックインでき、新しいバージョンがAEM Assetsに反映されます
* Creative Cloud2020、2019、2018のInDesign、Photoshop、Illustratorのデスクトップアプリケーションをサポート
* ユーザーは、Adobeアセットリンクアプリ内パネルからアセット検索を実行し、サイズ、アルファベット順、関連性に基づいてアセットを並べ替えることができます
* ユーザーは、アセットリンクパネルから直接、AEM Assetsコレクションやスマートコレクションにアクセスして参照できます
* 新たに追加作成されたアセットをパネルから直接AEM Assetsに
* InDesignは、アセットを直接ユーザーフレームにドラッグ&amp;ドロップできます

### AEM AssetsをInDesignに置く

次のいずれかのオプションを使用して、InDesignレイアウトにアセットを配置できます。

* **コピーを配置** — アセットを埋め込む（「コピーを配置」オプションを使用）と、バイナリをローカルシステムにダウンロードした後、元のアセットのコピーがInDesignレイアウトに配置されます。 Adobeアセットリンクは、埋め込まれたコピーと元のアセットとの間のリンクを維持しません。 元のアセットがAEM Assetsで変更されている場合は、埋め込まれたアセットをInDesignファイルから削除し、AEM Assetsからアセットを再埋め込みする必要があります。

* **リンクを配置** -InDesignドキュメントで作業する場合、（コンテキストメニューの「コピーを配置」オプションを使用して）アセットを直接埋め込む以外に、AEM Assetsのアセットを参照するオプションも追加されました。 アセットを参照することで、他のユーザと共同作業し、AEM Assetsで元のアセットに対して行った更新を取り込むことができます。 AEM Assetsからアセットを参照するには、コンテキストメニューの「リンクを配置」オプションを使用します。

### 配置のみ(FPO)解像度

Adobeアセットリンクを使用して大きなアセットファイルをAEM AssetsのInDesignドキュメントに配置する場合、クリエイティブユーザーは配置操作を開始してから数秒待つ必要があります。 これは、ユーザーエクスペリエンス全体に影響します。 Adobeアセットリンクを使用すると、AEM Assetsから元のアセットの低解像度画像を一時的に配置できるので、画像の配置にかかる時間を短縮できます。 同時に、全体的なユーザーエクスペリエンスと生産性が向上します。 低い解像度の画像が一時的に配置され、印刷や公開に最終出力が必要な場合は、FPOレンディションを元の画像に置き換える必要があります。 複数のFPO画像をそれぞれの元の画像で置き換える場合は、 **_ウィンドウ/リンク_** パネルに移動し、元のアセットをダウンロードします。 元の画像をダウンロードしたら、「すべてのFPOをオリジナルに置き換え」を選択します。

>[!NOTE]
>
> *配置のみ(FPO)* レンディションは、「リンクを配置」オプションでのみ機能します。 また、AEM Assets *ダム更新アセットのワークフロー内でFPOレンディションのサポートを有効にする必要があります* 。

FPOレンディションは、元のアセットの軽量の代替レンディションです。 縦横比は同じですが、元の画像よりもサイズが小さくなります。 現在、InDesignでは、次の画像タイプのFPOレンディションの読み込みのみがサポートされています。

* JPEG
* GIF
* PNG
* TIFF
* PSD
* BMP

AEM Assetsの特定のアセットでFPOレンディションが使用できない場合、元の高解像度アセットが代わりに参照されます。 FPO画像の場合、ステータスFPOはInDesignリンクパネルに表示されます。



## AEM AssetsとのAdobeアセットリンクの認証について{#understanding-adobe-asset-link-authentication-with-aem-assets}

AdobeIdentity Managementサービス(IMS)およびAdobe Experience Manager作成者のコンテキストでのAdobeアセットリンクの認証の動作。

![Adobeアセットリンクのアーキテクチャ](assets/adobe-asset-link-article-understand.png)

ダウンロード [Adobeのアセットリンクアーキテクチャ](assets/adobe-asset-link-article-understand-1.png)

1. Adobeアセットリンク拡張は、Adobe Creative Cloudデスクトップアプリを介してAdobeID管理サービス(IMS)に対する認証リクエストを行い、成功した場合はベアラートークンを受け取ります。
2. Adobeアセットリンクの拡張は、 **手順1**&#x200B;で取得したBearerトークンを含むHTTP(S)経由でAEM Authorに接続し、拡張の設定JSONで指定されたスキーム(HTTP/HTTPS)、ホストおよびポートを使用します。
3. AEMのBearer Authentication Handlerは、要求からBearerトークンを抽出し、AdobeIMSと比較して検証します。
4. AdobeIMSがBearerトークンを検証すると、ユーザーがAEMに作成され（存在しない場合）、AdobeIMSからプロファイルおよびグループ/メンバーシップデータを同期します。 AEMユーザーには標準のAEMログイントークンが発行され、これがHTTP(S)応答上のCookieとしてAdobeアセットリンク拡張に返されます。
5. 後続の操作( アセットの参照、検索、チェックイン/チェックアウトなど) adobeアセットリンク拡張を使用すると、AEM作成者に対するHTTP(S)リクエストが生成されます。このリクエストは、標準のAEMトークン認証ハンドラーを使用して、AEMログイントークンを使用して検証されます。

>[!NOTE]
>
>ログイントークンの有効期限が切れると、 **手順1 ～ 5** が自動的に呼び出され、Bearerトークンを使用したAdobeアセットリンク拡張の認証と、新しい有効なログイントークンの再発行を行います。

## その他のリソース{#additional-resources}

* [AdobeアセットリンクWebサイト](https://www.adobe.com/jp/creativecloud/business/enterprise/adobe-asset-link.html)