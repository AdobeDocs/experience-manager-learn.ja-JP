---
title: Brand Portalの使用
description: AEM オーサーとAEM Assets Brand Portalの統合のビデオウォークスルー。
feature: Brand Portal
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
exl-id: 42f13a19-52bf-413d-a141-63f1f0910dce
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '1764'
ht-degree: 45%

---

# AEM AssetsでのBrand Portalの使用{#using-brand-portal-with-aem-assets}

Adobe Experience Manager(AEM)Assets Brand Portal統合のビデオガイドです。

## Brand Portal 2019 年 9 月の機能および機能強化

Brand Portalが 2019 年 9 月に導入した最も注目すべきアセットソーシングは、コンテンツの速度を向上させ、Experience Manager作成者とサードパーティのクリエイティブおよびコントリビューターとの間でアセットを簡単かつ迅速に交換できるようにします。

### Brand Portal アセットソーシング{#asset-sourcing}

Brand Portalのアセットソーシングは、サードパーティの代理店やチームからアセットを収集し、それらをExperience Manager作成者にシームレスに同期してレビューと使用するために使用します。

>[!VIDEO](https://video.tv.adobe.com/v/29365/?quality=12&learn=on)

*Experience Managerソーシングを使用するには、アセットオーサー 6.5 SP2(6.5.2) 以降が必要です*

レビュー [Experience Managerソーシングのアセット作成者を有効にする](https://experienceleague.adobe.com/docs/experience-manager-brand-portal/using/asset-sourcing-in-brand-portal/brand-portal-asset-sourcing.html?lang=ja) を参照してください。

## Brand Portal 2019 年 2 月の機能および機能強化{#brand-portal-features-and-enhancements-644}

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

Brand Portalの 2019 年 2 月リリースでは、テキスト検索の機能強化と、お客様からのご要望への対応に重点を置いています。

### 検索の機能強化

Brand Portalは、フィルタリングウィンドウのプロパティの述語で部分テキスト検索を使用して検索を強化しました。 部分テキスト検索を可能にするには、検索フォームの「プロパティの述語」で「部分検索」を有効にする必要があります。

部分テキスト検索およびワイルドカード検索について詳しくは、以下の説明を参照してください。

#### 部分フレーズ検索

フィルタリングウィンドウで、検索対象フレーズの一部分（1 つか 2 つの単語）のみを指定してアセットを検索できます。

**使用事例** :部分フレーズ検索は、検索対象フレーズに出現する正確な単語の組み合わせが不明な場合に役立ちます。

例えば、Brand Portal の検索フォームで、「プロパティの述語」を使用してアセットのタイトルの部分検索を行う場合、「camp」という単語を指定すると、タイトルフレーズで「camp」という単語を使用しているアセットがすべて返されます。

#### ワイルドカード検索

Brand Portal では、検索クエリに、検索対象フレーズの単語の一部とアスタリスク（*）を使用できます。

**使用例** ：検索対象フレーズに出現する正確な単語が不明な場合は、ワイルドカード検索を使用して検索クエリを補完できます。

例えば、Brand Portal の検索フォームで、「プロパティの述語」を使用してアセットのタイトルの部分検索を行う場合に、「climb*」と指定すると、「climb」で始まる単語がタイトルフレーズで使用されているアセットがすべて返されます。

さらに、以下のような指定ができます。

* 「 climb 」と指定すると、「 climb 」で終わる単語がタイトルフレーズで使用されているアセットがすべて返されます。
* 「 climb 」と指定すると、「 climb 」という文字列を含む単語がタイトルフレーズで使用されているアセットがすべて返されます。

#### フォルダー階層の有効化

管理者以外のユーザー（編集者、閲覧者、ゲストユーザー）がログインしたときにフォルダーをどのように表示するかを管理者が設定できるようになりました。管理ツールパネルの一般設定に「[フォルダー階層を有効化](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html)」設定が追加されています。この設定は次のように動作します。

* 有効にすると、ルートフォルダーから始まるフォルダーツリーが管理者以外のユーザーに表示されます。 これにより、管理者と同じようなナビゲーション体験を提供できます。
* 無効にした場合、共有フォルダーのみがランディングページに表示されます。

[フォルダー階層の有効化](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) 機能（有効な場合）を使用すると、別の階層から共有されている同名のフォルダーを区別できます。 管理者以外のユーザーがログインすると、共有フォルダーの仮想親フォルダー（とその上位層）が表示されます。

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

* Brand Portal
* 共有リンク

### Brand Portal への公開のスケジュール設定

[AEM（6.4.2.0）](https://helpx.adobe.com/jp/experience-manager/6-5/release-notes/sp-release-notes.html#main-pars_header_9658011)オーサーインスタンスから Brand Portal へのアセット（およびフォルダー）公開ワークフローのスケジュールに未来の日時を指定できるようになりました。

同様に、「Brand Portal で非公開」ワークフローのスケジュールを設定することで、公開されているアセットを未来の特定の日時にポータルから取り下げることができます。

### URL 中の設定可能なテナントエイリアス

組織のポータル URL に代替プレフィックスを含めて、カスタマイズされたポータル URL を取得することができます。既存のポータル URL 中のテナント名のエイリアスを取得するには、各組織からアドビサポートへ依頼する必要があります。

カスタマイズできるのは Brand Portal URL のプレフィックスのみであり、URL 全体でないことに注意してください。例えば、`wknd.brand-portal.adobe.com` という既存ドメインを持つ組織は、アドビに依頼することで `wkndinc.brand-portal.adobe.com` という URL を作成できます。

ただし、AEM オーサーインスタンスを[設定](https://helpx.adobe.com/jp/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)する際にはテナント ID URL のみを使用できます。テナントエイリアス（代替）URL は使用できません。

**使用例** :Adobeから提供された URL をそのまま使用するのではなく、カスタマイズされたポータル URL を取得して、ブランディングのニーズを満たすことができます。

## Brand Portal 2018 年 12 月の機能および機能強化{#brand-portal-features-and-enhancements-642}

>[!VIDEO](https://video.tv.adobe.com/v/23707/?quality=9&learn=on)

### ゲストによるアクセス

AEM Brand Portal は、ゲストによるポータルへのアクセスを許可します。ゲストユーザーは資格情報がなくてもポータルに入ることができます。また、すべての公開フォルダーおよび公開コレクションにアクセスしたり、それらをダウンロードしたりすることができます。ゲストユーザーは、ライトボックス（非公開コレクション）にアセットを追加したり、同じアセットをダウンロードしたりできます。 また、管理者によって定められたスマートタグ検索や検索用述語を表示することもできます。ゲストセッションでは、ユーザーはコレクションや保存済みの検索を作成または共有したり、フォルダーやコレクションの設定にアクセスしたり、アセットをリンクとして共有したりすることはできません。

### 高速ダウンロード

Brand Portal ユーザーは、最大 25 倍の速度を実現できる Aspera の高速ダウンロードを活用し、世界中のどこにいてもシームレスにダウンロードをおこなうことができます。Brand Portal または共有 組織でダウンロードアクセラレーションが有効になっている場合は、ダウンロードダイアログで「ダウンロードアクセラレーションを有効化」オプションを選択する必要があります。

* [Brand Portalからのダウンロードを高速化するためのガイド](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Aspera Connect テストサーバー](https://test-connect.asperasoft.com/)

### ユーザーログインレポート

ユーザーのログインを追跡するための新しいレポートが導入されました。ユーザーログインレポートは、組織が Brand Portal の委任管理者やその他のユーザーを監査および監視するし、目を光らせるのに便利です。

レポートログには、各ユーザーの名前、電子メール ID、ペルソナ（管理者、閲覧者、編集者、ゲスト）、グループ、最終ログイン、アクティビティステータス、ログイン数が表示されます。

### オリジナルのレンディションへのアクセス

管理者は、元の画像ファイル (jpeg、tiff、png、bmp、gif、pjpeg、x-portable-anymap、x-portable-bitmap、x-portable-graymap、x-portable-pixmap、x-rgb、x-xpixmap、x-icon、image/photoshop、image/x-photoshop、image/vnd.adobe.photoshop) へのユーザーアクセスを制限し、Brand Portalからダウンロードする低解像度のレンディションにアクセスを許可できますまたは共有リンク このアクセス権は、管理ツールパネルのユーザーの役割ページの「グループ」タブからユーザーグループレベルで制御できます。

### 新しい設定

管理者が特定のテナントで次の機能を有効化／無効化できるよう、6 つの新しい設定が追加されました。

* ゲストによるアクセスを許可
* ユーザーによる Brand Portal へのアクセス要求を許可
* 管理者が Brand Portal からアセットを削除することを許可
* 公開コレクションの作成を許可
* 公開スマートコレクションの作成を許可
* ダウンロードアクセラレーションを許可

### その他の機能強化

* *カード表示およびリスト表示のフォルダー階層パス*  — ユーザーは、Brand Portalインスタンス内に保存されているフォルダーの場所を把握できます。 ユーザーが異なるフォルダー階層内で同じ名前を持つフォルダーを区別するのに役立ちます。
* *「概要」オプション*  — 管理者以外のユーザーが、アセットやフォルダーを選択し、ツールバーの「概要」オプションを選択することで、アセットやフォルダーに関するメタデータを提供します。 現在、には、タイトル、作成日およびパスが表示されます

### Adobe I/Oホスト OAuth 統合を設定する UI

Brand PortalはAdobe I/Oを使用 [https://legacy-oauth.cloud.adobe.io/](https://legacy-oauth.cloud.adobe.io/) JWT アプリケーションを作成するインターフェイス。AEM AssetsとBrand Portalの統合を許可するように OAuth 統合を設定できます。 OAuth 統合を設定するための UI は、以前、`https://marketing.adobe.com/developer/` / でホストされていました。Brand Portal にアセットとコレクションを公開するための、AEM Assets と Brand Portal の統合について詳しくは、[AEM Assets と Brand Portal の統合の設定](https://helpx.adobe.com/jp/experience-manager/6-4/assets/using/brand-portal-configuring-integration.html)を参照してください。

## Brand Portal 2018 年 2 月の機能および機能強化{#brand-portal-features-and-enhancements-632}

Brand PortalとAEMの連携に向けた新機能が強化されました。

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

### ナビゲーションの改善

* AEMに対応し、Coral3 UI を使用する、アップグレードされたユーザーインターフェイス。
* 新しい管理ロゴから、管理ツールにすばやく簡単にアクセスできます。Adobe
* オーバーレイを使用した製品ナビゲーション
* 子フォルダーから親フォルダーにクイックナビゲーション。
* オムニサーチオプションを使用して、管理ツールおよびコンテンツに移動できる。
* カード表示、リスト表示、列表示を使用すると、ネストされたフォルダを簡単に参照できます。
* リスト表示でのアセットの並べ替えを、画面に表示されるアセットの数に制限されなくなりました。 フォルダー内のアセットがすべて並べ替えられます。

### 検索機能の強化

* オムニサーチ機能を使用すると、Brand Portal内でアセットやファイルをすばやく検索できます。
* オムニサーチには、特定のフォルダーまたは場所内のアセットを検索するオプションも用意されています
* 検索を容易にする自動サーチクエリ
* 追加のフィルターでオムニサーチを改善する。 検索結果をスマートコレクションに保存し、後で再度検索できるようにするオプション。
* スマートタグ付きアセット検索をサポート
* AEMのスマートタグ付きアセットは、AEMからBrand Portalに共有し、Brand Portal内でのアセット検索にスマートタグを使用できます。

### ファイル共有の改善

* ユーザーは、「リンク共有」オプションを使用してアセットを共有できます。
* アセットを共有する際に、ユーザーは各アセットの有効期限を設定できます。 ユーザーが共有するアセットをより詳細に制御できるようにします。
* アセット共有リンクを持つ外部ユーザーは、画像をダウンロードして、そのプロパティを表示できます。
* ネストされた元のフォルダー階層は、ダウンロードしたアセットフォルダー用に保持されます。

### レポートおよび管理機能

* AEM AssetsのメタデータスキーマをAEMからBrand Portalに公開できるようになりました。
* 管理者は、3 種類のレポート（ダウンロードされたアセット、期限切れのアセット、公開されたアセット）を作成および管理できます
* レポートに含める必要がある列を設定する機能です。
* Brand Portal内でアセットの画像プリセットを作成します。
* 管理検索レールフォームまたは検索Formsを変更して、追加のフィルターオプションを含めることができます。
* ブランドのカスタム壁紙の更新とプレビュー
* 使用状況レポートを参照して、ユーザー数、使用されたストレージ領域、合計アセット数を確認します。

## その他のリソース{#additional-resources}

* [Brand Portalの新機能](https://helpx.adobe.com/experience-manager/brand-portal/using/whats-new.html)
* [AEM オーサーレプリケーションエージェント](https://helpx.adobe.com/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)
* [高速ダウンロードガイド](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [AEM Assets Brand PortalAdobeドキュメント](https://helpx.adobe.com/jp/experience-manager/brand-portal/using/brand-portal.html)
* [AEM Assets Dynamic MediaAdobeドキュメント](https://experienceleague.adobe.com/docs/?lang=ja)
* [Aspera Connect をダウンロード](https://downloads.asperasoft.com/connect2/)
* [Aspera Connect テストサーバー](https://test-connect.asperasoft.com/)
