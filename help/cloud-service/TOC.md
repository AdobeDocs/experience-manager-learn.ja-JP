---
user-guide-title: Adobe Experience Manager as a Cloud Service のチュートリアル
user-guide-description: Adobe Experience Manager as a Cloud Service のチュートリアルのコレクションです。
breadcrumb-title: AEM as a Cloud Service チュートリアル
sub-product: Experience Manager as a Cloud Service
version: Cloud Service
team: TM
source-git-commit: 9fd2240cace83c27083f8209e9cec21640718989
workflow-type: tm+mt
source-wordcount: '868'
ht-degree: 27%

---


# Adobe Experience Manager as a Cloud Service のチュートリアル {#cloud-service}

+ [概要](./overview.md)
+ AEM as a Cloud Service の概要{#introduction}
   + [AEMとは](./introduction/what-is-aem-as-a-cloud-service.md)
   + [変化](./introduction/evolution.md)
   + [アーキテクチャ](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
   + 戦略と思考のリーダーシップ{#strategy}
      + [Experience Manager — ガバナンスとスタッフのモデルとアーキタイプ](./introduction/experience-manager-governance-and-staffing-models.md)
      + [Adobe Experience Managerでのコンテンツベロシティの推進方法](./introduction/drive-content-velocity-for-sites.md)
      + [AEMスタイルシステムを使用してコンテンツの速度を向上](./introduction/accelerate-content-velocity-aem.md)
+ [Experience Cloud との統合](./experience-cloud/integrations.md)
+ 基盤となる技術 {#underlying-technology}
   + [AEM アーキテクチャ](./underlying-technology/introduction-architecture.md)
   + [OSGi](./underlying-technology/introduction-osgi.md)
   + [Java コンテンツリポジトリー](./underlying-technology/introduction-jcr.md)
   + [Sling](./underlying-technology/introduction-sling.md)
   + [オーサーとパブリッシュサービス](./underlying-technology/introduction-author-publish.md)
   + [Dispatcher](./underlying-technology/introduction-dispatcher.md)
+ Cloud Manager {#cloud-manager}
   + [プログラム](./cloud-manager/programs.md)
   + [環境](./cloud-manager/environments.md)
   + [CI/CD 実稼動パイプライン](./cloud-manager/cicd-production-pipeline.md)
   + [CI/CD 非実稼動パイプライン](./cloud-manager/cicd-non-production-pipeline.md)
   + [アクティビティ](./cloud-manager/activity.md)
   + 開発業務{#devops}
      + [コードのデプロイ](./cloud-manager/devops/deploy-code.md)
      + [プロジェクトを結合](./cloud-manager/devops/merge-projects.md)
      + [パイプラインの設定](./cloud-manager/devops/configure-pipelines.md)
      + [継続的統合](./cloud-manager/devops/continuous-integration.md)
      + [テスト結果の分析](./cloud-manager/devops/analyze-test-results.md)
      + [Dispatcher の設定](./cloud-manager/devops/dispatcher-configurations.md)
      + [Cloud Manager API](./cloud-manager/devops/cloud-manager-apis.md)
+ ローカル開発環境の設定 {#local-development-environment-set-up}
   + [概要](./local-development-environment/overview.md)
   + [開発ツール](./local-development-environment/development-tools.md)
   + [Local AEM Runtime](./local-development-environment/aem-runtime.md)
   + [ローカル Dispatcher ツール](./local-development-environment/dispatcher-tools.md)
+ 開発{#developing}
   + 拡張性{#extensibility}
      + コンテンツフラグメントコンソール{#content-fragments}
         + [概要](./developing/extensibility/content-fragments/overview.md)
         + [拡張機能の登録](./developing/extensibility/content-fragments/extension-registration.md)
         + [ヘッダーメニュー](./developing/extensibility/content-fragments/header-menu.md)
         + [アクションバー](./developing/extensibility/content-fragments/action-bar.md)
         + [モーダル](./developing/extensibility/content-fragments/modal.md)
         + [Adobe I/O Runtime action](./developing/extensibility/content-fragments/runtime-action.md)
         + [テスト](./developing/extensibility/content-fragments/test.md)
         + [デプロイ](./developing/extensibility/content-fragments/deploy.md)
         + 拡張の例{#example-extensions}
            + [プロパティ一括更新拡張機能](./developing/extensibility/content-fragments/example-extensions/bulk-property-update.md)
   + 開発の基本{#basics}
      + [AEM SDK](./developing/basics/aem-sdk.md)
      + [ローカル開発環境](./developing/basics/local-development-environment.md)
      + [AEM プロジェクトアーキタイプ](./developing/basics/aem-project-archetype.md)
      + [AEM プロジェクトの構造](./developing/basics/project-structure.md)
      + [可変コンテンツと不変コンテンツ](./developing/basics/mutable-immutable.md)
      + [リポジトリー構造パッケージ](./developing/basics/repository-structure-package.md)
      + [コンテンツ公開](./developing/basics/content-publishing.md)
      + [OSGi 設定](./developing/basics/osgi-configurations.md)
      + [Dispatcher 設定の移行](./developing/basics/dispatcher-configuration.md)
   + AEM プロジェクト{#aem-projects}
      + [AEM Maven Project](./developing/projects/maven-project-structure.md)
      + [AEM Maven プロジェクトのクリーニング](./developing/projects/remove-samples.md)
   + OSGi サービス{#osgi-services}
      + [OSGi サービスの基本](./developing/osgi-services/basics.md)
      + [OSGi コンポーネントのライフサイクル](./developing/osgi-services/lifecycle.md)
      + [OSGi 設定の基本](./developing/osgi-services/configurations.md)
      + [OCD を使用した OSGi 設定](./developing/osgi-services/configurations-ocd.md)
   + 詳細{#advanced}
      + [サービスユーザー](./developing/advanced/service-users.md)
      + [カスタム名前空間](./developing/advanced/custom-namespaces.md)
      + [ページバリアントのキャッシュ](./developing/advanced/variant-caching.md)
   + [AEM SDK API JavaDocs](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/index.html)
+ デバッグAEM{#debugging}
   + AEM SDK のデバッグ{#debugging-aem-sdk}
      + [概要](./debugging/aem-sdk-local-quickstart/overview.md)
      + [ログ](./debugging/aem-sdk-local-quickstart/logs.md)
      + [リモートデバッグ](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [OSGi Web コンソール](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Dispatcher ツール](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [その他のツール](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + デバッグAEM as a Cloud Service{#debugging-aem-as-a-cloud-service}
      + [概要](./debugging/cloud-service/overview.md)
      + [ログ](./debugging/cloud-service/logs.md)
      + [ビルドとデプロイ](./debugging/cloud-service/build-and-deployment.md)
      + [デベロッパーコンソール](./debugging/cloud-service/developer-console.md)
      + [リポジトリーブラウザー](./debugging/cloud-service/repository-browser.md)
      + リスク{#risks}
         + [横断の警告](./debugging/cloud-service/risks/traversals.md)
+ コンテンツ配信{#content-delivery}
   + [URL リダイレクト](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/administration/url-redirection.html)
+ AEMへのアクセス{#accessing}
   + [概要](./accessing/overview.md)
   + [Adobe IMSユーザー](./accessing/adobe-ims-users.md)
   + [Adobe IMSのユーザーグループ](./accessing/adobe-ims-user-groups.md)
   + [Adobe IMS製品プロファイル](./accessing/adobe-ims-product-profiles.md)
   + [AEMのユーザー、グループ、権限](./accessing/aem-users-groups-and-permissions.md)
   + [AEMウォークスルーへのアクセスの設定](./accessing/walk-through.md)
+ 認証{#authentication}
   + [概要](./authentication/authentication.md)
   + [SAML 2.0](./authentication/saml-2-0.md)
+ 高度なネットワーク機能{#networking}
   + [概要](./networking/advanced-networking.md)
   + [柔軟なポート出力](./networking/flexible-port-egress.md)
   + [出力専用 IP アドレス](./networking/dedicated-egress-ip-address.md)
   + [仮想プライベートネットワーク](./networking/vpn.md)
   + コードの例{#examples}
      + [非標準ポートでの HTTP/HTTPS（柔軟なポート出力用）](./networking/examples/http-on-non-standard-ports-flexible-port-egress.md)
      + [専用の出力 IP アドレス/VPN 用の HTTP/HTTPS](./networking/examples/http-dedicated-egress-ip-vpn.md)
      + [DataSourcePool を使用した SQL 接続](./networking/examples/sql-datasourcepool.md)
      + [Java SQL API を使用した SQL 接続](./networking/examples/sql-java-apis.md)
      + [電子メールサービス](./networking/examples/email-service.md)
+ 移行 {#migration}
   + [コンテンツ転送ツール](./migration/content-transfer-tool.md)
   + [アセットの一括読み込み](./migration/bulk-import.md)
   + AEM as a Cloud Service への移行 {#moving-to-aem-as-a-cloud-service}
      + [はじめに](./migration/moving-to-aem-as-a-cloud-service/introduction.md)
      + [オンボーディング](./migration/moving-to-aem-as-a-cloud-service/onboarding.md)
      + [Cloud Manager](./migration/moving-to-aem-as-a-cloud-service/cloud-manager.md)
      + [BPA および CAM](./migration/moving-to-aem-as-a-cloud-service/bpa-and-cam.md)
      + [AEM Modernization tools](./migration/moving-to-aem-as-a-cloud-service/aem-modernization-tools.md)
      + [リポジトリの最新化](./migration/moving-to-aem-as-a-cloud-service/repository-modernization.md)
      + [asset computeマイクロサービス](./migration/moving-to-aem-as-a-cloud-service/asset-compute-microservices.md)
      + [Dispatcher](./migration/moving-to-aem-as-a-cloud-service/dispatcher.md)
      + [検索およびインデックス作成](./migration/moving-to-aem-as-a-cloud-service/search-and-indexing.md)
      + コンテンツの移行 {#content-migration}
         + [一括読み込みサービス](./migration/moving-to-aem-as-a-cloud-service/content-migration/bulk-import-service.md)
         + [コンテンツ転送ツール](./migration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.md)
         + [FAQ](./migration/moving-to-aem-as-a-cloud-service/content-migration/faq.md)
      + [トラブルシューティング](./migration/moving-to-aem-as-a-cloud-service/troubleshooting.md)
      + AEM Formsas a Cloud Service {#aem-forms}
         + [はじめに](./migration/moving-to-aem-as-a-cloud-service/aem-forms/introduction.md)
         + [デジタル登録](./migration/moving-to-aem-as-a-cloud-service/aem-forms/digital-enrollment.md)
         + [通信](./migration/moving-to-aem-as-a-cloud-service/aem-forms/communications.md)
   + Cloud Acceleration Manager {#cloud-acceleration-manager}
      + [はじめに](./migration/cloud-acceleration-manager/introduction.md)
      + [準備状況およびベストプラクティスアナライザー](./migration/cloud-acceleration-manager/readiness-and-best-practice-analyzer.md)
      + [実装フェーズ](./migration/cloud-acceleration-manager/implementation-phase.md)
      + [コンテンツ転送ツール](./migration/cloud-acceleration-manager/content-transfer-tool.md)
      + [コードリファクタリングツール](./migration/cloud-acceleration-manager/code-refactoring-tools.md)
      + [Code Repository Modernizer](./migration/cloud-acceleration-manager/code-repository-modernizer.md)
      + [Dispatcher コンバーター](./migration/cloud-acceleration-manager/dispatcher-converter.md)
      + [インデックスコンバーター](./migration/cloud-acceleration-manager/index-converter.md)
      + [アセットワークフローの移行 ツール](./migration/cloud-acceleration-manager/asset-workflow-migration-tool.md)
      + [Cloud Acceleration Manager の操作](./migration/cloud-acceleration-manager/navigating.md)
      + [Cloud Acceleration Manager の使用](./migration/cloud-acceleration-manager/using.md)
+ Forms{#forms}
   + Formsas a Cloud Service向けの開発{#developing-for-cloud-service}
      + [概要](./forms/developing-for-cloud-service/getting-started.md)
      + [IntelliJ のインストール](./forms/developing-for-cloud-service/intellij-set-up.md)
      + [Git を設定](./forms/developing-for-cloud-service/setup-git.md)
      + [IntelliJ をAEMと同期](./forms/developing-for-cloud-service/intellij-and-aem-sync.md)
      + [フォームの作成](./forms/developing-for-cloud-service/deploy-your-first-form.md)
      + [フォームポータルコンポーネントを有効にする](./forms/developing-for-cloud-service/forms-portal-components.md)
      + [Cloud Servicesと FDM を含める](./forms/developing-for-cloud-service/azure-storage-fdm.md)
      + [コンテキスト対応クラウド設定](./forms/developing-for-cloud-service/context-aware-fdm.md)
      + [Cloud Manager にプッシュ](./forms/developing-for-cloud-service/push-project-to-cloud-manager-git.md)
      + [開発環境にデプロイ](./forms/developing-for-cloud-service/deploy-to-dev-environment.md)
      + [Maven アーキタイプの更新](./forms/developing-for-cloud-service/updating-project-archetype.md)
   + アダプティブフォームを作成{#create-first-af}
      + [はじめに](./forms/create-first-af/introduction.md)
      + [テーマを作成](./forms/create-first-af/create-theme.md)
      + [テンプレートを作成](./forms/create-first-af/create-template.md)
      + [フラグメントを作成](./forms/create-first-af/create-fragments.md)
      + [フォームを作成](./forms/create-first-af/create-af.md)
      + [ルートパネルを設定](./forms/create-first-af/configure-root-panel.md)
      + [ユーザーパネルを設定](./forms/create-first-af/configure-people-panel.md)
      + [所得パネルを設定](./forms/create-first-af/configure-income-panel.md)
      + [アセットパネルの設定](./forms/create-first-af/configure-assets-panel.md)
      + [スタートパネルを設定](./forms/create-first-af/configure-start-panel.md)
      + [ツールバーを追加して設定](./forms/create-first-af/add-configure-toolbar.md)
   + AEM Forms CS でのドキュメントの生成{#doc-gen-formscs}
      + [はじめに](./forms/doc-gen-forms-cs/introduction.md)
      + [サービス資格情報の作成](./forms/doc-gen-forms-cs/service-credentials.md)
      + [JWT トークンを作成](./forms/doc-gen-forms-cs/create-jwt.md)
      + [アクセストークンを作成](./forms/doc-gen-forms-cs/create-access-token.md)
      + [データをテンプレートと結合](./forms/doc-gen-forms-cs/merge-data-with-template.md)
      + [ソリューションをテストする](./forms/doc-gen-forms-cs/test.md)
      + [課題](./forms/doc-gen-forms-cs/challenge.md)
   + バッチ API を使用したドキュメント生成{#formscs-batch-api}
      + [はじめに](./forms/formscs-batch-api/introduction.md)
      + [Azure ストレージの構成](./forms/formscs-batch-api/configure-azure-storage.md)
      + [USC バッチ構成の作成](./forms/formscs-batch-api/configure-usc-batch.md)
      + [バッチ設定を作成](./forms/formscs-batch-api/create-batch-config.md)
      + [バッチを実行](./forms/formscs-batch-api/execute-batch-generate-documents.md)
   + Forms CS でのPDF操作{#forms-cs-assembler}
      + [はじめに](./forms/forms-cs-assembler/introduction.md)
      + [サービス資格情報の作成](./forms/forms-cs-assembler/service-credentials.md)
      + [JWT トークンを作成](./forms/forms-cs-assembler/create-jwt.md)
      + [アクセストークンを作成](./forms/forms-cs-assembler/create-access-token.md)
      + [アセンブリPDFファイル](./forms/forms-cs-assembler/assemble-pdf-files.md)
      + [PDF/A ユーティリティ](./forms/forms-cs-assembler/pdfa-utilities.md)
      + [ソリューションをテストする](./forms/forms-cs-assembler/test.md)
      + [課題](./forms/forms-cs-assembler/challenge.md)
   + Azure Portal ストレージ{#forms-cs-azure-portal}
      + [はじめに](./forms/forms-cs-azure-portal/introduction.md)
      + [フォームデータモデルの作成](./forms/forms-cs-azure-portal/create-fdm.md)
      + [Azure ストレージにフォームデータを保存](./forms/forms-cs-azure-portal/create-af.md)
      + [事前入力フォーム](./forms/forms-cs-azure-portal/prefill-af-storage.md)
      + [クエリの送信](./forms/forms-cs-azure-portal/query-submitted-data.md)
   + レビューワークフローを作成{#create-aem-workflow}
      + [ワークフローストレージの外部化](./forms/create-aem-workflow/externalize-workflow.md)
      + [ワークフローモデルを作成](./forms/create-aem-workflow/create-workflow.md)
      + [トリガーワークフロー](./forms/create-aem-workflow/configure-af.md)
   + Acrobat SignとAEM Forms{#forms-and-sign}
      + [はじめに](./forms/forms-and-sign/introduction.md)
      + [Acrobat Sign API アプリケーション](./forms/forms-and-sign/create-sign-api-application.md)
      + [Acrobat Sign Cloud Configuration](./forms/forms-and-sign/create-adobe-sign-cloud-configuration.md)
      + [アダプティブフォームを作成](./forms/forms-and-sign/create-adaptive-form.md)
      + [入力および署名用に設定](./forms/forms-and-sign/configure-form-fill-and-sign.md)
   + Microsoft Power Automate との統合{#forms-cs-and-power-automate}
      + [統合の設定](./forms/forms-cs-and-power-automate/integrate-formscs-power-automate.md)
      + [送信されたフォームデータを解析](./forms/forms-cs-and-power-automate/send-email-notification.md)
      + [DoR を電子メールの添付ファイルとして送信](./forms/forms-cs-and-power-automate/send-dor-email-attachment.md)
      + [送信されたデータからフォームの添付ファイルを抽出する](./forms/forms-cs-and-power-automate/send-af-attachments-in-email.md)
   + Microsoft Dynamics との統合{#formscs-dynamics-crm}
      + [Dynamics アプリケーションを作成](./forms/formscs-dynamics-crm/create-dynamics-account.md)
      + [データソースの設定](./forms/formscs-dynamics-crm/configure-odata-data-source.md)
      + [フォームデータモデルの作成](./forms/formscs-dynamics-crm/create-form-data-model.md)
      + [アダプティブフォームを作成](./forms/formscs-dynamics-crm/create-adaptive-form.md)
   + Salesforce との統合{#integrate-with-salesforce}
      + [はじめに](./forms/integrate-with-salesforce/introduction.md)
      + [接続されたアプリを作成](./forms/integrate-with-salesforce/create-connected-app.md)
      + [Swagger ファイルを作成](./forms/integrate-with-salesforce/describe-rest-api.md)
      + [データソースを作成](./forms/integrate-with-salesforce/create-data-source.md)
      + [フォームデータモデルの作成](./forms/integrate-with-salesforce/create-form-data-model.md)
      + [テストフォームの送信](./forms/integrate-with-salesforce/create-lead-submitting-form.md)
      + [クリックイベントをテスト](./forms/integrate-with-salesforce/create-lead-click-event.md)
+ asset compute拡張機能{#asset-compute}
   + [概要](./asset-compute/overview.md)
   + 設定{#set-up}
      + [アカウントとサービスのプロビジョニング](./asset-compute/set-up/accounts-and-services.md)
      + [ローカル開発環境](./asset-compute/set-up/development-environment.md)
      + [App Builder](./asset-compute/set-up/app-builder.md)
   + 開発{#develop}
      + [asset computeプロジェクト](./asset-compute/develop/project.md)
      + [環境変数の設定](./asset-compute/develop/environment-variables.md)
      + [manifest.yml の設定](./asset-compute/develop/manifest.md)
      + [作業者の開発](./asset-compute/develop/worker.md)
      + [開発ツールの使用](./asset-compute/develop/development-tool.md)
   + テストとデバッグ{#test-debug}
      + [ワーカーのテスト](./asset-compute/test-debug/test.md)
      + [ワーカーのデバッグ](./asset-compute/test-debug/debug.md)
   + {#deploy} のデプロイ
      + [Adobe I/O Runtimeにデプロイ](./asset-compute/deploy/runtime.md)
      + [ の AEM  との統合](./asset-compute/deploy/processing-profiles.md)
   + 詳細{#advanced}
      + [メタデータワーカー](./asset-compute/advanced/metadata.md)
   + [トラブルシューティング](./asset-compute/troubleshooting.md)
+ クラウド 5{#cloud-5}
   + [はじめに](./cloud-5/cloud5-introduction.md)
   + [シーズン 1](./cloud-5/cloud5-season-1.md)
   + [シーズン 2](./cloud-5/cloud5-season-2.md)
   + [AEM CDN Part 1](./cloud-5/cloud5-aem-cdn-part1.md)
   + [AEM CDN パート 2](./cloud-5/cloud5-aem-cdn-part2.md)
   + [AEM Log Files](./cloud-5/cloud5-aem-log-files.md)
   + [ログイントークン](./cloud-5/cloud5-getting-login-token-integrations.md)
   + [Cloud Dispatcher](./cloud-5/cloud5-aem-dispatcher-cloud.md)
   + [移行 1](./cloud-5/cloud5-aem-content-migration-part-1.md)
   + [移行 2](./cloud-5/cloud5-aem-content-migration-part-2.md)
   + [Dispatcher バリデーター](./cloud-5/cloud5-aem-dispatcher-validator.md)
   + [検索およびインデックス作成](./cloud-5/cloud5-aem-search-and-indexing.md)
   + [Adobeアプリビルダー](./cloud-5/cloud5-adobe-app-builder.md)
   + シーズン 2{#season-2}
      + [フラグメント](./cloud-5/season-2/cloud5-experience-v-content-fragments.md)
      + [Repo Modernizer](./cloud-5/season-2/cloud5-repo-modernizer.md)
      + [Admin Console](./cloud-5/season-2/cloud5-admin-console.md)
      + [REPOINIT](./cloud-5/season-2/cloud5-repoinit.md)
      + [Sling Job Scheduler](./cloud-5/season-2/cloud5-sling-job-scheduler.md)
      + [キャッシュの修正](./cloud-5/season-2/cloud5-fix-your-cache.md)
      + [書き換えの修正](./cloud-5/season-2/cloud5-fix-your-rewrites.md)
      + [Cloud Manager - Experience Audit](./cloud-5/season-2/cloud5-mocm-experience-audit.md)
      + [Cloud Manager — 単体テスト](./cloud-5/season-2/cloud5-mocm-unit-tests.md)
      + [Cloud Manager — 機能テスト](./cloud-5/season-2/cloud5-mocm-functional-tests.md)
+ [AEM Experts Series](./aem-experts-series.md)
+ 複数手順のTutorials{#multi-step-tutorials}
   + [AEM Sites開発](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=ja)
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=ja)
   + [SPAエディター (React)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html?lang=ja)
   + [AEM SitesとAdobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html)
   + [トークンベースの認証](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)
