---
title: URL リダイレクト
description: AEMで URL のリダイレクトを実行するための様々なオプションについて説明します。
version: 6.4, 6.5, Cloud Service
topic: Development, Administration
feature: Operations, Dispatcher
role: Developer, Architect
level: Intermediate
kt: 11466
last-substantial-update: 2022-10-14T00:00:00Z
index: y
source-git-commit: d5645e975aa290392348cc69d078b24921a7d13a
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 3%

---


# URL リダイレクト

URL のリダイレクトは、Web サイトの操作の一環として一般的な側面です。 アーキテクトや管理者は、柔軟性と迅速なリダイレクトデプロイメント時間を提供する、URL リダイレクトの管理方法と管理場所に関する最適なソリューションを見つける必要があります。

詳しくは、 [AEM 6.x)](https://experienceleague.adobe.com/docs/experience-manager-learn/dispatcher-tutorial/chapter-2.html#the-%E2%80%9Clegacy%E2%80%9D-setup) および [AEMas a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/architecture.html#runtime-architecture) インフラストラクチャ 主な違いは次のとおりです。

1.  AEMas a Cloud Service [組み込み CDN](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn.html?lang=ja)ただし、お客様はAEMが管理する CDN の前に CDN(BYOCDN) を提供できます。
1.  AEM 6.x( オンプレミスと Adobe Managed Services(AMS) のどちらでもAEMが管理する CDN が含まれていない場合 )。お客様は独自の CDN を持ち込む必要があります。

他のAEMサービス（AEM オーサー/パブリッシュ、および Dispatcher）は、AEM 6.x とAEM as a Cloud Serviceの間では概念的に似ています。

AEM URL のリダイレクトソリューションは次のとおりです。

|  | AEMプロジェクトコードとして管理および導入 | マーケティング/コンテンツチームによる変更機能 | AEM as aCloud Service互換 | リダイレクトの実行場所 |
|---------------------------------------------------|:-----------------------:|:---------------------:|:---------------------:| :---------------------:|
| [経由でエッジで独自の CDN を持ち込む](#at-edge-via-bring-your-own-cdn) | ✘ | ✘ | ✔ | Edge/CDN |
| [Apache `mod_rewrite` Dispatcher 設定としてのルール ](#apache-mod_rewrite-module) | ✔ | ✘ | ✔ | Dispatcher |
| [ACS Commons — リダイレクトマップマネージャ](#redirect-map-manager) | ✘ | ✔ | ✘ | Dispatcher |
| [ACS Commons — リダイレクトマネージャー](#redirect-manager) | ✘ | ✔ | ✔ | AEM |


## ソリューションオプション

以下は、Web サイトの訪問者のブラウザーに近い順にソリューションを選択する場合のオプションです。

### At Edge via bring your own CDN

一部の CDN サービスは、エッジレベルでリダイレクトソリューションを提供するので、オリジンへのラウンドトリップを減らします。 詳しくは、 [Akamai Edge リダイレクター](https://techdocs.akamai.com/cloudlets/docs/what-edge-redirector), [AWS CloudFront の機能](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-functions.html). Edge レベルのリダイレクト機能については、CDN サービスプロバイダーにお問い合わせください。

Edge レベルまたは CDN レベルでのリダイレクトの管理には、パフォーマンス上のメリットがありますが、AEMの一部ではなく、個別のプロジェクトとして管理されます。 リダイレクトルールを管理およびデプロイするためのよく考えられたプロセスは、問題を回避するために非常に重要です。


### Apache `mod_rewrite` モジュール

一般的なソリューションは [Apache モジュール mod_rewrite](https://httpd.apache.org/docs/current/mod/mod_rewrite.html). この [AEM Project Archetype](https://github.com/adobe/aem-project-archetype) は、両方に対して dispatcher プロジェクト構造を提供します [AEM 6.x](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.ams#file-structure) および [AEMas a Cloud Service](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud#file-structure) プロジェクト。 デフォルト（不変）およびカスタムの書き換えルールは、 `conf.d/rewrites` フォルダーと書き換えエンジンが `virtualhosts` ポートでリッスンする `80` 経由 `conf.d/dispatcher_vhost.conf` ファイル。 実装例は、 [AEM WKND Sites Project](https://github.com/adobe/aem-guides-wknd/tree/main/dispatcher/src/conf.d/rewrites).

AEM as a Cloud Serviceでは、これらのリダイレクトルールはAEMコードの一部として管理され、Cloud Manager を介してデプロイされます [Web 層設定パイプライン](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#web-tier-config-pipelines) または [フルスタックパイプライン](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines.html#full-stack-pipeline). したがって、AEMプロジェクト固有のプロセスは、リダイレクトルールの管理、デプロイ、トレースをおこなうために実行されています。

ほとんどの CDN サービスは、HTTP 301 および 302 リダイレクトを、それぞれの `Cache-Control` または `Expires` ヘッダー。 これは、Apache/Dispatcher からの最初のリダイレクト後の往復を避けるのに役立ちます。


### ACS AEM Commons

内では 2 つの機能を使用できます [ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/) :URL リダイレクトを管理します。 ACS AEM Commons はコミュニティが運営するオープンソースプロジェクトで、Adobeではサポートされていません。

#### リダイレクトマップマネージャ

[リダイレクトマップマネージャ](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-map-manager/index.html) AEM 6.x 管理者が簡単に管理および公開できるようにします [Apache RewriteMap](https://httpd.apache.org/docs/2.4/rewrite/rewritemap.html) Apache Web サーバーに直接アクセスしたり、Apache Web サーバーの再起動を必要としないファイルを削除できます。 この機能を使用すると、ユーザーは、開発チームやAEMのデプロイメントの手助けを借りずに、AEMのコンソールからリダイレクトルールを作成、更新および削除できます。 リダイレクトマップマネージャが **NOT AEM as a Cloud Service compatible**.

#### リダイレクトマネージャー

[リダイレクトマネージャー](https://adobe-consulting-services.github.io/acs-aem-commons/features/redirect-manager/index.html) を使用すると、AEMのユーザーは、AEMからのリダイレクトを簡単に管理および公開できます。 この実装は Java™サーブレットフィルターに基づいているので、一般的な JVM リソースの使用になります。 また、この機能により、AEM開発チームやAEMのデプロイメントに依存する必要がなくなります。 リダイレクトマネージャーは両方とも **AEMas a Cloud Service** および **AEM 6.x** 互換性あり 最初のリダイレクトリクエストでは AEM パブリッシュサービスで301/302（ほとんど）の CDN キャッシュを生成する必要がありますが、デフォルトでは301/302、以降のリクエストをエッジ/CDN でリダイレクトできます。


## 実装に最適なソリューション

次に、適切なソリューションを決定するためのいくつかの条件を示します。 また、組織の IT およびマーケティングプロセスが、適切なソリューションの選択に役立ちます。

1. マーケティングチームまたはスーパーユーザーが、AEM開発チームやAEMのリリース、デプロイメントサイクルなしにリダイレクトルールを管理できるようにする。
1. 変更またはリスク軽減を検証、追跡、および元に戻すプロセス。
1. の可用性 _主題に関する専門知識_ 対象 **CDN サービスを介したエッジでの配信** 解決策

