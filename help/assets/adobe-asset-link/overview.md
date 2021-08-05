---
title: AdobeアセットリンクとAEM
description: 'Adobe Experience Managerアセットは、デザイナーやクリエイティブユーザーが好きなAdobe Creative Cloudデスクトップアプリケーション内で使用できます。 Adobe Creative Cloud for enterpriseのAsset Link拡張機能は、Adobe XD、Photoshop、InDesign、IllustratorなどのCreative Cloudツール内でAEM Assetsのメタデータを検索および参照、並べ替え、プレビュー、アップロード、チェックアウト、変更、チェックイン、表示する機能を拡張しました。 '
feature: Adobe Asset Link
version: 6.4, 6.5, cloud-service
topic: コンテンツ管理
role: User
level: Beginner
thumbnail: 28988.jpg
source-git-commit: 0cfa83bdbd534f0fa06b3fa0013971feb188224e
workflow-type: tm+mt
source-wordcount: '997'
ht-degree: 3%

---


# Adobeアセットリンク3.0

Adobe Experience Managerアセットは、デザイナーやクリエイティブユーザーが好きなAdobe Creative Cloudデスクトップアプリケーション内で使用できます。

Adobe Creative Cloud for enterpriseのAdobeアセットリンク拡張機能は、Creative Cloudアプリケーション内でAEMアセットのメタデータを検索、参照、並べ替え、プレビュー、アップロード、チェックアウト、変更、チェックイン、表示する機能を拡張しました。

## Adobeアセットリンクの機能

+ Adobeアセットリンクは、AEM AssetsおよびAssets Essentialsと統合されます。
+ Adobeアセットリンクが、クラウドベースのAEM環境(AEM Assets as a Cloud ServiceおよびAssets Essentials)への接続を自動設定
+ Adobeアセットリンクは、Adobe Creative Cloudアプリケーション内で機能する拡張機能です。

   + Adobe XD
   + Adobe Photoshop
   + Adobe Illustrator
   + Adobe InDesign

+ AEMのAdobeEnterprise IDまたはFederated IDを使用した自動認証
+ AEMでのデジタルアセットの参照と検索
+ パネルを使用して、AEM内のアセットに関する次のファイルの詳細にアクセスします。
   + サムネール
   + 基本メタデータ
   + バージョン
+ アセットをレイアウトに配置、ダウンロード、ドラッグ&amp;ドロップします
+ アセットをAEMからチェックアウトし、Assetsアカウント内でアセット(WIP)をCreative Cloudして変更
+ アセットの変更が完了したら、アセットをAEMに再度チェックインすると、新しいバージョンがAEMに反映されます
+ アプリ内パネルでのAEM内のAdobeの検索
+ AEM AssetsコレクションとスマートコレクションをAsset Linkパネルから直接参照
+ 新しく作成したアセットをパネルから直接AEMに追加する
+ アセットをInDesignフレームに直接ドラッグ&amp;ドロップ

## アセットのInDesign

Adobeアセットリンクは、InDesignAsset LinkとAEMの間のAdobeの直接リンクをサポートします。 InDesignの直接リンクをサポートしている場合は、AdobeのAsset Linkパネルを使用して、「リンクされたアセットを配置&#x200B;__」または「__&#x200B;コピーを配置&#x200B;__」を配置したり、デジタルアセットをAEMからInDesignにドラッグ&amp;ドロップできます。__&#x200B;また、には、 *For Placement Only+(FPO)レンディションが導入されています。

