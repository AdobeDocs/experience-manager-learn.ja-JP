---
title: アップグレードの理由の理解
description: 最新版のAdobe Experience Managerへのアップグレードを検討しているお客様向けの主な機能の概要を説明します。
version: 6.5
sub-product: アセット， cloud manager，コマース，コンテンツサービス，ダイナミックメディア，フォーム，基盤，画面，サイト
topics: best-practices, upgrade
audience: all
activity: understand
doc-type: article
translation-type: tm+mt
source-git-commit: 1519856731758ece2860615c06fc0d64edb104a5
workflow-type: tm+mt
source-wordcount: '3540'
ht-degree: 5%

---


# アップグレード理由

最新版のAdobe Experience Managerへのアップグレードを検討しているお客様向けの主な機能の概要を説明します。

## AEM 6.5へのアップグレードの主な機能

+ [Adobe Experience Manager6.5リリースノート](https://helpx.adobe.com/jp/experience-manager/6-5/release-notes.html)

### 基盤の強化

Adobe Experience Manager6.5は、次の機能を通じて、システムの安定性、パフォーマンス、サポート性を引き続き強化します。

+ **Java 11の** サポート（Java 8のサポートを維持しながら）。

### Webサイトの作成と管理

AEM Sitesは、ウェブサイトの作成と構築を高速化するために設計された多数の機能を紹介しています。

+ **SPA** Editorsupportを使用すると、SPA（シングルページアプリケーション）をAEMで完全にオーサリングでき、マーケティング担当者にとって使いやすい豊富なオーサリングエクスペリエンスをサポートできます。+_ **JavaScript SDKの**, SPAプロジェクト開始キットとサポートするビルドツール。フロントエンド開発者はAEMとは別にSPAエディタ互換のシングルページアプリケーションを開発できます。
+ **コア** コンポーネントは、新しいコンポーネント、 **コンポーネント** ライブラリ、および既存のコアコンポーネントに対する様々な拡張を追加します。
+ さらに&#x200B;**翻訳**&#x200B;が強化され、AEM Sitesの翻訳が合理化されました。

### 流体エクスペリエンス

AEMは、AEM以外でのコンテンツの使用を容易にする新しい改善されたツールを使用して、流体エクスペリエンスを引き続き受け入れます。

+ **コンテンツ** フラグメントは、バージョンの比較/相違と注釈をサポートします。
+ **AEM Assets HTTP** APIは、DAM内での **コンテンツ** フラグメントの **JSONとしての直接公開をサポートします**。
   **エクスペリエンス** フラグメントは、 **ページを参照するための** 全文検索とAEMディスパッチャーキャッシュ **の無効化をサポートし** ます ****。

### アセット管理

AEM Assetsは、DAMの使用、管理、理解を向上させるため、豊富な資産管理機能を基盤として構築を続けています。 AEM 6.5では、Adobe Creative Cloudとクリエイティブワークフローの統合が引き続き改善されます。

+ **Adobeアセット** リンクは、クリエイティブをAdobe Creative CloudツールからAEM Assetsに直接接続します。
+ **Adobe** 在庫の統合により、AEM Assetsの経験からAdobe Stockの画像に直接アクセスでき、シームレスなコンテンツ検出を実現します。
+ **AEM Desktop** Apprelasesバージョン2.0をリリースし、パフォーマンスと安定性を向上させつつ自身を再エンビジョンします。
+ **接続** アセットは、別のAEM Assetsインスタンスのアセットにシームレスにアクセスし、使用するために、個別のAEM Sitesインスタンスをサポートします。
+ **Dynamic Media**&#x200B;のビデオサポートを更新しました。**360ビデオ**&#x200B;と&#x200B;**カスタムビデオサムネール**&#x200B;が含まれます。

### コンテンツ情報

AEMは、機械学習と人工知能を活用して、スマートテクノロジーとの統合を継続的に構築し、すべてのエクスペリエンスを向上させます。

+ **Adobeアセット** リンク： **視覚的類似性検索を追加し**、 **** Adobe Creative Cloudツール内で類似の画像を簡単に見つけて使用できます。

### 統合

AEMは、他のAdobeサービスとの統合機能を拡張します。

+ **Experience** Fragmentsは、JSONとしての **書き出しをAdobe Targetにサポートし、Experience DeleteベースのフラグメントをAdobe Targetから** 削除する機能をサポートすることで、 **Adobeターゲットとの統合を**  ****  ****&#x200B;強化します。

### AMS Cloud Manager

[Adobe Managed Services](https://adobe.ly/2HODmsv)(AMS)のお客様専用のCloud Managerでは、次の機能がオファーに提供されます。

+ Cloud Managerは、AEM Sitesから&#x200B;**AEM Assets**&#x200B;までAEMの展開サポートを拡張します。これには、**アセット処理**&#x200B;の自動パフォーマンステストも含まれます。
+ **事前に定義されたしきい値でAEM Publish層を自動** 拡張することで、エンドユーザーの操作性を最適化できます。
+ **非実稼働** パイプラインでは、開発チームはCloud Managerを利用して、コードの品質を継続的に確認し、環境（開発とQA）の削減に導入できます。
+ **CI/CDパイプライン** APICloud Managerとプログラム的に連携し、オンプレミスの開発インフラストラクチャとの統合の可能性を深めます。

## 基礎機能

以下に、AEMが提供する主な基盤機能の一覧を示します。 これらの機能の一部は、以前のバージョンで導入され、各リリースで追加された増分機能です。

+ [AEM Foundationリリースノート](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***このバージョンの✔機能に対する重要な機能強化です。<sup></sup>***

***✔SPは、Service Packまたは機能パックを介して使用可能な機能であることを示します。<sup></sup>***

<table>
    <thead>
        <tr>
            <td>基礎機能</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <strong>Java 11のサポート：</strong> AEMは、Java 11（およびJava 8）をサポートします。
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Oak Content Repository</a>：前任のJackrabbit 2よりもはるかに高いパフォーマンスと拡張性を</strong> 提供します。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">oak-run.jarインデックスのサポート</a>:Oakインデックスの再インデックス/インデックス作成、統計情報収集、整合性チェックを</strong> 改善。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html" target="_blank">カスタム検索インデックス</a>: </strong>
                カスタムインデックス定義を追加して、クエリのパフォーマンスと検索の関連性を最適化する機能。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">オンラインリビジョンのクリーンアップ</a>：サーバーのダウンタイムを伴わずにリポジトリの保守を</strong>
                実行します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">TarMKまたはMongoMKリポジトリストレージ</a>:TarMK（TarPMの次世代版）のシンプルでパフォーマンスの高いファイルベースのストレージを使用する</strong>
                <br> 
                <br> か、MongoMKを使用してMongoDBリポジトリを使用して水平方向に拡大・縮小するかを選択できます。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-with-mongodb.html" target="_blank">MongoMKのパフォーマンスと安定性</a>:MongoMKはAEM 6.0での導入以降、</strong>
            引き続き機能強化が行われています。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore">AmazonS3 DataStore</a>：拡張可能なクラウドストレージソリューションを</strong>
            利用して、バイナリアセットを保存します。</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>タッチ操作対応UI機能のパリティ：従来のUIと同等の生産性と機能を向上させ、オーサリングUIの</strong>
                継続的な機能強化。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Omnisearch:AEMの検索とナビゲ</strong>
                ーションが簡単です。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">操作のダッシュボード</a>：保守の</strong>
 実行、サーバーの正常性の監視、AEM内でのパフォーマンスの分析を行います。</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">アップグレードの改善</a>:</strong>
            アップグレードの改善により、AEMのインプレースアップグレードを簡単かつ迅速に行うことができます。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/htl/using/overview.html" target="_blank">HTLテンプレート言語</a>：プレゼンテーション</strong>
            とロジックを分離する最新のテンプレート化エンジン。コンポーネントの開発時間を大幅に短縮できます。 各リリースで追加される増分機能</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">Sling Models</a>:JCRリソース</strong>
            をビジネスオブジェクトやロジックにモデリングするための柔軟なフレームワーク。各リリースで追加される増分機能
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://adobe.ly/2HODmsv" target="_blank">Cloud Manager</a>: </strong>
                Adobe Managed Services(AMS)のお客様専用であるCloud Managerは、最新のCI/CDパイプラインを通じて開発と展開を加速します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## セキュリティ機能

以下に、AEMが提供する主なセキュリティ機能の一覧を示します。 これらの機能の一部は、以前のバージョンで導入され、各リリースで追加された増分機能です。

+ [セキュリティリリースノート](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***は、✔このバージョンで機能が大幅に強化されたことを示しています。***

***Service PackまたはFeature Packを使用して使用可能な機能を✔<sup>+</sup> で示します。***

<table>
    <thead>
        <tr>
            <td>セキュリティ機能</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">サービス</a></strong>
            <br> ユーザー権限を区分化し、管理者権限を不要に使用しないようにします。</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">キーストアの</a></strong>
            <br> 管理グローバル信頼ストア、証明書、およびキーはすべてリポジトリ内で管理されます。</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong>CSRFrotectionクロスサイト要求偽造</strong> <strong></strong></a>
            <br> 保護をすぐに使用できます。</td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong>CORSupport接触チャネル間のリソース共有のサポートにより、アプリケーションの柔軟性を向上。</strong> <strong></strong></a>
            <br> </td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://docs.adobe.com/docs/en/aem/6-5/administer/security/saml-2-0-authenticationhandler.html" target="_blank">SAML認証の</a><br>
 </strong>サポートの強化SAMLリダイレクト、最適化されたグループ情報、およびキー暗号化の問題が解決されました。 
            <br>
        </td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">OSGi</a><br>
 </strong>設定としてのLDAP LDAP認証の管理と更新を簡素化します。</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong>プレーンテキストの<br>
 </strong>パスワードに対するOSGi暗号化のサポートパスワードやその他の機密値は、暗号化された形式で保存し、自動的に復号化できます。</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">CUGの</a><br>
 </strong>強化パフォーマンスと拡張性の問題に対処するため、クローズドユーザーグループの実装が書き直されました。</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔<sup>+</sup></td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">SSL </a></strong>
            <br> WizardUIを使用して、SSLのセットアップと管理を簡単に行うことができます。</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">カプセル化トークンの</a></strong>
            <br> サポートパブリッシュインスタンス間の水平認証をサポートする「スティッキー」セッションは不要になりました。</td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">AdobeIMS認証</a><br>
 </strong>サポートAdobe Managed Services(AMS)専用。AdobeIMS(Identity Managementシステム)を使用して、AEM Authorインスタンスへのアクセスを一元的に管理します。</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
    </tr>
</tbody>
</table>

## Sites の機能

以下に、AEMが提供する主要なサイト機能の一覧を示します。 これらの機能の一部は、以前のバージョンで導入され、各リリースで追加された増分機能です。

+ [AEM Sitesリリースノート](https://helpx.adobe.com/experience-manager/6-5/release-notes/sites.html)

***このバージョンの✔機能に対する重要な機能強化です。<sup></sup>***

***✔SPは、Service Packまたは機能パックを介して使用可能な機能であることを示します。<sup></sup>***

<table>
    <thead>
        <tr>
            <td><strong>サイト機能</strong></td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank">タッチ操作向けに最適化されたページオーサリング</a>：タッチスクリーンを使用したタブレットやコンピューターを</strong>
            利用できます。</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">レスポンシブサイトオーサリング</a>：レ</strong>
                イアウトモードを使用すると、レスポンシブサイトのデバイスの幅に基づいて、コンポーネントのサイズを変更できます。</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/template-editor-feature-video-use.html" target="_blank">編集可能なテンプレート</a></strong>
            ：特殊な作成者がページテンプレートを作成および編集できるようにします。</td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/core-components/user-guide.html" target="_blank">コアコンポーネント</a>：サイト開発</strong>
            の迅速化GitHubで、頻繁なリリーススケジュールと柔軟性を提供。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">SPAエディタ</a>:ReactまたはAngularに基づいて構築されたシングルページアプリケーション(SPA)フレームワークを使用して、承認可能なWebエクスペリエンスを</strong>
            作成し、有効にします。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/release-notes/style-system-fp.html" target="_blank">スタイルシステム</a></strong>
            ：コンテキスト内スタイルシステムを使用して外観を定義することで、AEMコンポーネントの再利用を促進します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">マルチサイトマネージャー(MSM)</a>：共通のコンテンツを共有する複数のWebサイト（多言語、複数のブランド）を</strong>
            管理します。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">コンテンツの翻訳</a>:</strong>
            プラグアンドプレイのフレームワークは、業界をリードするサードパーティの翻訳サービスと統合されています。</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html" target="_blank">ContextHub</a>:</strong>
            コンテンツのパーソナライズのための次世代クライアントコンテキストフレームワーク。</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/launches.html" target="_blank">起動回数</a>:</strong>
            日々のオーサリングを中断することなく、将来のリリース向けにコンテンツを開発します。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-understand.html" target="_blank">コンテンツフラグメント</a>：再利用しやすいように、プレゼンテーションから編集コンテンツを</strong>
            作成およびキュレーションします。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html" target="_blank">エクスペリエンスフラグメント</a>：デスクトップ、モバイルおよびソーシャルチャネルに最適化された、再利用可能なエクスペリエンスおよびバリエーションを</strong>
            作成します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/release-notes/content-services-fragments-featurepack.html" target="_blank">Content Services</a>：デバイスやアプリケーション間での使用に備えて、AEMからJSONとしてコンテンツを</strong>
            書き出します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Adobe Analytics統合とコンテンツのインサイト：Adobe AnalyticsとDTMの</strong>
                容易な統合。作成者環境内にパフォーマンス情報を表示します。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/sites/authoring/using/content-targeting-touch.html" target="_blank">Adobe Target統合</a></strong>
            ：ターゲット設定されたエクスペリエンスを作成し、再利用可能なオファーライブラリを作成するための順を追ったウィザード。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank">Adobe Campaign統合</a>：次世代の電子メールキャンペーンソリューションとの</strong>
            統合を容易にします。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/using/aem_launch_adobeio_integration.html" target="_blank">Adobe起動の統合</a>:Adobeの次世代tag managementクラウドサービスとの</strong>
            統合。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-screens-introduction.html" target="_blank">画面</a>：デジタル署名およびキオスクのエクスペリエンスを</strong>
            管理します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ecommerce.html" target="_blank">eコマース</a>:Web、モバイルおよびソーシャルのタッチポイントにわたって、ブランドのパーソナライズされたショッピング体験を</strong>
            提供します。
            </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html" target="_blank">コミュニティ</a>:</strong>
            フォーラム、スレッドコメント、イベントカレンダー、その他多くの機能により、サイト訪問者との深い関与が可能です。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## アセットの機能

以下に、AEMが提供する主なアセット機能の一覧を示します。 これらの機能の一部は、以前のバージョンで導入され、各リリースで追加された増分機能です。

+ [AEM Assetsリリースノート](https://helpx.adobe.com/experience-manager/6-5/release-notes/assets.html)

***は、✔このバージョンで機能が大幅に強化されたことを示しています。***

***Service PackまたはFeature Packを使用して使用可能な機能を✔<sup>+</sup> で示します。***

<table>
    <thead>
        <tr>
            <td>アセット機能</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">タッチ操作向けUI</a>：デスクトップコンピューターまたはタッチ対応デバイスでアセットを</strong>
            管理します。</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/metadata.html" target="_blank">高度なメタデータ管理</a>:</strong>
            メタデータテンプレート、メタデータスキーマエディター、およびバルクメタデータの編集。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank">Taskand </a> WorkflowManagement:AEMプロジェクトを利用したデジタルアセットのレビューと承認を行うための、 <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects-with-workflows.html" target="_blank"></a> </strong>
            事前に作成されたワークフローとタスク。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>拡張性とパフォーマンス：</strong>
            拡張性の高い取り込み、アップロード、ストレージのサポート。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">アセットHTTP API</a>：プログラム</strong>
            によって、HTTPとJSONを介してアセットとやり取りします。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/link-sharing.html" target="_blank">リンク共有</a></strong>
            ：ログインを必要とせずに、デジタルアセットを簡単にアドホックに共有できます。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/brand-portal/using/brand-portal.html" target="_blank">Brand Portal</a>:</strong>
            CloudサービスSAASソリューション。デジタルアセットのシームレスな共有と配信を実現します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/use-assets-across-connected-assets-instances.html" target="_blank">接続されたアセット</a>:</strong>
            AEM Sitesインスタンスは、別のAEM Assetsインスタンスのアセットにシームレスにアクセスし、使用できます。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">アセットインサイト</a>:Adobe Analyticsを</strong>
            活用して、AEMのデジタルアセットや表示の顧客とのやり取りを捕捉します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/multilingual-assets.html" target="_blank">多言語アセット</a>：アセットメタデータの</strong>
            翻訳を言語ルートで自動的にサポートします。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/enhanced-smart-tags.html" target="_blank">スマートタグとモデレート</a>:</strong>
            Adobe Senseiを利用して、便利なメタデータを含む画像に自動的にタグ付けします。</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">スマート翻訳検索</a>:AEM Assets検索時に検索用語を</strong>
            自動的に翻訳します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/indesign.html" target="_blank">Adobe InDesign Serverとの統合</a>：製品カタログを</strong>
            生成します。InDesignテンプレートに基づいて、パンフレット、チラシ、印刷用広告を作成します。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/desktop-app/aem-desktop-app.html" target="_blank">AEMデスクトップアプリ</a>:Creative Suite製品で編集するために、アセットをローカルデスクトップに</strong>
            同期します。
            </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank">Adobeイメージングライブラリ</a>:</strong>
                <br> PhotoshopおよびAcrobatのPDFライブラリ。高品質のファイル操作に使用します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/jp/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">Adobeアセットリンク</a>:AdobeCreate Cloudアプリケーションから直接AEM Assetsに</strong>
            アクセスできます。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank">Adobe Stockとの統合</a>:Adobe Stockの画像に</strong>
            直接アクセスし、使用します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

### AEM AssetsDynamic Media

***このバージョンの✔機能に対する重要な機能強化です。<sup></sup>***

***✔SPは、Service Packまたは機能パックを介して使用可能な機能であることを示します。<sup></sup>***


<table>
    <thead>
        <tr>
            <td>Dynamic Media機能</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3 +FP</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets.html" target="_blank">画像処理</a>：スマート切り抜きなど、様々なサイズや形式で画像を</strong>
            動的に配信します。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/assets/using/video-profiles.html" target="_blank">ビデオ</a>:</strong>
            高度なビデオエンコーディングとアダプティブビデオストリーミング</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/interactive-images.html" target="_blank">インタラクティブメディア</a>：インタラクティブバナーやクリック可能コンテンツを含むビデオを</strong>
            作成して、主要オファーを紹介します。
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>セット(<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">画像</a>、 <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">スピン</a>、 <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank">混在メディア</a></strong>
            ):360°の画像表示をユーザーがズーム、パン、回転、シミュレーションできます。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://docs.adobe.com/docs/en/aem/6-5/administer/content/dynamic-media/viewer-presets.html" target="_blank">ビューア</a></strong>
            ：様々な画面/デバイスをサポートするカスタムブランドのリッチメディアプレイヤーおよびプリセット。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/delivering-dynamic-media-assets.html" target="_blank">配信</a></strong>
            :Dynamic Mediaコンテンツのリンクまたは埋め込みと、HTTP/2プロトコルを介した配信の柔軟なオプション。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Scene7からDynamic Mediaへのアップグレード：マスターアセットを移行し、既存のS7 URLを引き続き使用する</strong>
            機能。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

## Formsの特徴

以下はAEMが提供する主なAEM Formsオン追加機能の一覧です。 これらの機能の一部は、以前のバージョンで導入され、各リリースで追加された増分機能です。

+ [AEM Formsリリースノート](https://helpx.adobe.com/experience-manager/6-5/release-notes/forms.html)

***このバージョンの✔機能に対する重要な機能強化です。<sup></sup>***

***✔SPは、Service Packまたは機能パックを介して使用可能な機能であることを示します。<sup></sup>***

<table>
    <thead>
        <tr>
            <td>Forms機能</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">アダプティブFormsエディタ</a>：デバイスとブラウザの設定に基づいて、魅力的でレスポンシブなアダプティブフォームを</strong>
            作成します。</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">レコードのドキュメント</a>:ドキュメントを</strong>
            作成して、データ取得の経験や印刷用のストレージを長期にわたって保証します。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/forms/using/themes.html" target="_blank">テーマエディタ</a>：フォームのコンポーネントやパネルのスタイルを設定するための、再利用可能なテーマを</strong>
            作成します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/forms/using/template-editor.html" target="_blank">テンプレートエディター</a>：アダプティブフォームの</strong>
            標準化とベストプラクティスの実装。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Adobe Sign統合</a>:Adobe Sign統合フォームのデプロイを</strong>
            許可します。署名シナリオを基にした署名シナリオを使用します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/cm-overview.html" target="_blank">Correspondence Management</a>:AEM Forms</strong>
            では、パーソナライズされたインタラクティブな顧客対応を作成、管理および配信できます。
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">サードパーティデータ統合</a>：データ統合を</strong>
            使用すると、フォームのユーザー入力に基づいて、異なるデータソースからデータが取得されます。この場合、フォームを送信すると、取得したデータが取得元のデータソースに再度書き込まれます。
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">Forms処理のワークフロー（OSGi上）</a>：フォーム承認プロセスの</strong>
            シンプルなデプロイメント。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/user-guide.html?topic=/experience-manager/6-5/forms/morehelp/integrations.ug.js" target="_blank">Marketing Cloudとの統合</a>:Adobe AnalyticsとAdobe Targetの</strong>
            統合により、顧客体験を強化し測定します。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank">Form Manager</a></strong>
            ：分析、翻訳、A/Bテスト、レビュー、発行の有効化など、フォーム/ドキュメント/通信をすべて管理する単一の場所。
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/aem-forms-app.html" target="_blank">AEM Formsアプリ</a>:iOS、Android、またはWindows上のアプリ内でオンライン/オフラインのフォーム処理を</strong>
            許可します。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/adaptive-document.html" target="_blank">Interactive Communications</a></strong>
            ：チャート(旧称Adaptiveドキュメント)などのインタラクティブな要素を使用して、ターゲット設定された文など、豊富な通信を作成できます。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/pdf/aem-forms/6-5/WorkbenchHelp.pdf" target="_blank">Forms処理のワークフロー(J2EE)</a>：直観的なIDEを使用して複雑なフォームやドキュメント中心のワークフローを</strong>
            構築します。</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank">AEM Formsドキュメントセキュリティ</a>:PDFおよびOfficeドキュメントへの</strong>
            安全なアクセスと認証。
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">テストフレームワーク</a>:CalvinフレームワークとChromeプラグインを</strong>
            使用して、アダプティブフォームをサポートおよびデバッグします。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

## Communities の機能

以下はAEMが提供する主なAEM Communitiesオン追加機能の一覧です。 これらの機能の一部は、以前のバージョンで導入され、各リリースで追加された増分機能です。

+ [AEM Communitiesの新機能の概要](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html#main-pars_text)

***このバージョンの✔機能に対する重要な機能強化です。<sup></sup>***

***✔SPは、Service Packまたは機能パックを介して使用可能な機能であることを示します。<sup></sup>***

<table>
    <thead>
        <tr>
            <td> </td>
            <td>Communities 機能</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="7">コミュニティ機能</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/forum.html" target="_blank">フォーラム</a>:</strong> (Social Component Framework)新しいトピック、または表示、フォロー、検索、移動の既存のトピックを作成します。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <p><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-qna.html" target="_blank">QnA</a>:</strong>
                Ask、表示、回答の質問</p>
            </td>
            <td></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/blog-feature.html" target="_blank">ブログ</a></strong>
                ：ブログ記事と投稿側でのコメントの作成。
            </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/ideation-feature.html" target="_blank">アイデア</a>：アイデアを</strong>
                作成し、コミュニティや表示と共有し、既存のアイデアに従ってコメントします。
            </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/calendar.html" target="_blank">カレンダー</a>:</strong>
                (Social Component Framework)コミュニティイベント情報をサイト訪問者に提供します。
            </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/file-library.html" target="_blank">ファイルライブラリ</a>：コミュニティサイト内のファイルの</strong>
                アップロード、管理およびダウンロードを行います。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/users.html#AboutCommunityGroups" target="_blank">User Groups</a>:
            </strong>ユーザーのセットは、メンバーグループに属することができ、まとめてロールを割り当てることができます。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong> </strong></td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">割り当て</a>：学習リソースを</strong>
            作成し、コミュニティメンバーに割り当てます。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">Enablement</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/catalog.html" target="_blank"></a> カタログと <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">リソース管理</a>：カタログから有効化リソースに</strong>
            アクセスします。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#CreateaLearningPath" target="_blank">学習パス管理</a>:</strong>
            有効化リソースのコースまたはグループを管理します。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reports.html#main-pars_text_1739724213" target="_blank">有効化レポート</a>：有効化リソースと学習パスの</strong>
            レポート。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#main-pars_text_899882038" target="_blank">有効化に関する取り組み</a>：有効化リソースに関する</strong>
            追加コメント。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">有効化分析</a>:</strong>
            ビデオ分析、進行状況レポート、割り当てレポート</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="8">コモンズ</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/comments.html" target="_blank">コメ</a> ントと添付ファイル：</strong>
            (Social Component Framework)コミュニティのメンバーは、Communitiesサイトで意見やコンテンツに関する知識を共有します。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>コンテンツフラグメントの変換：コンテンツフラグメントに対するUGCの貢献度を</strong>
            変換します。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reviews.html" target="_blank">レビュー</a>:</strong>
                (Social Component Framework)コミュニティのメンバーは、コメントと評価機能の組み合わせを使用してコンテンツの一部をレビューします。</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/rating.html" target="_blank">評価</a>:/strong&gt;(Social Component Framework)コミュニティのメンバーは、コンテンツの一部を評価します。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/voting.html" target="_blank">投票数</a>:</strong>
                (Social Component Framework)コミュニティのメンバーは、コンテンツの一部を投票または否決します。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tag-ugc.html" target="_blank">タグ</a>:</strong>
            タグ（キーワードまたはラベル）にコンテンツを付加して、コンテンツをすばやく見つけることができます。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/search.html" target="_blank">検索</a>:</strong>
            予測検索と暗示的検索。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/translate-ugc.html" target="_blank">翻訳</a>：ユーザ生成コンテンツの</strong>
            機械翻訳。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="10">Administration</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/create-site.html" target="_blank">サイト管理</a>：コミュニティ機能を使用したサイトの</strong>
            作成。</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">テンプレート</a>：完全に機能するコミュニティサイトをウィザードベースで作成するための</strong>
                <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank"></a> サイトおよび <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tools-groups.html" target="_blank"></a> グループテンプレート。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>編集可能なテンプレート：コミュニティ管理者は、AEM編集可能なテンプレートを使用して豊富なエクスペリエンスを構築で</strong>
            きます。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/creating-groups.html" target="_blank">グループまたはサブコミュニティ</a>：コミュニティサイト内にサブコミュニティを</strong>
            動的に作成します。
            </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/in-context.html" target="_blank">モデレート</a>:</strong>
            ユーザー生成コンテンツのモデレート。
            </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderation.html" target="_blank">一括モデレート</a>:</strong>
            モデレートコンソールを使用して、ユーザー生成コンテンツを一括管理します。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderate-ugc.html#CommonModerationConcepts" target="_blank">スパムの検出と不敬のフィルター</a>:</strong>
            自動スパム検出。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/members.html" target="_blank">メンバー管理</a>：メンバー管理領域からユーザープロファイルおよびグループを</strong>
            管理します。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html#main-pars_text_866731966" target="_blank">レスポンシブデザイン</a>:</strong>
            AEM Communitiesサイトがレスポンシブです。
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Analytics</a>:</strong>
            Adobe Analyticsとの統合を参照して、Communitiesのサイトの使用に関する主要なインサイトを確認してください。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="4">メンバー</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/advanced.html" target="_blank">スコアリングとバッジング</a>:</strong>
            (Adobe Senseiが提供する高度なスコアリング)コミュニティメンバーを専門家として特定し、報酬を与えます。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/activities.html" target="_blank">アク</a> ティビティと <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/notifications.html" target="_blank">通知</a>:</strong>
            表示の最近のアクティビティストリームを表示し、関心のあるイベントに関する通知を受け取ります。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/configure-messaging.html" target="_blank">メッセージ</a>：ユーザーおよびグループへの</strong>
            ダイレクトメッセージング。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/communities/using/social-login.html" target="_blank">ソーシャルログイン</a>:</strong>
            FacebookアカウントまたはTwitterアカウントでログインします。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">プラットフォーム</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">MSRP(Mongoストレージ)</a>:</strong>
            ユーザー生成コンテンツ(UGC)は、ローカルのMongoDBインスタンスに直接保持されます</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">DSRP(データベースストレージ)</a>:</strong>
            ユーザー生成コンテンツ(UGC)は、ローカルのMySQLデータベースインスタンスに直接保持されます。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">SRP(クラウドストレージ)</a>:</strong>
                ユーザー生成コンテンツ(UGC)は、Adobeがホストし管理するクラウドサービスでリモートで保持されます。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank"><strong>JSRP</a>:</strong>
                コミュニティのコンテンツはJCRに保存され、UGCには、そのコンテンツが投稿された作成者（または発行）インスタンスからアクセスできます。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sync.html" target="_blank">ユーザーとグループの同期</a>：発行ファームトポロジを使用する場合に、発行インスタンス間でユーザーとグループを</strong>
            同期します。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

AEM Communitiesは、[改良](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html)をリリースし、組織がユーザーを惹きつけ、ユーザーを有効にするための&lt;a0/>改良&lt;a1/>を加えました。

+ **ユーザー生成コンテンツでの@** mentionsupport。
+ **有効化**&#x200B;コンポーネントの&#x200B;**キーボードナビゲーション**&#x200B;を使用してアクセシビリティを改善。
+ **バルクモデレート**&#x200B;が改善され、**カスタムフィルター**&#x200B;を使用できるようになりました。
+ **編集可能な** テンプレートを使用すると、コミュニティ管理者はAEMで豊富なコミュニティエクスペリエンスを構築できます。
+ ユーザーは、**ダイレクトメッセージをバルク**&#x200B;でグループの全メンバーに送信できるようになりました。
