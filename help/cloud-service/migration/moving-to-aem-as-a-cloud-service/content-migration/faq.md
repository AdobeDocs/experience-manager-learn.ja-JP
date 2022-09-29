---
title: AEMas a Cloud Serviceコンテンツ移行に関する FAQ
description: AEM as a Cloud Serviceへのコンテンツの移行に関するよくある質問に対する回答を示します。
version: Cloud Service
doc-type: article
feature: Migration
topic: Migration
role: Architect, Developer
level: Beginner
kt: 11200
thumbnail: kt-11200.jpg
source-git-commit: b2656329270ac90458dbc25bb05f39bf76921f26
workflow-type: tm+mt
source-wordcount: '2283'
ht-degree: 1%

---


# AEMas a Cloud Serviceコンテンツ移行に関する FAQ

AEM as a Cloud Serviceへのコンテンツの移行に関するよくある質問に対する回答を示します。

## 用語

+ **AEMaaCS**: [AEMas a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/introduction.html)
+ **BPA**: [ベストプラクティスアナライザー](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/best-practices-analyzer/overview-best-practices-analyzer.html)
+ **CTT**: [コンテンツ転送ツール](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html)
+ **CAM**: [Cloud Acceleration Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/getting-started-cam.html)
+ **IMS**: [Identity Management System](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/ims-support.html)
+ **DM**: [Dynamic Media](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/dynamicmedia/dm-journey/dm-journey-part1.html)

CTT 関連のAdobeサポートチケットを作成する際に、詳細を提供するには、以下のテンプレートを使用してください。

![コンテンツ移行Adobeサポートチケットテンプレート](../../assets/faq/adobe-support-ticket-template.png) { align=&quot;center&quot; }

## コンテンツ移行に関する一般的な質問

### Q:コンテンツをCloud ServicesとしてAEMに移行する様々な方法を教えてください。

次の 3 つの方法を使用できます

+ コンテンツ転送ツールの使用 (AEM 6.3 以降→ AEMaaCS)
+ パッケージマネージャー (AEM → AEMaaCS) を使用
+ アセットの一括読み込みサービス (S3/Azure → AEMaaCS) が標準で用意されています。

### Q:CTT を使用して転送できるコンテンツの量に制限はありますか。

いいえ。CTT をツールとして使用すると、AEMソースから抽出して AEMaaCS に取り込むことができます。 ただし、AEMaaCS プラットフォームには、移行前に考慮すべき特定の制限があります。

詳しくは、 [クラウド移行の前提条件](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/prerequisites-content-transfer-tool.html).

### Q:ソースシステムから最新の BPA レポートを取得していますが、これを使用するにはどうすればよいですか？

レポートを CSV 形式で書き出し、Cloud Acceleration Manager にアップロードします。 [IMS Org に関連付けられている](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-acceleration-manager/using-cam/getting-started-cam.html). 次に、レビュープロセスを次のように実行します。 [準備段階で概要を説明](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-acceleration-manager/using-cam/cam-readiness-phase.html).

ツールが提供するコードとコンテンツの複雑さの評価を確認し、コードリファクタリングバックログまたはクラウド移行評価につながる関連するアクション項目をメモしておいてください。

### Q:ソースオーサーで抽出し、AEMaaCS オーサーに取り込んでパブリッシュすることをお勧めしますか？

オーサー層とパブリッシュ層の間で 1:1 の抽出と取り込みを実行することを常にお勧めします。 ただし、ソースの実稼動作成者を抽出して、それを開発、ステージ、実稼動の CS に取り込むことは可能です。

### Q:CTT を使用して、ソースAEMから AEMaaCS にコンテンツを移行するのに要する時間を見積もる方法はありますか。

移行プロセスは、インターネット帯域幅、CTT プロセスに割り当てられるヒープ、使用可能な空きメモリ、各ソースシステムに固有のディスク IO に依存するので、Proof Of の移行を早期に実行し、そのデータポイントを推定に基づいて推定することをお勧めします。

### Q:CTT 抽出プロセスを開始した場合、ソースAEMのパフォーマンスはどのように影響を受けますか。

CTT ツールは、最大 4gb ヒープを取る独自の Java™プロセスで実行されます。このプロセスは、OSGi 設定を通じて設定できます。 この数は変わる場合がありますが、Java™プロセスに対して grep を実行し、それを確認できます。

