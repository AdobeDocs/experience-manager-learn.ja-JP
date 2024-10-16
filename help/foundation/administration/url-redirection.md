---
title: URL リダイレクト
description: AEMで URL リダイレクトを実行するための様々なオプションについて説明します。
version: 6.4, 6.5, Cloud Service
topic: Development, Administration
feature: Operations, Dispatcher
role: Developer, Architect
level: Intermediate
jira: KT-11466
last-substantial-update: 2022-10-14T00:00:00Z
index: y
doc-type: Article
exl-id: 8e64f251-e5fd-4add-880e-9d54f8e501a6
duration: 164
source-git-commit: 3cc9b4fa0a30d36638a8c28a73663ffa455ba4a3
workflow-type: ht
source-wordcount: '877'
ht-degree: 100%

---

# URL リダイレクト

URL リダイレクトは、web サイト操作の一環として一般的な側面です。アーキテクトや管理者は、柔軟性とリダイレクトデプロイメント時間の高速化を実現する、URL リダイレクトの管理方法と管理場所に関する最適なソリューションを見つける必要があります。

[AEM（6.x）aka AEM Classic](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/dispatcher-tutorial/chapter-2) と [AEM as a Cloud Service](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/overview/architecture) インフラストラクチャに精通している必要があります。主な違いは次のとおりです。

1. AEM as a Cloud Service には[組み込み CDN](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn) がありますが、お客様は AEM の管理による CDN の前に CDN（BYOCDN）を提供できます。
1. オンプレミスまたは Adobe Managed Services（AMS）のどちらの AEM 6.x にも AEM の管理による CDN が含まれていないため、お客様は独自の CDN を用意する必要があります。

他の AEM サービス（AEM オーサー、パブリッシュ、Dispatcher）については、AEM 6.x と AEM as a Cloud Service の間で概念的に似ています。

AEM の URL リダイレクトソリューションは次のとおりです。

