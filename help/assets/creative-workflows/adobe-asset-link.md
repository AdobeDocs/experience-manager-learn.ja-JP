---
title: Adobe Asset Link と AEM
description: Adobe Experience Manager Assets は、デザイナーやクリエイティブユーザーがお気に入りの Adobe Creative Cloud デスクトップアプリケーション内で使用することができます。Adobe Creative Cloud for enterprise の Adobe Asset Link 拡張機能は、Adobe XD、Photoshop、InDesign、Illustrator などの Creative Cloud ツール内で AEM アセットのメタデータを検索、参照、並べ替え、プレビュー、アセットのアップロード、チェックアウト、変更、チェックインおよび表示する機能を拡張します。
feature: Adobe Asset Link
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Content Management
role: User
level: Beginner
thumbnail: 28988.jpg
jira: KT-8413, KT-3707
last-substantial-update: 2022-06-25T00:00:00Z
doc-type: Feature Video
exl-id: 6c49f8c2-f468-4b29-b7b6-029c8ab39ce9
duration: 1027
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1039'
ht-degree: 100%

---

# Adobe Asset Link 3.0

Adobe Experience Manager Assets は、デザイナーやクリエイティブユーザーがお気に入りの Adobe Creative Cloud デスクトップアプリケーション内で使用することができます。

Adobe Creative Cloud for enterprise の Adobe Asset Link 拡張機能は、Creative Cloud アプリケーション内で AEM アセットのメタデータを検索、参照、並べ替え、プレビュー、アセットのアップロード、チェックアウト、変更、チェックインおよび表示する機能を拡張します。