AZCopy がインストールされている場合、または Pre copy option / validation 機能が有効になっている場合、AZCopy プロセスは CPU サイクルを消費します。

jvm とは別に、このツールはディスク IO を使用してデータを一時的な一時領域に保存し、抽出サイクル後にクリーンアップします。 CTT ツールは、RAM、CPU、およびディスク IO 以外にも、ソースシステムのネットワーク帯域幅を使用して、Azure BLOB ストアにデータをアップロードします。

CTT 抽出プロセスがとるリソースの量は、ノード数、BLOB の数および集計されたサイズによって異なります。 数式を提供するのは困難なので、移行元サーバーのアップサイズ要件を特定するには、移行の検証を小さく実行することをお勧めします。

クローン環境を移行に使用する場合、実稼動サーバーのリソース使用率に影響を与えませんが、実稼動環境と複製間のコンテンツの同期に関する独自の欠点があります

### Q:ソースオーサーシステムで、ユーザーがオーサーインスタンスを認証するための SSO が設定されています。 この場合、CTT のユーザーマッピング機能を使用する必要がありますか？

短い答えは、「**はい**&quot;.

CTT の抽出と取り込み **なし** ユーザーマッピングは、コンテンツに関連する原則（ユーザー、グループ）をソースAEMから AEMaaCS に移行するだけです。 ただし、Adobe IMSに存在し、AEMaaCS インスタンスに対する（プロビジョニングされた）アクセス権を持つこれらのユーザー (ID) が正常に認証される必要があります。 のジョブ [ユーザーマッピングツール](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-migration/content-transfer-tool/user-mapping-tool/overview-user-mapping-tool.html) は、認証と認証が連携するように、ローカルAEMユーザーを IMS ユーザーにマッピングします。

この場合、SAML ID プロバイダーは、認証ハンドラーを使用してAEMに直接送信するのではなく、フェデレーテッド/Enterprise IDを使用するようにAdobe IMSに対して設定されます。

### Q:ソースオーサーシステムには、ローカルのAEMユーザーを使用してオーサーインスタンスに対して認証をおこなうための基本的な認証が設定されています。 この場合、CTT のユーザーマッピング機能を使用する必要がありますか？

短い答えは、「**はい**&quot;.

ユーザーマッピングを使用しない CTT 抽出と取り込みでは、関連する原則（ユーザー、グループ）であるコンテンツがソースAEMから AEMaaCS に移行されます。 ただし、Adobe IMSに存在し、AEMaaCS インスタンスに対する（プロビジョニングされた）アクセス権を持つこれらのユーザー (ID) が正常に認証される必要があります。 のジョブ [ユーザーマッピングツール](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-migration/content-transfer-tool/user-mapping-tool/overview-user-mapping-tool.html) は、認証と認証が連携するように、ローカルAEMユーザーを IMS ユーザーにマッピングします。

この場合、ユーザーは個人用のAdobe IDを使用し、IMS 管理者は AEMaaCS へのアクセスを提供するためにAdobe IDを使用します。

### Q:CTT のコンテキストでの「ワイプ」および「上書き」という用語の意味は何ですか。

