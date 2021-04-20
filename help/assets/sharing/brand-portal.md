---
title: ブランドポータルの使用
description: AEM AuthorとAEM Assetsブランドポータルの統合に関するビデオウォークスルー。
feature: Brand Portal
version: 6.3, 6.4, 6.5
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1774'
ht-degree: 45%

---


# ブランドポータルとAEM Assets{#using-brand-portal-with-aem-assets}の連携

Adobe Experience Manager(AEM)Assets Brand Portal統合のビデオガイド。

## Brand Portal 2019年9月の機能と機能強化

Brand Portalの2019年9月に最も顕著に導入されたのは、コンテンツ速度が向上し、Experience Managerの作成者とサードパーティのクリエイティブおよび寄稿者との間でアセットを簡単かつ迅速に交換できるアセットソーシングです。

### Brand Portal アセットソーシング{#asset-sourcing}

Brand Portalのアセットソーシングは、サードパーティのエージェンシーやチームからアセットを収集し、レビューや使用のためにExperience Managerの作成者にシームレスに同期します。

>[!VIDEO](https://video.tv.adobe.com/v/29365/?quality=12&learn=on)

*Asset Sourcingを使用するには、Experience Manager作成者6.5 SP2 (6.5.2)以降が必要です*

Experience Manager作成者でアセットソーシングを設定および設定する方法については、「[アセットソーシング用のExperience Manager作成者を有効にする](https://docs.adobe.com/content/help/en/experience-manager-brand-portal/using/asset-sourcing-in-brand-portal/configure-asset-sourcing-in-aem/brand-portal-enable-asset-sourcing.html)」を参照してください。

## Brand Portal 2019年2月の機能と機能強化{#brand-portal-features-and-enhancements-644}

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

Brand Portalの2019年2月リリースでは、テキスト検索とトップ顧客のリクエストに対する機能強化に重点を置いています。

### 検索の機能強化

ブランドポータルは、フィルタリングペインでプロパティ述語の部分テキスト検索を強化します。 部分テキスト検索を可能にするには、検索フォームの「プロパティの述語」で「部分検索」を有効にする必要があります。

部分テキスト検索およびワイルドカード検索について詳しくは、以下の説明を参照してください。

#### 部分フレーズ検索

フィルタリングウィンドウで、検索対象フレーズの一部分（1 つか 2 つの単語）のみを指定してアセットを検索できます。

**使用事例** :部分的なフレーズ検索は、検索したフレーズに含まれる単語の正確な組み合わせが不明な場合に便利です。

例えば、Brand Portal の検索フォームで、「プロパティの述語」を使用してアセットのタイトルの部分検索をおこなう場合、「camp」という単語を指定すると、タイトルフレーズで「camp」という単語を使用しているアセットがすべて返されます。

#### ワイルドカード検索

Brand Portal では、検索クエリに、検索対象フレーズの単語の一部とアスタリスク（*）を使用できます。

**使用例** ：検索語句に含まれる単語が正確にわからない場合は、ワイルドカード検索を使用して検索クエリの隙間を埋めることができます。

例えば、Brand Portal の検索フォームで、「プロパティの述語」を使用してアセットのタイトルの部分検索をおこなう場合に、「climb*」と指定すると、「climb」で始まる単語がタイトルフレーズで使用されているアセットがすべて返されます。

さらに、以下のような指定ができます。

* \*climpは、単語の末尾が文字で終わるアセットをタイトルフレーズ内に含むすべてのアセットを返します。
* \*climp\*は、文字を含む単語を持つすべてのアセットをタイトルフレーズ内に含めて返します。

#### フォルダー階層の有効化

管理者以外のユーザー（エディター、閲覧者、ゲストユーザー）がログインしたときにフォルダーをどのように表示するかを管理者が設定できるようになりました。管理ツールパネルの一般設定に「[フォルダー階層を有効化](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html)」設定が追加されています。この設定は次のように動作します。

* 有効にすると、ルートフォルダーから始まるフォルダーツリーは、管理者以外のユーザーに対して表示されます。 これにより、管理者と同じようなナビゲーション体験を提供できます。
* 無効にすると、共有ランディングページーのみがフォルダーに表示されます。

[フォルダ](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) 階層を有効にする機能（有効になっている場合）を使用すると、異なる階層と同じ名前を持つフォルダを区別できます。管理者以外のユーザーがログインすると、共有フォルダーの仮想親フォルダー（とその上位層）が表示されます。

共有フォルダーは、仮想フォルダー内のそれぞれのディレクトリ内で整理されます。仮想フォルダーは鍵のアイコン付きで表示されます。

仮想フォルダーのデフォルトのサムネールは最初の共有フォルダーのサムネール画像になることに注意してください。

### Dynamic Media ビデオレンディションのサポート

Dynamic Media ハイブリッドモードの AEM オーサーインスタンスを使用しているユーザーは、オリジナルのビデオファイルに加えて、Dynamic Media レンディションをプレビューしたりダウンロードしたりできます。

特定のテナントアカウントでダイナミックメディアレンディションのプレビューおよびダウンロードができるようにするには、管理者が管理ツールパネルのビデオ設定で Dynamic Media 設定（ダイナミックビデオを取得するためのビデオサービスの URL（Dynamic Media ゲートウェイの URL）と登録 ID）を指定する必要があります。

Dynamic Media ビデオは以下の場所でプレビューできます。

* アセットの詳細ページ
* アセットのカード表示
* リンク共有のプレビューページ

Dynamic Media ビデオエンコードは以下の場所からダウンロードできます。

* ブランドポータル
* 共有リンク

### Brand Portal への公開のスケジュール設定

[AEM（6.4.2.0）](https://helpx.adobe.com/jp/experience-manager/6-5/release-notes/sp-release-notes.html#main-pars_header_9658011)オーサーインスタンスから Brand Portal へのアセット（およびフォルダー）公開ワークフローのスケジュールに未来の日時を指定できるようになりました。

同様に、「Brand Portal で非公開」ワークフローのスケジュールを設定することで、公開されているアセットを未来の特定の日時にポータルから取り下げることができます。

### URL 中の設定可能なテナントエイリアス

組織のポータル URL に代替プレフィックスを含めて、カスタマイズされたポータル URL を取得することができます。既存のポータル URL 中のテナント名のエイリアスを取得するには、各組織からアドビサポートへ依頼する必要があります。

カスタマイズできるのは Brand Portal URL のプレフィックスのみであり、URL 全体でないことに注意してください。例えば、`wknd.brand-portal.adobe.com` という既存ドメインを持つ組織は、アドビに依頼することで `wkndinc.brand-portal.adobe.com` という URL を作成できます。

ただし、AEM オーサーインスタンスを[設定](https://helpx.adobe.com/jp/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)する際にはテナント ID URL のみを使用できます。テナントエイリアス（代替）URL は使用できません。

**使用例** :組織は、Adobeが提供するURLに固定するのではなく、ポータルURLをカスタマイズすることで、自社のブランディングニーズを満たすことができます。

## Brand Portal 2018年12月の機能と機能強化{#brand-portal-features-and-enhancements-642}

>[!VIDEO](https://video.tv.adobe.com/v/23707/?quality=9&learn=on)

### ゲストによるアクセス

AEM Brand Portal は、ゲストによるポータルへのアクセスを許可します。ゲストユーザーは資格情報がなくてもポータルに入ることができます。また、すべての公開フォルダーおよび公開コレクションにアクセスしたり、それらをダウンロードしたりすることができます。ゲストユーザーは、ライトボックス（プライベートコレクション）にアセットを追加して、ダウンロードすることができます。 また、管理者によって定められたスマートタグ検索や検索用述語を表示することもできます。ゲストセッションでは、ユーザーはコレクションや保存済みの検索を作成または共有したり、フォルダーやコレクションの設定にアクセスしたり、アセットをリンクとして共有したりすることはできません。

### 高速ダウンロード

Brand Portal ユーザーは、最大 25 倍の速度を実現できる Aspera の高速ダウンロードを活用し、世界中のどこにいてもシームレスにダウンロードをおこなうことができます。Brand Portal または共有 リンクに含まれていない場合、組織でダウンロードアクセラレーションが有効になっていれば、ダウンロードダイアログで「ダウンロードアクセラレーションを有効にする」オプションを選択する必要があります。

* [Brand Portalからのダウンロードを高速化するガイド](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Aspera接続テストサーバ](https://test-connect.asperasoft.com/)

### ユーザーログインレポート

ユーザーのログインを追跡するための新しいレポートが導入されました。ユーザーログインレポートは、組織が Brand Portal の委任管理者やその他のユーザーを監査および監視するし、目を光らせるのに便利です。

レポートログには、各ユーザーの名前、電子メールID、個人（管理者、ビューア、エディター、ゲスト）、グループ、最終ログイン、アクティビティステータス、ログイン数が表示されます。

### オリジナルレンディションへのアクセス

管理者は、元の画像ファイル(jpeg、tiff、png、bmp、gif、pjpeg、x-portable-anymap、x-portable-bitmap、x-portable-graymap、x-portable-pixmap、x-rgb、x-xbitmap、x-icon、image/photoshop、image/x-photoshop、image/vnd.adobe.photoshop)へのアクセスを制限し、低解像度レンディションからのレンディションにダウンロードできます。ブランドポータルまたは共有リンク。 このアクセス権は、管理ツールパネルのユーザーの役割ページの「グループ」タブからユーザーグループレベルで制御できます。

### 新しい設定

管理者が特定のテナントで次の機能を有効化／無効化できるよう、6 つの新しい設定が追加されました。

* ゲストによるアクセスを許可
* ユーザーによる Brand Portal へのアクセス要求を許可
* 管理者が Brand Portal からアセットを削除することを許可
* 公開コレクションの作成を許可
* 公開スマートコレクションの作成を許可
* ダウンロードアクセラレーションを許可

### その他の機能強化

* *カードフォルダーとリスト表示ーのフォルダー階層パス*  — ブランドポータルインスタンス内に保存されているフォルダーの場所をユーザーが把握できるようにします。異なるフォルダー階層内に同じ名前を持つフォルダーを区別しやすくします。
* *概要オプション*  — 管理者以外のユーザーが、アセット/フォルダーを選択し、ツールバーから概要オプションを選択して、アセット/フォルダーに関するメタデータを提供します。現在、タイトル、作成日、パスが表示されます。

### Adobe I/OホストUIを使用したoAuth統合の構成

Brand Portalは、Adobe I/O[https://legacy-oauth.cloud.adobe.io/](https://legacy-oauth.cloud.adobe.io/)のインターフェイスを使用してJWTアプリケーションを作成します。これにより、AEM AssetsがBrand Portalと統合できるようにoAuth統合を設定できます。 OAuth 統合を設定するための UI は、以前、`https://marketing.adobe.com/developer/` / でホストされていました。Brand Portal にアセットとコレクションを公開するための、AEM Assets と Brand Portal の統合について詳しくは、[AEM Assets と Brand Portal の統合の設定](https://helpx.adobe.com/jp/experience-manager/6-4/assets/using/brand-portal-configuring-integration.html)を参照してください。

## Brand Portal 2018年2月の機能と機能強化{#brand-portal-features-and-enhancements-632}

ブランドポータルとAEMの連携に向けた新機能が強化されました。

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

### ナビゲーションの改善

* AEMと連携し、Coral3 UIを使用する、アップグレードされたユーザーインターフェイス。
* 新しいAdobeロゴを使用して、管理ツールにすばやく簡単にアクセスできます。
* オーバーレイでの製品のナビゲーション
* 子フォルダから親フォルダへのクイックナビゲーション。
* Omnisearchオプションを使用して、管理ツールとコンテンツに移動できます。
* カード表示、リスト表示および列表示を使用すると、階層化されたフォルダーを簡単に参照できます。
* リスト表示でのアセットの並べ替えが、画面に表示されるアセットの数に制限されなくなりました。 フォルダー内のすべてのアセットが並べ替えられます。

### 検索の機能強化

* Omnisearch機能を使用すると、Brand Portal内のアセットやファイルをすばやく検索できます。
* また、Omnisearchは、特定のフォルダーまたは場所内のアセットを検索するオプションも提供します
* 検索を容易にするための自動サーチクエリ
* 追加のフィルターを使用してOmnisearchを改善します。 検索結果をスマートコレクションに保存し、後で検索に再度アクセスできるようにするオプションです。
* スマートタグ付きアセット検索をサポート
* AEMのスマートタグ付きアセットは、AEMからBrand Portalに共有でき、スマートタグを使用してBrand Portal内でアセットを検索できます。

### ファイル共有の強化

* ユーザーは、リンク共有オプションを使用してアセットを共有できます。
* アセットを共有する際に、ユーザーは各アセットの有効期限を設定できます。 共有するアセットをユーザーがより詳細に制御できるようにします。
* アセット共有リンクを持つ外部ユーザーは、画像をダウンロードし、そのプロパティを表示できます。
* 元のネストされたフォルダ階層は、ダウンロードしたアセットフォルダ用に保持されます。

### レポートおよび管理機能

* AEM AssetsのメタデータスキーマをAEMからBrand Portalに公開できるようになりました。
* 管理者は、ダウンロード済み、期限切れ、公開済みの3種類のレポートを作成および管理できます
* レポートに含める必要がある列を設定する機能。
* ブランドポータル内のアセット用の画像プリセットを作成します。
* 管理者の検索レールフォームまたは検索Formsを変更して、追加のフィルターオプションを含めることができます。
* ブランド用のカスタム壁紙の更新とプレビュー
* 使用状況レポートを参照して、ユーザー数、使用ストレージ容量、およびアセットの合計を確認します。

## その他のリソース{#additional-resources}

* [ブランドポータルの新機能](https://helpx.adobe.com/experience-manager/brand-portal/using/whats-new.html)
* [AEM作成者レプリケーションエージェント](https://helpx.adobe.com/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)
* [高速ダウンロードガイド](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [AEM AssetsブランドポータルAdobeドキュメント](https://helpx.adobe.com/jp/experience-manager/brand-portal/using/brand-portal.html)
* [AEM AssetsDynamic MediaAdobe文書](https://docs.adobe.com/docs/ja/aem/6-3/author/assets/dynamic-media.html)
* [Aspera接続をダウンロード](https://downloads.asperasoft.com/connect2/)
* [Aspera接続テストサーバ](https://test-connect.asperasoft.com/)