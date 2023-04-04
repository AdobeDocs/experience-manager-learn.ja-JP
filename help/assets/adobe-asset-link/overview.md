---
title: AdobeAsset Link とAEM
description: Adobe Experience Manager Assets は、デザイナーやクリエイティブユーザーが好みのAdobe Creative Cloudデスクトップアプリケーション内で使用できます。 Adobe Creative Cloud for enterpriseのAdobeAsset Link 拡張機能は、Adobe XD、Photoshop、InDesign、IllustratorなどのCreative Cloudツール内でAEM Assets のメタデータを検索および参照、並べ替え、プレビュー、アップロード、チェックアウト、変更、チェックイン、表示する機能を拡張します。
feature: Adobe Asset Link
version: 6.4, 6.5, Cloud Service
topic: Content Management
role: User
level: Beginner
thumbnail: 28988.jpg
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '990'
ht-degree: 2%

---


# AdobeAsset Link 3.0

Adobe Experience Manager Assets は、デザイナーやクリエイティブユーザーが好みのAdobe Creative Cloudデスクトップアプリケーション内で使用できます。

Adobe Creative Cloud for enterpriseのAdobeAsset Link 拡張機能は、Creative Cloudアプリケーション内でAEM Assets のメタデータを検索および参照、並べ替え、プレビュー、アップロード、チェックアウト、変更、チェックイン、表示する機能を拡張します。

## Adobeアセットリンク機能

+ AdobeAsset Link は、AEM AssetsおよびAssets Essentialsと統合できます。
+ AdobeAsset Link は、クラウドベースのAEM環境 (AEM Assets as a Cloud ServiceおよびAssets Essentials) への接続を自動設定します
+ Adobeアセットリンクは、Adobe Creative Cloudアプリケーション内で機能する拡張機能です。

   + Adobe XD
   + Adobe Photoshop
   + Adobe Illustrator
   + Adobe InDesign

+ AEMのAdobeEnterprise IDまたはFederated IDを使用したへの自動認証
+ AEMでのデジタルアセットの参照と検索
+ パネルを使用して、AEM内のアセットに関するファイルの詳細にアクセスします。
   + サムネール
   + 基本メタデータ
   + バージョン
+ アセットをレイアウトに配置、ダウンロード、ドラッグ&amp;ドロップ
+ アセットをAEMからチェックアウトし、Creative CloudAssets アカウント内でアセットを操作 (WIP) して変更
+ アセットの変更が完了し、新しいバージョンがAEMに反映されたら、アセットをAEMに再度チェックインする
+ アプリ内パネルの Asset LinkAdobeからAEM内のアセットを検索します。
+ Asset Link パネルからAEM Assetsのコレクションとスマートコレクションを直接参照する
+ 新しく作成したアセットをパネルから直接AEMに追加
+ アセットをInDesignフレームに直接ドラッグ&amp;ドロップ

## アセットのInDesign

Adobeアセットリンクは、InDesignの Asset Link とAEM間のAdobeの直接リンクをサポートします。 InDesignのダイレクトリンクをサポートしているので、__リンク場所__ または __コピーを配置__) をクリックするか、AdobeAsset Link パネルを使用して、デジタルアセットをAEMからInDesignにドラッグ&amp;ドロップします。 また、 *For Placement Only+(FPO) レンディションも導入されています。