>[!VIDEO](https://video.tv.adobe.com/v/28988/?quality=12&learn=on)

>[!NOTE]
>
>Adobe Creative CloudのEnterprise IDまたはFederated IDのみを使用します。 必ず[Adobeアセットリンク用にAEMを設定](https://helpx.adobe.com/jp/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html)してください。

次のいずれかのオプションを使用して、InDesignのレイアウトにアセットを配置できます。

+ **コピーを配置**  - （「コピーを配置」オプションを使用して）InDesignを埋め込むと、バイナリをローカルシステムにダウンロードした後で、元のアセットのコピーがアセットレイアウトに配置されます。Adobeアセットリンクでは、埋め込みコピーと元のアセットの間のリンクは維持されません。 AEMで元のアセットを変更した場合は、埋め込みアセットをInDesignファイルから削除し、AEMからアセットを再埋め込みする必要があります。

+ **リンクを配置**  -InDesignドキュメントを操作する場合、アセットを直接埋め込む以外に、AEMのアセットを参照するオプションもあります（コンテキストメニューの「コピーを配置」オプションを使用）。アセットの参照を使用すると、他のユーザーと共同作業したり、元のアセットに対しておこなわれた更新をAEMに組み込んだりできます。 AEMからアセットを参照するには、コンテキストメニューの「リンクを配置」オプションを使用します。

### 配置専用の画像

Adobeアセットリンクを使用して大きなInDesignファイルをAEMのアセットドキュメントに配置する場合、クリエイティブユーザーは配置操作を開始してから数秒待つ必要があります。 これは、ユーザーエクスペリエンス全体に影響します。 Adobeアセットリンクを使用すると、AEMから元のアセットの低解像度の画像を一時的に配置できるので、画像の配置にかかる時間を短縮できます。 同時に、全体的なユーザーエクスペリエンスと生産性が向上します。 低い解像度の画像を一時的に配置します。最終出力が印刷または公開に必要な場合は、FPOレンディションを元の画像に置き換える必要があります。 複数のFPO画像をそれぞれの元の画像で置き換える場合は、**_Windows/リンク_**&#x200B;パネルに移動して、元のアセットをダウンロードします。 元の画像をダウンロードしたら、「すべてのFPOを元の画像に置き換える」を選択します。

FPOレンディションは、元のアセットの軽量な代替です。 縦横比は同じですが、元の画像よりも小さいサイズです。 現在、InDesignは、次の画像タイプのFPOレンディションの読み込みのみをサポートしています。

+ JPEG
+ GIF
+ PNG
+ TIFF
+ PSD
+ BMP

FPOレンディションがAEMの特定のアセットで使用できない場合は、元の高解像度アセットが参照されます。 FPO画像の場合、ステータスFPOがInDesignリンクパネルに表示されます。

## AEM AssetsとのAdobeアセットリンク認証

AdobeIdentity Managementサービス(IMS)およびAdobe Experience ManagerオーサーのコンテキストでのAdobeアセットリンク認証の仕組み。

![Adobeアセットリンクのアーキテクチャ](assets/adobe-asset-link-article-understand.png)

1. Adobeアセットリンク拡張は、Adobe Creative Cloud Desktop Appを介してAdobeID管理サービス(IMS)への認証リクエストをおこない、成功するとBearerトークンを受け取ります。
1. Adobeアセットリンク拡張は、**手順1**&#x200B;で取得したBearerトークンを含むHTTP(S)を介して、拡張機能の設定JSONに指定されたスキーム(HTTP/HTTPS)、ホストおよびポートを使用してAEMオーサーに接続します。
1. AEM Bearer Authentication Handlerは、リクエストからBearerトークンを抽出し、AdobeIMSを使用して検証します。
1. AdobeIMSがBearerトークンを検証すると、AEMにユーザーが作成され（まだ存在しない場合）、AdobeIMSからプロファイルおよびグループ/メンバーシップデータを同期します。 AEMユーザーは、標準のAEMログイントークンが発行され、HTTP(S)応答のCookieとしてAdobeアセットリンク拡張に戻されます。
1. 後続のインタラクション( アセットの参照、検索、チェックイン/チェックアウトなど) AdobeAsset Link拡張機能を使用すると、AEMオーサーに対するHTTP(S)リクエストが生成され、AEMログイントークンを使用して、標準のAEM Token Authentication Handlerを使用して検証されます。

>[!NOTE]
>
>ログイントークンの有効期限が切れると、**手順1～5**&#x200B;が自動的にを呼び出し、Bearerトークンを使用したAdobeアセットリンク拡張の認証と、新しい有効なログイントークンの再発行をおこないます。

## その他のリソース

+ [AdobeアセットリンクWebサイト](https://www.adobe.com/jp/creativecloud/business/enterprise/adobe-asset-link.html)