のコンテキストでは [抽出段階](https://experienceleague.adobe.com/docs/experience-manager-cloud-servicemoving/cloud-migration/content-transfer-tool/extracting-content.html)の場合は、以前の抽出サイクルのステージングコンテナ内のデータを上書きするか、差分（追加/更新/削除）をそのコンテナに追加するかのどちらかのオプションがあります。 ステージングコンテナは何もありませんが、移行セットに関連付けられた BLOB ストレージコンテナです。 各移行セットには、独自のステージングコンテナが用意されています。

のコンテキストでは [取得段階](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-migration/content-transfer-tool/ingesting-content.html?lang=ja)の場合、オプションは+で、AEMaaCS のコンテンツリポジトリ全体を置き換えるか、ステージング移行コンテナから差分（追加/更新/削除）コンテンツを同期します。

### Q:ソースシステムには、複数の Web サイト、関連アセット、ユーザー、グループがあります。 AEMaaCS に段階的に移行することは可能ですか？

はい、可能ですが、以下に関する慎重な計画が必要です。

+ サイト、アセットがそれぞれの階層にあると想定した移行セットの作成
   + 1 つの移行セットの一部としてすべてのアセットを移行することが許容できるかどうかを確認してから、それらを使用しているサイトを段階的に取り込みます
+ 現在の状態では、オーサーの取り込みプロセスにより、パブリッシュ層でコンテンツを提供できる場合でも、オーサーインスタンスがコンテンツオーサリングで使用できなくなります
   + つまり、オーサーへの取り込みが完了するまで、コンテンツオーサリングアクティビティは凍結されます

移行を計画する前に、ドキュメントに記載されている追加の抽出および取り込みプロセスを確認してください。

### Q:AEMaaCS オーサーインスタンスまたはパブリッシュインスタンスに取り込みがおこなわれても、エンドユーザーは Web サイトを使用できるようになりますか？

はい。コンテンツ移行アクティビティによってエンドユーザーのトラフィックが中断されることはありません。 ただし、オーサーの取り込みは、コンテンツのオーサリングが完了するまでフリーズします。

### Q:BPA レポートは、見つからない元のレンディションに関連する項目を表示します。 抽出前にソース上でクリーンアップする必要がありますか？

はい。元のレンディションが見つからない場合は、アセットバイナリが最初に正しくアップロードされないことを意味します。 不正なデータと見なす場合は、必要に応じてパッケージマネージャーを使用して確認、バックアップし、抽出を実行する前にソースAEMから削除してください。 不正なデータは、アセットの処理手順で悪い結果になります。

### Q:BPA レポートに、見つからない項目が関連しています `jcr:content` フォルダーのノード。 私は彼らをどうすれば良いのですか？

条件 `jcr:content` はフォルダーレベルで見つからず、処理プロファイルなどの設定を反映するアクションがあります。 親からはこのレベルで壊れる。 見つからない理由を確認してください `jcr:content`. これらのフォルダーは移行できますが、ユーザーの操作性が低下し、後で不要なトラブルシューティングサイクルが発生することに注意してください。

### Q:移行セットを作成しました。 そのサイズを調べることは可能ですか？

はい、 [サイズを確認](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html#migration-set-size) 機能（CTT の一部）に含まれています。

### Q:移行（抽出、取り込み）を実行しています。 抽出したすべてのコンテンツが Target に取り込まれていることを検証できますか？

はい、 [検証](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/validating-content-transfers.html) 機能（CTT の一部）

### Q:お客様は、AEMaaCS Dev から AEMaaCS Stage や AEMaaCS Prod など、AEMaaCS 環境間でコンテンツを移動する必要があります。 これらの使用例でコンテンツ転送ツールを使用できますか？

残念ながら、いいえ。 CTT の使用例は、オンプレミス/AMS でホストされているAEM 6.3 以降のソースから AEMaaCS クラウド環境にコンテンツを移行する場合です。 [CTT ドキュメントをお読みください](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html).

### Q:抽出時に予想される問題の種類

抽出フェーズは複雑なプロセスで、期待どおりに動作するために複数の側面が必要です。 発生する可能性のある様々な問題の種類と、その軽減方法を認識することで、コンテンツ移行の全体的な成功が向上します。

パブリックドキュメントは、学習に基づいて継続的に改善されていますが、いくつかの高レベルの問題カテゴリと考えられる基礎的な理由を次に示します。

![AEMas a Cloud Serviceのコンテンツ移行抽出の問題](../../assets/faq/extraction-issues.jpg) { align=&quot;center&quot; }

### Q:取り込み中に予想される問題の種類

取り込み段階はクラウドプラットフォームで完全におこなわれ、AEMaaCS インフラストラクチャにアクセスできるリソースのヘルプが必要です。 詳しいヘルプを参照するには、サポートチケットを作成してください。

考えられる問題カテゴリを次に示します（これを排他的なリストと見なさないでください）。

![AEMas a Cloud Serviceのコンテンツ移行の取り込み問題](../../assets/faq/ingestion-issues.jpg) { align=&quot;center&quot; }



### Q:CTT が機能するには、送信元サーバーにアウトバウンドインターネット接続が必要ですか？

短い答えは、「**はい**&quot;.

CTT プロセスでは、以下のリソースに接続する必要があります。

+ ターゲット AEM as a Cloud Service 環境：`author-p<program_id>-e<env_id>.adobeaemcloud.com`
+ Azure BLOB ストレージサービス：`casstorageprod.blob.core.windows.net`
+ ユーザーマッピング IO エンドポイント：`usermanagement.adobe.io`

詳しくは、ドキュメントを参照してください。 [ソース接続](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html#source-environment-connectivity).

## アセット処理に関するDynamic Media関連の質問

### Q:AEMaaCS での取り込み後、アセットは自動的に再処理されますか。

いいえ。アセットを処理するには、再処理のリクエストを開始する必要があります。

### Q:AEMaaCS での取り込み後、アセットのインデックスは自動的に再作成されますか。

はい。アセットのインデックスは、AEMaaCS で使用可能なインデックス定義に基づいて再作成されます。

### Q:ソースAEMはDynamic Mediaと統合されています。 コンテンツ移行の前に考慮する必要がある特定の事項はありますか？

はい、ソースAEMがDynamic Media統合を使用する場合は、次の点を考慮してください。

+ AEMaaCS は、Dynamic Media Scene7モードのみをサポートします。 ソースシステムがハイブリッドモードの場合、Scene7モードへの DM 移行が必要です。
+ ソースクローンインスタンスから移行する方法の場合は、CTT に使用するクローンの DM 統合を無効にしても安全です。 この手順は、DM への書き込みを避けるか、DM トラフィックの負荷を避けるためのものです。
+ CTT は、移行セットのノード、メタデータをソースAEMから AEMaaCS に移行します。 DM に対して直接操作を実行しません。

### Q:DM 統合がソースAEMに存在する場合、異なる移行方法は何ですか。

前述の質問と回答を読んでから、

（これら 2 つのオプションが可能ですが、これら 2 つに限定されるわけではありません）。 UAT、パフォーマンステスト、利用可能な環境に対するお客様のアプローチ方法、およびクローンを移行に使用しているかどうかによって異なります。 この二つを議論の出発点として考えて下さい

**オプション 1**

ソース環境内のアセット/ノードの数が低い (100 K) 場合は、抽出と取り込みを含め、24 時間+ 72 時間にわたって移行できると仮定して、より適切なアプローチは次のとおりです。

+ 本番環境から直接移行を実行
+ を使用して、AEMaaCS に最初の抽出と取り込みを実行する `wipe=true`
   + この手順は、すべてのノードとバイナリを移行します
+ オンプレミス/AMS Prod オーサーでの作業を続ける
+ 今後は、移行サイクルの検証を他のすべての `wipe=true`
   + この操作は、完全なノードストアを移行しますが、BLOB 全体とは異なり、変更された BLOB のみを移行します。 以前の BLOB セットは、ターゲット AEMaaCS インスタンスの Azure BLOB ストアにあります。
   + 移行期間、ユーザーマッピング、テスト、その他すべての機能の検証を測定するには、この移行の配達確認を使用します
+ 最後に、運用開始の週の前に、ワイプ=true 移行を実行します
   + AEMaaCS でのDynamic Mediaの接続
   + AEMオンプレミスのソースから DM 設定を切断します

このオプションを使用すると、On-prem Dev → AEMaaCS Dev など、移行を 1 対 1 で実行できます。 および DM 設定を各環境から移動

（移行をクローンから実行する予定の場合）

**オプション 2**

+ 実稼動オーサーのクローンを作成し、クローンから DM 設定を削除
+ オンプレミスクローンの移行→ AEMaaCS Dev / Stage
   + 実稼動用 DM の会社を AEMaaCS Dev/Stage に簡単に接続して、検証を実施します。
   + DM 接続がアクティブな間は、AEMaaCS へのアセットの取り込みを避けます
   + これにより、CTT（DM 固有の検証）を検証できます。
+ AEMaaCS でテストが完了したら、
   + オンプレミスステージから AEMaaCS ステージへのワイプ移行の実行

オンプレミスの開発から AEMaaCS 開発へのワイプ移行を実行します。

上記の方法は、移行時間の測定にのみ使用できますが、後でクリーンアップする必要があります。

## その他のリソース

+ [Cloud でのExperience Managerへの移行に関するヒントとテクニック (Summit 2022)](https://business.adobe.com/summit/2022/sessions/tips-and-tricks-for-migrating-to-experience-manage-tw109.html)

+ [CTT エキスパートシリーズビデオ](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-servicemigration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.html)

+ [他の AEMaaCS トピックに関する Expert シリーズビデオ](https://experienceleague.adobe.com/docs/experience-manager-learncloud-service/aem-experts-series.html)
