---
title: Brand Portal の使用
description: AEM オーサーと AEM Assets Brand Portal の統合に関するビデオウォークスルー。
feature: Brand Portal
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-06-15T00:00:00Z
doc-type: Feature Video
exl-id: 42f13a19-52bf-413d-a141-63f1f0910dce
duration: 2460
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1702'
ht-degree: 100%

---

# AEM Assets と Brand Portal の使用{#using-brand-portal-with-aem-assets}

Adobe Experience Manager（AEM）Assets Brand Portal 統合のビデオガイド。

## Brand Portal 2019年9月の機能および機能強化

Brand Portal の 2019年9月リリースで最も注目すべきは、アセットソーシングを導入したことです。これにより、コンテンツの速度が向上し、Experience Manager の作成者とサードパーティのクリエイターやコントリビューターとの間で、アセットを簡単かつ迅速に交換できるようになりました。

### Brand Portal アセットソーシング{#asset-sourcing}

Brand Portal のアセットソーシングを使用すると、サードパーティの代理店やチームからアセットを収集し、それらを Experience Manager 作成者にシームレスに同期してレビューおよび使用できます。

>[!VIDEO](https://video.tv.adobe.com/v/29365?quality=12&learn=on)

*アセットソーシングを使用するには、Experience Manager Author 6.5 SP2（6.5.2）以降が必要です*

Experience Manager Author でアセットソーシングを構成および設定する方法については、[Experience Manager Author をアセットソーシング用に有効にする](https://experienceleague.adobe.com/docs/experience-manager-brand-portal/using/asset-sourcing-in-brand-portal/brand-portal-asset-sourcing.html?lang=ja)を参照してください。

## Brand Portal 2019年2月の機能および機能強化{#brand-portal-features-and-enhancements-644}

>[!VIDEO](https://video.tv.adobe.com/v/26354?quality=12&learn=on)

Brand Portal の 2019年2月のリリースでは、テキスト検索の機能強化と、お客様からの主なご要望への対応に重点を当てています。

### 検索の機能強化

Brand Portal は、フィルタリングペインのプロパティ述語で部分テキスト検索を使用することにより、検索を強化します。部分テキスト検索を可能にするには、検索フォームの「プロパティの述語」で「部分検索」を有効にする必要があります。

部分テキスト検索およびワイルドカード検索について詳しくは、以下の説明を参照してください。

#### 部分フレーズ検索

フィルタリングペインで検索したフレーズの一部（1 つか 2 つの単語）のみを指定して、アセットを検索できるようになりました。

**使用事例**：部分フレーズ検索は、検索対象フレーズに出現する正確な単語の組み合わせが不明な場合に役立ちます。

例えば、Brand Portal の検索フォームで、「プロパティの述語」を使用してアセットのタイトルの部分検索を行う場合、「camp」という単語を指定すると、タイトルフレーズで「camp」という単語を使用しているアセットがすべて返されます。

#### ワイルドカード検索

Brand Portal では、検索クエリに、検索対象フレーズの単語の一部とアスタリスク（*）を使用できます。

**使用事例**：検索対象フレーズに出現する正確な単語が不明な場合は、ワイルドカード検索を使用して検索クエリを補完できます。

例えば、Brand Portal の検索フォームで、「プロパティの述語」を使用してアセットのタイトルの部分検索を行う場合に、「climb*」と指定すると、「climb」で始まる単語がタイトルフレーズで使用されているアセットがすべて返されます。

さらに、以下のような指定ができます。

* 「\*climb」と指定すると、「climb」という文字で終わる単語をタイトルフレーズで使用しているアセットがすべて返されます。
* 「\*climb\*」と指定すると、「climb」という文字を含んだ単語をタイトルフレーズで使用しているアセットがすべて返されます。

#### フォルダー階層の有効化

管理者以外のユーザー（編集者、閲覧者、ゲストユーザー）がログインしたときにフォルダーをどのように表示するかを管理者が設定できるようになりました。
管理ツールパネルの一般設定に「[フォルダー階層を有効化](https://helpx.adobe.com/jp/experience-manager/brand-portal/using/brand-portal-general-configuration.html)」設定が追加されています。設定を：

* 有効にすると、ルートフォルダーから始まるフォルダーツリーが管理者以外のユーザーにも表示されます。そのため、これらのユーザーは管理者と同様のナビゲーション操作が可能になります。
* 無効にすると、ランディングページには共有フォルダーのみが表示されます。

「[フォルダー階層を有効化](https://helpx.adobe.com/jp/experience-manager/brand-portal/using/brand-portal-general-configuration.html)」機能を有効にすると、異なる階層から共有された、同じ名前のフォルダーを区別することができます。管理者以外のユーザーがログインすると、共有フォルダーの仮想親フォルダー（とその上位層）が表示されます。

共有フォルダーは、仮想フォルダー内のそれぞれのディレクトリ内で整理されます。これらの仮想フォルダーには、鍵のアイコンが付きます。

仮想フォルダーのデフォルトのサムネールは最初の共有フォルダーのサムネール画像になることに注意してください。

### Dynamic Media ビデオレンディションのサポート

Dynamic Media ハイブリッドモードの AEM オーサーインスタンスを使用しているユーザーは、オリジナルのビデオファイルに加えて、Dynamic Media レンディションをプレビューしたりダウンロードしたりできます。

特定のテナントアカウントでダイナミックメディアレンディションのプレビューおよびダウンロードができるようにするには、管理者が管理ツールパネルのビデオ設定で Dynamic Media 設定（ダイナミックビデオを取得するためのビデオサービスの URL（Dynamic Media ゲートウェイの URL）と登録 ID）を指定する必要があります。

Dynamic Media ビデオは以下の場所でプレビューできます。

* アセットの詳細ページ
* アセットのカード表示
* リンク共有のプレビューページ

Dynamic Media ビデオのエンコードは、次の場所からダウンロードできます。

* Brand Portal
* 共有リンク

### Brand Portal への公開のスケジュール設定

アセット（およびフォルダー）を [AEM（6.4.2.0）](https://helpx.adobe.com/jp/experience-manager/6-5/release-notes/sp-release-notes.html#main-pars_header_9658011)オーサーインスタンスから Brand Portal へ公開するワークフローのスケジュールに未来の日時を指定できるようになりました。

同様に、「Brand Portal で非公開」ワークフローのスケジュールを設定することで、公開されているアセットを未来の特定の日時にポータルから取り下げることができます。

### URL を編集可能なテナントエイリアス

組織は、URL に代わりのプレフィックスを付けることで、ポータルの URL をカスタマイズすることができます。既存のポータル URL のテナント名のエイリアスを取得するには、各組織からアドビサポートへ依頼する必要があります。

カスタマイズできるのは Brand Portal URL のプレフィックスのみであり、URL 全体でないことに注意してください。
例えば、`wknd.brand-portal.adobe.com` という既存ドメインを持つ組織は、アドビに依頼することで `wkndinc.brand-portal.adobe.com` という URL を作成できます。

ただし、AEM オーサーインスタンスを[設定](https://helpx.adobe.com/jp/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)する際にはテナント ID URL のみを使用できます。テナントエイリアス（代替）URL は使用できません。

**使用事例**：アドビから提供された URL をそのまま使用するのではなく、ポータルの URL をカスタマイズして、ブランドのニーズを満たすことができます。

## Brand Portal 2018年12月の機能および機能強化{#brand-portal-features-and-enhancements-642}

>[!VIDEO](https://video.tv.adobe.com/v/23707?quality=12&learn=on)

### ゲストによるアクセス

AEM Brand Portal を使用すると、ゲストがポータルにアクセスできます。ゲストユーザーは資格情報がなくてもポータルに入ることができます。また、すべての公開フォルダーおよび公開コレクションにアクセスしたり、それらをダウンロードしたりすることができます。ゲストユーザーは、Light-box（非公開コレクション）にアセットを追加したり、Lightbox からアセットをダウンロードしたりすることができます。また、スマートタグ検索および管理者が設定した検索用述語を表示することもできます。ゲストセッションでは、ユーザーはコレクションや保存済みの検索を作成または共有したり、フォルダーやコレクションの設定にアクセスしたり、アセットをリンクとして共有したりすることはできません。

### ダウンロードの高速化

Brand Portal ユーザーは、最大 25 倍の速度を実現できる Aspera の高速ダウンロードを利用して、世界中のどこにいてもシームレスにダウンロードをおこなうことができます。Brand Portal または共有リンクからアセットを高速にダウンロードするには、ダウンロードダイアログで「ダウンロードの高速化を有効化」オプションを選択する必要があります。ただし、その組織でダウンロードの高速化が可能になっている必要があります。

* [Brand Portal からのダウンロードを高速化するためのガイド](https://helpx.adobe.com/jp/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Aspera Connect テストサーバー](https://test-connect.asperasoft.com/)

### ユーザーログインレポート

ユーザーログインを追跡する新しいレポートが追加されました。ユーザーログインレポートは、組織が Brand Portal の委任管理者やその他のユーザーを監査および監視するし、目を光らせるのに便利です。

レポートログには、各ユーザーの名前、メール ID、ペルソナ（管理者、閲覧者、編集者、ゲスト）、グループ、最終ログイン、アクティビティステータス、ログイン回数が表示されます。

### オリジナルのレンディションへのアクセス

管理者は、元の画像ファイル (jpeg、tiff、png、bmp、gif、pjpeg、x-portable-anymap、x-portable-bitmap、x-portable-graymap、x-portable-pixmap、x-rgb、x-xpixmap、x-icon、image/photoshop、image/x-photoshop、image/vnd.adobe.photoshop) へのユーザーアクセスを制限し、Brand Portal または共有リンクからダウンロードした低解像度のレンディションへのアクセスを許可できます。このアクセスは、管理ツールパネルのユーザーロールページの「グループ」タブから、ユーザーグループレベルで制御できます。

### 新しい設定

管理者が特定のテナントで次の機能を有効化／無効化できるよう、6 つの新しい設定が追加されました。

* ゲストによるアクセスを許可
* ユーザーによる Brand Portal へのアクセス要求を許可
* 管理者が Brand Portal からアセットを削除することを許可
* 公開コレクションの作成を許可
* 公開スマートコレクションの作成を許可
* ダウンロードの高速化を許可

### その他の機能強化

* *カード表示およびリスト表示のフォルダー階層パス* - ユーザーが Brand Portal インスタンス内に保存されているフォルダーの場所を知ることができるようにします。ユーザーが異なるフォルダー階層にある同じ名前のフォルダーを区別することができます。
* *概要オプション* - アセットやフォルダーを選択して、ツールバーの「概要」オプションを選択することで、管理者以外のユーザーにアセットやフォルダーに関するメタデータを提供します。現在は、タイトル、作成日、パスが表示されるようになっています

### OAuth 統合を設定するための UI を Adobe I/O がホスト

Brand Portal では、Adobe I/O [https://legacy-oauth.cloud.adobe.io/](https://legacy-oauth.cloud.adobe.io/) インターフェイスを使用して作成した JWT アプリケーションを使用すると、OAuth 統合を設定することにより、AEM Assets と Brand Portal を統合できるようになります。OAuth 統合を設定するための UI は、以前、`https://marketing.adobe.com/developer/` / でホストされていました。Brand Portal にアセットとコレクションを公開するための、AEM Assets と Brand Portal の統合について詳しくは、[AEM Assets と Brand Portal の統合の設定](https://helpx.adobe.com/jp/experience-manager/6-4/assets/using/brand-portal-configuring-integration.html)を参照してください。

## Brand Portal 2018年2月の機能および機能強化{#brand-portal-features-and-enhancements-632}

Brand Portal と AEM の連携に向けて強化された機能が新しく追加されました。

>[!VIDEO](https://video.tv.adobe.com/v/26354?quality=12&learn=on)

### ナビゲーションの向上

* AEM と連携し Coral3 UI を使用するように、ユーザーインターフェイスがアップグレードされました。
* 新しいアドビロゴから管理ツールにすばやく容易にアクセスできるようになりました。
* オーバーレイを使用した製品ナビゲーションが可能になりました。
* 子フォルダーから親フォルダーにすばやく移動できるようになりました。
* 管理ツールおよびコンテンツに移動するためのオムニサーチオプションが用意されました。
* カード表示、リスト表示および列表示で、ネストされたフォルダーを容易に参照できるようになりました。
* リスト表示でのアセットの並べ替えが、画面に表示されているアセットの数に関係なく実行できるようになりました。フォルダー内のアセットがすべて並べ替えられます。

### 検索の向上

* オムニサーチ機能を使用すると、Brand Portal 内でアセットやファイルをすばやく検索できます。
* オムニサーチには、特定のフォルダーまたは場所内のアセットを検索するオプションも用意されています。
* キーワード候補の自動提示で検索しやすくなりました。
* 追加のフィルターでオムニサーチが改善されました。検索結果をスマートコレクションに保存し、後で参照できるようにするオプションが用意されました。
* スマートタグ付きアセット検索をサポートしています。
* AEM のスマートタグ付きアセットは、AEM から Brand Portal に共有することができ、Brand Portal 内でのアセット検索にスマートタグを使用できます。

### ファイル共有の向上

* ユーザーは、リンク共有オプションを使用してアセットを共有できます。
* アセットを共有する際に、ユーザーはアセットごとに有効期限を設定できるようになりました。共有するアセットをユーザーがよりきめ細かく制御できます。
* アセット共有リンクを持つ外部ユーザーが、画像をダウンロードしたり、画像のプロパティを確認したりできます。
* ネストされた元のフォルダー階層は、ダウンロードしたアセットフォルダーで保持されます。

### レポート機能と管理機能

* AEM Assets のメタデータスキーマを AEM から Brand Portal に公開できるようになりました。
* 管理者は、3 種類のレポート（ダウンロードされたアセット、期限切れのアセット、公開されたアセット）を作成および管理できます。
* レポートに含める必要がある列を設定できます。
* Brand Portal 内でアセットの画像プリセットを作成できます。
* 管理者の検索パネルフォームまたは検索フォームを変更して、フィルタリングオプションを追加できます。
* ブランドのカスタム壁紙の更新とプレビューが可能です。
* 使用状況レポートでユーザー数、使用済みストレージ容量および合計アセット数を確認できます。

## その他のリソース{#additional-resources}

* [Brand Portal の新機能](https://helpx.adobe.com/jp/experience-manager/brand-portal/using/whats-new.html)
* [AEM オーサーのレプリケーションエージェント](https://helpx.adobe.com/jp/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)
* [ダウンロードの高速化ガイド](https://helpx.adobe.com/jp/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [AEM Assets Brand Portal のアドビドキュメント](https://helpx.adobe.com/jp/experience-manager/brand-portal/using/brand-portal.html)
* [AEM Assets Dynamic Media のアドビドキュメント](https://experienceleague.adobe.com/docs/?lang=ja)
* [Aspera Connect のダウンロード](https://downloads.asperasoft.com/connect2/)
* [Aspera Connect テストサーバー](https://test-connect.asperasoft.com/)