>[!VIDEO](https://video.tv.adobe.com/v/28988?quality=12&learn=on)

>[!NOTE]
>
>Adobe Creative CloudのEnterprise IDまたはFederated IDのみを使用してください。 必ず [AEM for Asset Link のAdobe](https://helpx.adobe.com/jp/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html).

次のいずれかのオプションを使用して、InDesignレイアウトにアセットを配置できます。

+ **コピーを配置**  — アセットを（「コピーを配置」オプションを使用して）埋め込むと、バイナリをローカルシステムにダウンロードした後で、元のInDesignのコピーがアセットレイアウトに配置されます。 Adobeアセットリンクでは、埋め込まれたコピーと元のアセットの間のリンクは保持されません。 元のアセットがAEMで変更されている場合は、埋め込まれたアセットをInDesignファイルから削除し、AEMからアセットを再埋め込みする必要があります。

+ **リンク場所** -InDesignドキュメントを操作する場合、アセットを直接埋め込むだけでなく、AEMからアセットを参照することもできます（コンテキストメニューの「コピーを配置」オプションを使用）。 アセットの参照を使用すると、他のユーザーと共同作業したり、元のアセットに対しておこなわれた更新をAEMに組み込んだりできます。 AEMからアセットを参照するには、コンテキストメニューの「リンクを配置」オプションを使用します。

### 配置専用の画像

Adobeの Asset Link を使用して、AEMからInDesignドキュメントに大きなアセットファイルを配置する場合、クリエイティブユーザーは、配置操作を開始してから数秒待つ必要があります。 これは、ユーザーエクスペリエンス全体に影響します。 Adobeアセットリンクを使用すると、AEMから元のアセットの低解像度の画像を一時的に配置できるので、画像の配置にかかる時間を短縮できます。 同時に、ユーザーエクスペリエンス全体と生産性が向上します。 低い解像度の画像は一時的に配置され、印刷や公開に最終出力が必要な場合は、FPO レンディションを元の画像に置き換える必要があります。 複数の FPO 画像をそれぞれの元の画像で置き換える場合は、に移動します。 **_Windows /リンク_** パネルを開き、元のアセットをダウンロードします。 元の画像をダウンロードしたら、「すべての FPO をオリジナルに置き換える」を選択します。

FPO レンディションは、元のアセットの軽量な代替物です。 縦横比は同じですが、元の画像に比べてサイズが小さくなります。 現在、InDesignは、次の画像タイプに対する FPO レンディションの読み込みのみをサポートしています。

+ JPEG
+ GIF
+ PNG
+ TIFF
+ PSD
+ BMP

FPO レンディションがAEMの特定のアセットで使用できない場合は、元の高解像度アセットが代わりに参照されます。 FPO 画像の場合、ステータス FPO が [InDesignリンク ] パネルに表示されます。

## AEM AssetsとのAdobeアセットリンク認証

AdobeIdentity Managementサービス (IMS) およびAdobe Experience ManagerオーサーのコンテキストでのAdobeAsset Link 認証の仕組み。

![AdobeAsset Link のアーキテクチャ](assets/adobe-asset-link-article-understand.png)

1. Adobeアセットリンク拡張は、Adobe Creative Cloud Desktop App を介してAdobeID 管理サービス (IMS) への認証リクエストをおこない、成功すると Bearer トークンを受け取ります。
1. Adobeアセットリンク拡張機能は、HTTP(S) を介して AEM オーサーに接続します。この中には、 **手順 1**&#x200B;を使用し、拡張機能の設定 JSON で提供されるスキーム (HTTP/HTTPS)、ホストおよびポートを使用します。
1. AEM Bearer Authentication Handler は、リクエストから Bearer トークンを抽出し、Adobe IMSで検証します。
1. Adobe IMSが Bearer トークンを検証すると、AEMにユーザーが作成され（存在しない場合）、Adobe IMSからプロファイルとグループ/メンバーシップのデータを同期します。 AEMユーザーは、標準AEMログイントークンが発行され、HTTP(S) 応答の Cookie としてAdobeAsset Link 拡張機能に返されます。
1. 後続のインタラクション ( アセットの参照、検索、チェックイン/チェックアウトなど ) AdobeAsset Link 拡張機能を使用すると、AEM オーサーに対する HTTP(S) リクエストが生成され、標準のAEMトークン認証ハンドラーを使用してAEMログイントークンを使用して検証されます。

>[!NOTE]
>
>ログイントークンの有効期限が切れた場合、 **手順 1 ～ 5** が自動的にを呼び出し、Bearer トークンを使用してAdobeAsset Link 拡張の認証をおこない、新しい有効なログイントークンを再発行します。

## その他のリソース

+ [Adobeアセットリンク Web サイト](https://www.adobe.com/jp/creativecloud/business/enterprise/adobe-asset-link.html)
