---
title: AEM AssetsでのAdobeAsset Link拡張機能の使用
description: 'Adobe Experience Managerアセットは、デザイナーやクリエイティブユーザーが、お気に入りのAdobe Creative Cloudデスクトップアプリケーション内で使用できるようになりました。 Adobe Creative Cloud Enterprise用のAsset Link拡張機能は、Adobe Photoshop、InDesign、IllustratorなどのCreative Cloudツール内でAEM Assetsのメタデータを検索、参照、並べ替え、プレビュー、アップロード、チェックアウト、変更、チェックイン、表示する機能を拡張しました。 '
feature: Adobe Asset Link
version: 6.4, 6.5
topic: コンテンツ管理
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1099'
ht-degree: 2%

---


# AEM Assets{#using-adobe-asset-link-extension-with-aem-assets}でのAdobeAsset Link拡張の使用

Adobe Experience Managerアセットは、デザイナーやクリエイティブユーザーが、お気に入りのAdobe Creative Cloudデスクトップアプリケーション内で使用できるようになりました。 Adobe Creative Cloud Enterprise用のAsset Link拡張機能は、Adobe Photoshop、InDesign、IllustratorなどのCreative Cloudツール内でAEM Assetsのメタデータを検索、参照、並べ替え、プレビュー、アップロード、チェックアウト、変更、チェックイン、表示する機能を拡張しました。


## Adobeアセットリンク1.1

Adobeアセットリンクv1.1で、AdobeアセットリンクとAEM Assetsの間のInDesignの直接リンクがサポートされるようになりました。 InDesignのダイレクトリンクをサポートしているので、AdobeのAsset Linkパネルを使用して、（リンクを配置またはコピーを配置）デジタルアセットをAEM AssetsからInDesignに配置（ドラッグ&amp;ドロップ）できるようになりました。 また、には、 *配置専用* (FPO)レンディションが導入されます。