>[!TIP]
>
> [Adobe XD プレミアムトレーニングプログラム](https://helpx.adobe.com/jp/support/xd.html)が、アセットリンクを Adobe Experience Manager ワークフローと統合するのにどのように役立つかをご覧ください。

## Adobe Asset Link と AEM クリエイティブワークフロー

次のビデオは、Adobe Creative Cloud アプリケーションで作業し、Adobe Asset Link を使用して AEM と直接統合するクリエイティブが使用する一般的なワークフローを示しています。

>[!VIDEO](https://video.tv.adobe.com/v/335927?quality=12&learn=on)

## Adobe Asset Link の機能

+ Adobe Asset Link は、AEM Assets および Assets Essentials と統合されています。
+ Adobe Asset Link は、クラウドベースの AEM 環境（AEM Assets as a Cloud Service および Assets Essentials）への接続を自動設定します
+ Adobe Asset Link は、Adobe Creative Cloud アプリケーション内で機能する拡張機能です。

   + Adobe XD
   + Adobe Photoshop
   + Adobe Illustrator
   + Adobe InDesign

+ Adobe Enterprise ID または Federated ID を使用した AEM への自動認証
+ AEM 内のデジタルアセットの参照と検索
+ パネルを使用して、AEM に存在するアセットのファイル詳細にアクセスします。
   + サムネール
   + 基本メタデータ
   + バージョン
+ レイアウトへのアセットの配置、ダウンロードまたはドラッグ＆ドロップ
+ アセットを AEM からチェックアウトし Creative Cloud Assets アカウント内で操作（WIP）して、アセットを変更
+ アセットの変更が完了し、新しいバージョンがAEMに反映されたら、アセットを AEM に再度チェックインします。
+ Adobe Asset Link のアプリ内パネルから AEM 内のアセットを検索
+ Asset Link パネルから AEM Assets コレクションおよびスマートコレクションを直接参照
+ 新規作成したアセットをパネルから直接 AEM に追加
+ アセットを InDesign フレームに直接ドラッグ＆ドロップ

## InDesign へのアセットの配置

Adobe Asset Link は、Adobe Asset Link と AEM の間の InDesign の直接リンクをサポートしています。InDesign の直接リンクのサポートにより、Adobe Asset Link パネルを使用して、AEM から InDesign にデジタルアセットを配置（__リンクを配置__&#x200B;または&#x200B;__コピーを配置__）またはドラッグ＆ドロップすることができます。また、*For Placement Only+（FPO）レンディションも導入します。

>[!VIDEO](https://video.tv.adobe.com/v/28988?quality=12&learn=on)

>[!NOTE]
>
>Adobe Creative Cloud の Enterprise ID または Federated ID のみを使用します。 必ず [AEM for Asset Link を設定してください](https://helpx.adobe.com/jp/enterprise/using/adobe-asset-link.html)。

次のいずれかのオプションを使用して、InDesign レイアウトにアセットを配置することができます。

+ **コピーを配置** -（「コピーを配置」オプションを使用して）アセットを埋め込むと、バイナリをローカルシステムにダウンロードした後で、オリジナルアセットのコピーが InDesign レイアウトに配置されます。Adobe Asset Link では、埋め込まれたコピーとオリジナルのアセットの間のリンクは保持されません。 オリジナルアセットが AEM で変更された場合は、埋め込まれたアセットを InDesign ファイルから削除し、アセットを AEM から再度埋め込む必要があります。

+ **リンクを配置** - InDesign ドキュメントを操作する場合は、（コンテキストメニューの「コピーを配置」オプションを使用して）アセットを直接埋め込むだけでなく、AEM 内のアセットを参照することもできます。アセットを参照すると、他のユーザーと共同作業したり、AEM 内のオリジナルアセットに加えられた更新を取り込んだりすることができます。AEM 内のアセットを参照するには、コンテキストメニューの「リンクを配置」オプションを使用します。

### 配置専用の画像の場合

Adobe Asset Link を使用して大容量のアセットファイルを AEM から InDesign ドキュメントに配置する場合、クリエイティブユーザーは配置操作を開始してから数秒待つ必要があります。これは、ユーザーエクスペリエンス全体に影響します。Adobe Asset Link を使用すると、AEM 内のオリジナルアセットの低解像度画像を一時的に配置できるので、画像の配置にかかる時間を短縮することができます。と同時に、ユーザーエクスペリエンス全体と生産性が向上します。 この低解像度画像は一時的に配置され、印刷や公開に最終的な出力が必要な場合は、FPO レンディションをオリジナル画像に置き換える必要があります。 複数の FPO 画像をそれぞれのオリジナル画像で置き換える場合は、**_Windows／リンク_**&#x200B;パネルに移動し、オリジナルアセットをダウンロードします。 オリジナル画像をダウンロードしたら、「すべての FPO をオリジナルに置き換える」を選択します。

FPO レンディションは、オリジナルアセットの代わりとなる軽量のアセットです。 アスペクト比は同じですが、オリジナル画像に比べてサイズが小さくなります。 現在 InDesign では、次の画像タイプの FPO レンディションのみ、読み込みをサポートしています。

+ JPEG
+ GIF
+ PNG
+ TIFF
+ PSD
+ BMP

AEM 内の特定のアセットに対応する FPO レンディションが入手できない場合は、オリジナルの高解像度アセットが代わりに参照されます。 FPO 画像の場合、ステータス FPO が InDesign リンクパネルに表示されます。

## AEM Assets を使用した Adobe Asset Link の認証

Adobe Identity Management Services（IMS）および Adobe Experience Manager オーサーのコンテキストで Adobe Asset Link の認証が機能する仕組み。

![Adobe Asset Link のアーキテクチャ](assets/adobe-asset-link-article-understand.png)

1. Adobe Asset Link 拡張機能は、Adobe Creative Cloud デスクトップアプリを介して Adobe Identity Management System（IMS）への認証リクエストを行い、成功するとベアラートークンを受け取ります。
1. Adobe Asset Link 拡張機能は、HTTP(S) を介して AEM オーサーに接続します。この中には、拡張機能の設定 JSON で提供されるスキーム（HTTP／HTTPS）、ホストおよびポートを使用して&#x200B;**手順 1**&#x200B;で取得したベアラートークンが含まれています。
1. AEM ベアラー認証ハンドラーは、リクエストからベアラートークンを抽出し、Adobe IMS で検証します。
1. Adobe IMS がベアラートークンを検証すると、AEM にユーザーが作成され（まだ存在しない場合）、Adobe IMS からプロファイルとグループ／メンバーシップのデータを同期します。 AEM ユーザーに標準の AEM ログイントークンが発行されます。このトークンは、HTTP／HTTPS 応答の Cookie として Adobe Asset Link 拡張機能に送り返されます。
1. Adobe Asset Link との後続のインタラクション（アセットの参照、検索、チェックイン／チェックアウトなど）の結果、HTTP(S) リクエストが AEM オーサーに送られ、標準の AEM トークン認証ハンドラーで AEM ログイントークンを使用して検証されます。

>[!NOTE]
>
>ログイントークンの有効期限が切れると、**手順 1～5** が自動的に起動され、ベアラートークンを使用して Adobe Asset Link 拡張機能の認証を行い、新しい有効なログイントークンを再発行します。

## その他のリソース

+ [Adobe Asset Link の web サイト](https://www.adobe.com/creativecloud/business/enterprise/adobe-asset-link.html)