|                                                   | AEM プロジェクトコードとしての管理とデプロイ | マーケティング／コンテンツチームによる変更機能 | AEM as Cloud Service との互換性 | リダイレクトの実行場所 |
|---------------------------------------------------|:-----------------------:|:---------------------:|:---------------------:| :---------------------:|
| [AEM 管理の CDN による Edge](#at-edge-via-aem-managed-cdn) | ✔ | ✘ | ✔ | Edge／CDN（組み込み） |
| [独自の CDN（BYOCDN）による Edge](#at-edge-via-bring-your-own-cdn) | ✘ | ✘ | ✔ | Edge／CDN（BYOCDN） |
| [Dispatcher 設定としての Apache `mod_rewrite` ルール](#apache-mod_rewrite-module) | ✔ | ✘ | ✔ | Dispatcher |
| [ACS Commons - リダイレクトマップマネージャ](#redirect-map-manager) | ✘ | ✔ | ✘ | Dispatcher |
| [ACS Commons - リダイレクトマネージャー](#redirect-manager) | ✘ | ✔ | ✔ | AEM |
| [`Redirect` ページのプロパティ](#the-redirect-page-property) | ✘ | ✔ | ✔ | AEM |


## ソリューションオプション

以下は、web サイトの訪問者のブラウザーに近い順に示したソリューションオプションです。

### AEM 管理の CDN による Edge {#at-edge-via-aem-managed-cdn}

このオプションは、AEM as a Cloud Service のお客様のみが利用できます。

[AEM 管理の CDN](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn) には、Edge レベルのリダイレクトソリューションが提供されており、オリジンとの往復時間を短縮できます。[クライアントサイドのリダイレクト](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#client-side-redirectors)機能を使用すると、AEM プロジェクトコードにリダイレクトルールを設定し、[Config パイプライン](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/security/traffic-filter-and-waf-rules/how-to-setup#deploy-rules-through-cloud-manager)を使用してデプロイできます。CDN 設定ファイル（`cdn.yaml`）のサイズは 100 KB 以下にする必要があります。

Edge または CDN レベルでリダイレクトを管理すると、パフォーマンスが向上します。

### Edge と独自の CDN

一部の CDN サービスでは、Edge レベルのリダイレクトソリューションが提供されており、オリジンとの往復時間を短縮できます。詳しくは、「[Akamai Edge リダイレクター](https://techdocs.akamai.com/cloudlets/docs/what-edge-redirector)」、「[AWS CloudFront の機能](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html)」を参照してください。Edge レベルのリダイレクト機能については、CDN サービスプロバイダーにお問い合わせください。

Edge レベルまたは CDN レベルでリダイレクトを管理するとパフォーマンスが向上しますが、リダイレクトが AEM の一部ではなく、個別のプロジェクトとして管理されます。問題を回避するには、リダイレクトルールの管理とデプロイする明確に定義されたプロセスが重要です。


### Apache `mod_rewrite` モジュール

一般的なソリューションは、[Apache モジュール mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html) を使用します。この [AEM プロジェクトアーキタイプ](https://github.com/adobe/aem-project-archetype)は、[AEM 6.x](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.ams#file-structure) と [AEM as a Cloud Service](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud#file-structure) プロジェクトの両方に Dispatcher プロジェクト構造を提供します。デフォルト（不変）とカスタムの書き換えルールは `conf.d/rewrites` フォルダーで定義され、`conf.d/dispatcher_vhost.conf` ファイル経由でポート `80` をリッスンする `virtualhosts` に対する書き換えエンジンはオンになります。実装例は、[AEM WKND サイトプロジェクト](https://github.com/adobe/aem-guides-wknd/tree/main/dispatcher/src/conf.d/rewrites)で入手できます。

AEM as a Cloud Service では、これらのリダイレクトルールが AEM コードの一部として管理され、Cloud Manager [web 層設定パイプライン](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines)または[フルスタックパイプライン](https://experienceleague.adobe.com/ja/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines)を使用してデプロイされます。したがって、AEM プロジェクト固有のプロセスは、リダイレクトルールの管理、デプロイ、トレースを実行するために使用されます。

ほとんどの CDN サービスは、`Cache-Control` または `Expires` ヘッダーに応じて、HTTP 301 および 302 リダイレクトをキャッシュします。これは、Apache/Dispatcher で発生する最初のリダイレクト後の往復を回避するのに役立ちます。


### ACS AEM Commons

URL リダイレクトを管理するために [ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/) 内で使用できる 2 つの機能があります。ACS AEM Commons はコミュニティが運営するオープンソースプロジェクトで、Adobe ではサポートされていません。

#### リダイレクトマップマネージャー

[リダイレクトマップマネージャー](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html?lang=ja)を使用すると、AEM 6.x 管理者は、Apache Web サーバーに直接アクセスしたり、Apache web サーバーを再起動したりすることなく、[Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html) ファイルを簡単に管理および公開できます。この機能を使用すると、ユーザーは、開発チームや AEM デプロイメントの手助けを借りずに、AEM のコンソールからリダイレクトルールを作成、更新、および削除できます。リダイレクトマップマネージャは **AEM as a Cloud Service と互換性がありません**。

#### リダイレクトマネージャ

[リダイレクトマネージャ](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html?lang=ja)を使用すると、AEM のユーザーは、AEM からのリダイレクトを簡単に管理および公開できます。この実装は Java™サーブレットフィルターに基づいているので、一般的な JVM リソースの使用になります。また、この機能により、AEM 開発チームや AEM のデプロイメントに依存する必要がなくなります。リダイレクトマネージャーは **AEM as a Cloud Service** および **AEM 6.x** と互換性があります。最初のリダイレクトされたリクエストは、デフォルトで 301/302（ほとんどの）CDN のキャッシュ 301/302 を生成するために AEM パブリッシュサービスにヒットする必要があります。

### この `Redirect` ページプロパティ

[詳細タブ](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/sites-console/page-properties.html?lang=ja) のすぐに使える（OOTB）`Redirect` ページプロパティにより、コンテンツ作成者は現在のページのリダイレクト先を定義できます。このソリューションは、ページごとのリダイレクトシナリオに最適で、ページのリダイレクトを一元的に表示および管理する場所はありません。

## 実装に最適なソリューション

次に、適切なソリューションを決定するためのいくつかの条件を示します。また、組織の IT およびマーケティングプロセスが、適切なソリューションの選択に役立ちます。

1. マーケティングチームまたはスーパーユーザーが、AEM 開発チームおよび AEM デプロイメントを使用せずにリダイレクトルールを管理できるようにします。
1. 変更またはリスク軽減を管理、検証、追跡、および元に戻すプロセス。
1. **CDN サービスを介したエッジでの配信** ソリューションの&#x200B;_内容領域専門家_&#x200B;の利用可能性。