>[!VIDEO](https://video.tv.adobe.com/v/28988/?quality=12&learn=on)

>[!NOTE]
>
>Adobe Creative CloudのEnterprise IDまたはFederated IDのみを使用します。 必ず[Adobeアセットリンク用にAEMを設定](https://helpx.adobe.com/jp/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html)してください。


### Adobeアセットリンクの機能

* Adobeアセットリンクは、PS、AI、ID内で機能し、AEM Assetsにあるデジタルアセットへの直接アクセスを提供する拡張機能です
* クリエイティブは、AdobeのIMSEnterprise IDまたはFederated IDを使用してAEMに自動的にログインします。
* クリエイティブは、AEM AssetsとCreative Cloudアセット全体での検索に加えて、AEM Assetsに存在するデジタルアセットを閲覧できます
* クリエイティブは、AEM Assetsにあるアセットのファイルの詳細にアクセスできます。パネル内からのサムネール、基本メタデータおよびバージョン
* クリエイティブは、アセットをレイアウトに配置、ダウンロード、ドラッグ&amp;ドロップできます
* クリエイティブは、AEM Assetsからアセットをチェックアウトし、Creative CloudAssetsアカウント内でアセットを操作(WIP)することで、アセットを変更できます
* クリエイティブは、アセットの変更が完了した後でAEM Assetsにアセットを再度チェックインでき、新しいバージョンがAEM Assetsに反映されます
* Creative Cloud2020年、2019年、2018年のInDesign、Photoshop、Illustratorのデスクトップアプリをサポート
* ユーザーは、AdobeAsset Linkのアプリ内パネルからアセット検索を実行し、サイズ、アルファベット順、関連度に基づいて並べ替えることができます
* AEM Assetsのコレクションやスマートコレクションには、Asset Linkパネルから直接アクセスして参照できます
* 新しく作成したアセットをパネルから直接AEM Assetsに追加する
* ユーザーは、アセットを直接InDesignフレームにドラッグ&amp;ドロップできます

### AEM AssetsのInDesign

次のいずれかのオプションを使用して、InDesignのレイアウトにアセットを配置できます。

* **コピーを配置**  - （「コピーを配置」オプションを使用して）InDesignを埋め込むと、バイナリをローカルシステムにダウンロードした後で、元のアセットのコピーがアセットレイアウトに配置されます。Adobeアセットリンクでは、埋め込みコピーと元のアセットの間のリンクは維持されません。 AEM Assetsで元のアセットを変更した場合は、InDesignファイルから埋め込みアセットを削除し、AEM Assetsからアセットを再埋め込みする必要があります。

* **リンクを配置**  -InDesignドキュメントを操作する際に、（コンテキストメニューの「コピーを配置」オプションを使用して）アセットを直接埋め込む以外に、AEM Assetsのアセットを参照するオプションが追加されました。アセットの参照を使用すると、他のユーザーと共同作業したり、元のアセットに対しておこなわれた更新をAEM Assetsに組み込んだりできます。 AEM Assetsからアセットを参照するには、コンテキストメニューの「リンクを配置」オプションを使用します。

### 配置専用(FPO)解像度

Adobeアセットリンクを使用してAEM AssetsのInDesignドキュメントに大きなアセットファイルを配置する場合、クリエイティブユーザーは配置操作を開始してから数秒待つ必要があります。 これは、ユーザーエクスペリエンス全体に影響します。 Adobeアセットリンクを使用すると、AEM Assetsからの元のアセットの低解像度画像を一時的に配置できるようになり、画像の配置にかかる時間を短縮できます。 同時に、全体的なユーザーエクスペリエンスと生産性が向上します。 低い解像度の画像を一時的に配置します。最終出力が印刷または公開に必要な場合は、FPOレンディションを元の画像に置き換える必要があります。 複数のFPO画像をそれぞれの元の画像で置き換える場合は、**_Windows/リンク_**&#x200B;パネルに移動して、元のアセットをダウンロードします。 元の画像をダウンロードしたら、「すべてのFPOを元の画像に置き換える」を選択します。

>[!NOTE]
>
> *配置のみ(FPO)* レンディションは、「リンクを配置」オプションに対してのみ機能します。また、AEM Assets *Dam Update Asset*&#x200B;ワークフロー内でFPOレンディションのサポートを有効にする必要があります。

FPOレンディションは、元のアセットの軽量な代替です。 縦横比は同じですが、元の画像よりも小さいサイズです。 現在、InDesignは、次の画像タイプのFPOレンディションの読み込みのみをサポートしています。

* JPEG
* GIF
* PNG
* TIFF
* PSD
* BMP

AEM Assetsの特定のアセットに対してFPOレンディションが使用できない場合は、元の高解像度アセットが参照されます。 FPO画像の場合、ステータスFPOがInDesignリンクパネルに表示されます。

## AEM Assets{#understanding-adobe-asset-link-authentication-with-aem-assets}とのAdobeアセットリンク認証について

AdobeIdentity Managementサービス(IMS)およびAdobe Experience ManagerオーサーのコンテキストでのAdobeアセットリンク認証の仕組み。

![Adobeアセットリンクのアーキテクチャ](assets/adobe-asset-link-article-understand.png)

[Adobeアセットリンクアーキテクチャ](assets/adobe-asset-link-article-understand-1.png)のダウンロード

1. Adobeアセットリンク拡張は、Adobe Creative Cloud Desktop Appを介してAdobeID管理サービス(IMS)への認証リクエストをおこない、成功するとBearerトークンを受け取ります。
2. Adobeアセットリンク拡張は、**手順1**&#x200B;で取得したBearerトークンを含むHTTP(S)を介して、拡張機能の設定JSONに指定されたスキーム(HTTP/HTTPS)、ホストおよびポートを使用してAEMオーサーに接続します。
3. AEM Bearer Authentication Handlerは、リクエストからBearerトークンを抽出し、AdobeIMSと照らし合わせて検証します。
4. AdobeIMSがBearerトークンを検証すると、AEMにユーザーが作成され（まだ存在しない場合）、AdobeIMSからプロファイルおよびグループ/メンバーシップデータを同期します。 AEMユーザーは、標準のAEMログイントークンが発行され、HTTP(S)応答のCookieとしてAdobeアセットリンク拡張に戻されます。
5. 後続のインタラクション( アセットの参照、検索、チェックイン/チェックアウトなど) AdobeAsset Link拡張機能を使用すると、AEMオーサーに対するHTTP(S)リクエストが生成され、AEMログイントークンを使用して、標準のAEM Token Authentication Handlerを使用して検証されます。

>[!NOTE]
>
>ログイントークンの有効期限が切れると、**手順1～5**&#x200B;が自動的にを呼び出し、Bearerトークンを使用したAdobeアセットリンク拡張の認証と、新しい有効なログイントークンの再発行をおこないます。

## その他のリソース{#additional-resources}

* [AdobeアセットリンクWebサイト](https://www.adobe.com/jp/creativecloud/business/enterprise/adobe-asset-link.html)