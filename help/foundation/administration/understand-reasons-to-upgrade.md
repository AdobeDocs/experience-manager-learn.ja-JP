---
title: アップグレード理由の理解
description: お客様が最新バージョンのAdobe Experience Managerへのアップグレードを検討する際の主な機能の大まかな分類。
version: 6.5
sub-product: アセット， cloud-manager，コマース，コンテンツサービス， dynamic-media，フォーム，基盤，スクリーン，サイト
topics: best-practices, upgrade
audience: all
activity: understand
doc-type: article
topic: アップグレード
role: Leader, Architect, Developer, Administrator, Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '3541'
ht-degree: 6%

---


# アップグレード理由の理解

お客様が最新バージョンのAdobe Experience Managerへのアップグレードを検討する際の主な機能の大まかな分類。

## AEM 6.5へのアップグレードの主な機能

+ [Adobe Experience Manager 6.5リリースノート](https://helpx.adobe.com/jp/experience-manager/6-5/release-notes.html)

### Foundationの強化

Adobe Experience Manager 6.5は、以下を通じて、システムの安定性、パフォーマンス、サポート性を引き続き向上させます。

+ **Java 11** のサポート（Java 8のサポートを維持しながら）。

### Webサイトの作成と管理

AEM Sitesでは、Webサイトの作成と構築を高速化するために設計された、次のような多くの機能が導入されています。

+ **SPA** Editorsupportを使用すると、SPA（シングルページアプリケーション）をAEMで完全にオーサリングし、マーケティング担当者に適した豊富なオーサリングエクスペリエンスをサポートできます。+_ **JavaScript SDKの**、SPA Project Start Kitおよびサポートするビルドツールを使用して、フロントエンド開発者は、AEMとは無関係に、SPA Editor互換の単一ページアプリケーションを開発できます。
+ **コアコン** ポーネントは、多数の新しいコンポーネン **ト、コンポーネ** ントライブラリのほか、既存のコアコンポーネントに対する様々な機能強化を追加します。
+ さらに&#x200B;**翻訳**&#x200B;機能が強化され、AEM Sitesの翻訳が効率化されます。

### 流動的なエクスペリエンス

AEMは、AEM以外でのコンテンツの使用を容易にする新しい改善されたツールを使用して、流動的なエクスペリエンスを引き続き採用しています。

+ **コンテンツフ** ラグメントは、バージョンの比較/差分と注釈をサポートしています。
+ **AEM Assets HTTP APIでは、JSONと** してDAMへ **のコンテ** ンツフラグメントの直接公開がサ **ポートされています**。
   **エクスペリエ** ンスフラグメ **ントは、参照** ページに対する全文検索と **AEM Dispatcherキャッシュの無効** 化をサポート ****&#x200B;します。

### アセット管理

AEM Assetsは、DAMの使用、管理、理解を向上させるために、豊富なアセット管理機能のセットを基盤として、引き続き構築しています。 AEM 6.5では、Adobe Creative Cloudとクリエイティブワークフローの統合が引き続き改善されます。

+ **Adobeアセッ** トリンクは、クリエイティブをAdobe Creative CloudツールからAEM Assetsに直接接続します。
+ **Adobe** Stock統合を使用すると、AEM AssetsのエクスペリエンスからAdobe Stockの画像に直接アクセスし、シームレスなコンテンツ検出エクスペリエンスを実現できます。
+ **AEM Desktop Appreasesバ** ージョン2.0をリリースし、パフォーマンスと安定性を向上させつつ、自らを再エンビジョンしています。
+ **Connected Assetsは、別の** AEM Sitesインスタンスのアセットにシームレスにアクセスし、使用するために、個別のAEM Assetsインスタンスをサポートします。
+ **Dynamic Media**&#x200B;のビデオサポートを更新しました。**360ビデオ**&#x200B;および&#x200B;**カスタムビデオサムネール**&#x200B;が含まれます。

### コンテンツインテリジェンス

AEMは、機械学習と人工知能を活用して、すべてのエクスペリエンスを向上させ、スマートテクノロジーとの統合を引き続き構築します。

+ **Adobeアセ** ットのリ **ンクは、視覚的類似性検索を追加**&#x200B;し、類似した画像を簡単に見つけて、 **Adobe Creative Cloudツール**&#x200B;内で使用できるようにします。

### 統合

AEMは、他のAdobeサービスと統合する機能を拡大します。

+ **エクスペ** リエンスフラグメントは、 **** JSONとしてAdobe Targetに書き出 **し** や、 **** Adobe Target **からエクスペリエンスフラグメントベースのオファーを削除する機能をサポートすることで、Adobeターゲットとの統合を深化しま**&#x200B;す。

### AMS Cloud Manager

[Adobe Managed Services(AMS)のお客様専用のCloud Manager](https://adobe.ly/2HODmsv)は、次の機能を提供します。

+ Cloud Managerでは、AEM Sitesから&#x200B;**AEM Assets**&#x200B;まで、AEMデプロイメントのサポートが拡張されています。**アセット処理の自動パフォーマンステスト**&#x200B;もサポートされます。
+ **事前に定義さ** れたしきい値でのAEMパブリッシュ層の自動スケーリングにより、エンドユーザーの最適なエクスペリエンスを実現します。
+ **非実稼動パイプライン** を使用すると、開発チームはCloud Managerを活用して、コード品質を継続的に確認し、低い環境（開発およびQA）にデプロイできます。
+ **CI/CDパイプラインAPIを使用す** ると、Cloud Managerをプログラムで操作し、オンプレミス開発インフラストラクチャとの統合の可能性を広げることができます。

## 基盤機能

以下に、AEMが提供する主な基盤機能の一覧を示します。 これらの機能の一部は、以前のバージョンで、各リリースで追加された増分機能の強化で導入されました。

+ [AEM Foundationリリースノート](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***✔<sup> + </sup> このバージョンの機能が大幅に強化されました。***

***✔<sup></sup>  SPは、サービスパックまたは機能パックを介して使用可能な機能を示します。***

<table>
    <thead>
        <tr>
            <td>基盤機能</td>
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
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Oak Content Repository</a>:</strong> 以前のJackrabbit 2よりもはるかに高いパフォーマンスと拡張性を提供します。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">oak-run.jarインデックスのサポート</a>:</strong> Oakインデックスの再インデックス/インデックス作成、統計の収集、一貫性の確認が改善されました。</td>
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
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">オンラインでのリビジョンクリーンアップ</a>:</strong>
                サーバーのダウンタイムを発生させずにリポジトリのメンテナンスを実行します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">TarMKまたはMongoMKリポジトリストレージ</a>:</strong>
                <br> TarMK（TarPMの次世代バージョン）のシンプルでパフォーマンスの高いファイルベースのストレージを使用するか、MongoMKを使用してMongoDBリポジトリを水平方向に拡張するかのオプションで
                <br> す。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-with-mongodb.html" target="_blank">MongoMKのパフォーマンスと安定性</a>:</strong>
            AEM 6.0での導入以降、MongoMKに対して継続的な機能強化がおこなわれています。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore">Amazon S3データストア</a>:</strong>
            バイナリアセットを保存するには、拡張可能なクラウドストレージソリューションを利用します。</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>タッチ操作対応UI機能の同等性：</strong>
                生産性の向上とクラシックUIとの機能の同等性により、オーサリングUIが引き続き強化されます。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>オムニサーチ：</strong>
                AEMをすばやく検索およびナビゲートします。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">操作ダッシュボード</a>:</strong>
 AEM内からメンテナンスの実行、サーバーの正常性の監視、パフォーマンスの分析をおこないます。</td>
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
            アップグレードの改善により、AEMのインプレースアップグレードが容易かつ迅速になります。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/htl/using/overview.html" target="_blank">HTLテンプレート言語</a>:</strong>
            プレゼンテーションをロジックから分離する最新のテンプレートエンジン。コンポーネントの開発時間を大幅に短縮します。 各リリースで追加された増分機能。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">Sling Models</a>:</strong>
            JCRリソースをビジネスオブジェクトとロジックにモデリングするための柔軟なフレームワークです。各リリースで追加された増分機能。
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
                Adobe Managed Services(AMS)のお客様専用のCloud Managerは、最新のCI/CDパイプラインの状態を通じて、開発とデプロイメントを加速します。</td>
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

以下に、AEMが提供する主なセキュリティ機能の一覧を示します。 これらの機能の一部は、以前のバージョンで、各リリースで追加された増分機能の強化で導入されました。

+ [セキュリティリリースノート](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***✔は、このバージョンでの機能の大幅な強化を示しています。***

***✔<sup> + </sup> は、この機能がサービスパックまたは機能パック経由で使用可能であることを示します。***

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
            <br> UsersCompartmentalizes権限は、管理者権限を不要に使用しないようにします。</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">キーストア管</a></strong>
            <br> 理グローバルトラストストア、証明書、およびキーは、すべてリポジトリ内で管理されます。</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong></strong> <strong></strong></a>
            <br> CSRFrotectionクロスサイトリクエストフォージェリ保護をすぐに使用できます。</td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong></strong> <strong></strong></a>
            <br> アプリケーションの柔軟性を高めるための、CORSsupportCross-originリソース共有のサポート。</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://docs.adobe.com/docs/en/aem/6-5/administer/security/saml-2-0-authenticationhandler.html" target="_blank">SAML認証のサポートの向</a><br>
 </strong>上SAMLリダイレクト、最適化されたグループ情報、および鍵の暗号化の問題が解決されました。 
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
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">LDAP as an OSGi Configuration LDAP認証</a><br>
 </strong>の管理と更新を簡素化します。</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong>OSGiでのプレーンテキストパスワードの暗<br>
 </strong>号化のサポートパスワードおよびその他の機密値は、暗号化された形式で保存し、自動的に復号化できます。</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">CUGの強</a><br>
 </strong>化閉じられたユーザーグループの実装は、パフォーマンスとスケーラビリティの問題に対処するために書き直されました。</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔<sup>+</sup></td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">SSL WizardUIを使</a></strong>
            <br> 用して、SSLの設定と管理を簡略化できます。</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">カプセル化されたト</a></strong>
            <br> ークンのサポート「スティッキー」セッションで、パブリッシュインスタンス間の水平認証をサポートする必要がなくなりました。</td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">AdobeIMS認証のサ</a><br>
 </strong>ポートAdobe Managed Services(AMS)専用で、AdobeIMS(Identity Managementシステム)を介してAEMオーサーインスタンスへのアクセスを一元管理します。</td>
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

以下に、AEMが提供する主なSites機能の一覧を示します。 これらの機能の一部は、以前のバージョンで、各リリースで追加された増分機能の強化で導入されました。

+ [AEM Sitesリリースノート](https://helpx.adobe.com/experience-manager/6-5/release-notes/sites.html)

***✔<sup> + </sup> このバージョンの機能が大幅に強化されました。***

***✔<sup></sup>  SPは、サービスパックまたは機能パックを介して使用可能な機能を示します。***

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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank">タッチ操作向けページのオーサリング</a>:</strong>
            タッチスクリーンを使用したタブレットやコンピューターを利用できます。</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">レスポンシブサイトオーサリング</a>:</strong>
                レイアウトモードを使用すると、エディターは、レスポンシブサイトのデバイスの幅に基づいてコンポーネントのサイズを変更できます。</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/template-editor-feature-video-use.html" target="_blank">編集可能なテンプレート</a>:</strong>
            専門的な作成者がページテンプレートを作成および編集できるようにします。</td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/core-components/user-guide.html" target="_blank">コアコンポーネント</a>:</strong>
            サイト開発を高速化します。頻繁なリリーススケジュールと柔軟性を考慮して、GitHubで入手できます。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">SPA Editor</a>:</strong>
            ReactまたはAngularに基づいて構築されたシングルページアプリケーション(SPA)フレームワークを使用して、オーサリング可能で有効なWebエクスペリエンスを作成します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/release-notes/style-system-fp.html" target="_blank">スタイルシステム</a>:</strong>
            コンテキスト内スタイルシステムを使用して外観を定義し、AEMコンポーネントの再利用を増やします。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">マルチサイトマネージャー(MSM)</a>:</strong>
            共通のコンテンツを共有する複数のWebサイト（多言語、複数のブランド）を管理します。</td>
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
            プラグアンドプレイフレームワークは、業界をリードするサードパーティの翻訳サービスと統合されます。</td>
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
            コンテンツをパーソナライズするための次世代のクライアントコンテキストフレームワーク。</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/launches.html" target="_blank">ローンチ</a>:</strong>
            毎日のオーサリングを中断することなく、将来のリリース向けにコンテンツを開発できます。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-understand.html" target="_blank">コンテンツフラグメント</a>:</strong>
            編集用コンテンツを作成およびキュレーションし、プレゼンテーションから組み合わせを解除して、再利用しやすくします。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html" target="_blank">エクスペリエンスフラグメント</a>:</strong>
            デスクトップ、モバイル、ソーシャルの各チャネル用に最適化された、再利用可能なエクスペリエンスやバリエーションを作成します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/release-notes/content-services-fragments-featurepack.html" target="_blank">コンテンツサービス</a>:</strong>
            デバイスやアプリケーションをまたいで使用するために、AEMからJSON形式でコンテンツを書き出します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Adobe Analyticsの統合とコンテンツのインサイト：</strong>
                Adobe AnalyticsとDTMの統合が容易になります。オーサー環境内でパフォーマンス情報を表示します。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/sites/authoring/using/content-targeting-touch.html" target="_blank">Adobe Targetの統合</a>:</strong>
            ターゲット設定されたエクスペリエンスを作成し、再利用可能なオファーライブラリを作成するためのステップバイステップ形式のウィザード。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank">Adobe Campaignとの統合</a>:</strong>
            次世代の電子メールキャンペーンソリューションとの簡単な統合。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/using/aem_launch_adobeio_integration.html" target="_blank">AdobeLaunchの統合</a>:</strong>
            Adobeの次世代タグ管理クラウドサービスと統合します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-screens-introduction.html" target="_blank">Screens</a>:</strong>
            デジタルサイネージとキオスクのエクスペリエンスを管理します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ecommerce.html" target="_blank">eコマース</a>:</strong>
            Web、モバイル、ソーシャルの各タッチポイントにわたって、ブランド化されたパーソナライズされたショッピングエクスペリエンスを提供します。
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
            フォーラム、スレッドコメント、イベントカレンダー、その他の多くの機能を使用すると、サイト訪問者との深いエンゲージメントを実現できます。</td>
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

## Assetsの機能

以下に、AEMが提供する主なAssets機能の一覧を示します。 これらの機能の一部は、以前のバージョンで、各リリースで追加された増分機能の強化で導入されました。

+ [AEM Assetsリリースノート](https://helpx.adobe.com/experience-manager/6-5/release-notes/assets.html)

***✔は、このバージョンでの機能の大幅な強化を示しています。***

***✔<sup> + </sup> は、この機能がサービスパックまたは機能パック経由で使用可能であることを示します。***

<table>
    <thead>
        <tr>
            <td>Assets機能</td>
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
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">タッチ操作向けUI</a>:</strong>
            デスクトップコンピューターまたはタッチ操作対応デバイスでアセットを管理します。</td>
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
            メタデータテンプレート、メタデータスキーマエディターおよび一括メタデータ編集。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank"></a> タスクとワー <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects-with-workflows.html" target="_blank"></a> クフロー管理：</strong>
            AEMプロジェクトを活用したデジタルアセットのレビューと承認のための事前に作成されたワークフローとタスク。</td>
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
            大規模な取り込み、アップロード、保存のサポートが強化されました。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">Assets HTTP API</a>:</strong>
            HTTPおよびJSONを介してプログラムによってアセットとやり取りします。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/link-sharing.html" target="_blank">リンク共有</a>:</strong>
            ログインしなくてもデジタルアセットをシンプルにアドホック共有できます。</td>
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
            デジタルアセットをシームレスに共有および配信するためのクラウドサービスSAASソリューション。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/use-assets-across-connected-assets-instances.html" target="_blank">Connected Assets</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">アセットインサイト</a>:</strong>
            Adobe Analyticsを活用して、デジタルアセットの顧客インタラクションをキャプチャし、AEMで表示します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/multilingual-assets.html" target="_blank">多言語アセット</a>:</strong>
            言語ルートを使用したアセットメタデータの翻訳サポート。</td>
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
            Adobe Senseiを利用して、有用なメタデータで画像に自動的にタグ付けします。</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">スマート翻訳検索</a>:</strong>
            AEM Assetsを検索する際に検索語句を自動的に翻訳します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/indesign.html" target="_blank">Adobe InDesign Server統合</a>:</strong>
            製品カタログを生成します。パンフレット、チラシ、印刷広告を、広告テンプレートに基づいてInDesignします。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-desktop-app/using/using.html?lang=ja" target="_blank">AEM Desktop App</a>:</strong>
            アセットをローカルデスクトップに同期して、Creative Suite製品で編集します。
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
                <br> 高品質なファイル操作に使用されるPhotoshopおよびAcrobat PDFライブラリ。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/jp/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">Adobeアセットリンク</a>:</strong>
            クラウド作成アプリケーションからAEM Assetsに直接アクセスします。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank">Adobe Stockの統合</a>:</strong>
            AEMから直接Adobe Stock画像にシームレスにアクセスし、使用できます。</td>
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

***✔<sup> + </sup> このバージョンの機能が大幅に強化されました。***

***✔<sup></sup>  SPは、サービスパックまたは機能パックを介して使用可能な機能を示します。***


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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets.html" target="_blank">画像</a>:</strong>
            スマート切り抜きを含め、様々なサイズおよび形式で画像を動的に配信します。</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/interactive-images.html" target="_blank">インタラクティブメディア</a>:</strong>
            重要なオファーを紹介するためのクリック可能なコンテンツを含むインタラクティブバナーやビデオを作成します。
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
            <td><strong>セット(<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">画像</a>、 <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">スピン</a>、 <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank">混在メディア</a>):</strong>
            360度の視聴エクスペリエンスを、ユーザーがズーム、パン、回転およびシミュレートできるようにします。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://docs.adobe.com/docs/ja-JP/aem/6-5/administer/content/dynamic-media/viewer-presets.html" target="_blank">ビューア</a>:</strong>
            様々な画面やデバイスをサポートするカスタムブランドのリッチメディアプレーヤーおよびプリセット。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/delivering-dynamic-media-assets.html" target="_blank">配信</a>:</strong>
            Dynamic Mediaコンテンツのリンクまたは埋め込みと、HTTP/2プロトコルを使用した配信に関する柔軟なオプション。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Scene7からDynamic Mediaへのアップグレード：</strong>
            マスターアセットを移行し、既存のS7 URLを引き続き使用する機能。</td>
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

## Formsの機能

AEMが提供する主なAEM Formsアドオン機能の一覧を以下に示します。 これらの機能の一部は、以前のバージョンで、各リリースで追加された増分機能の強化で導入されました。

+ [AEM Formsリリースノート](https://helpx.adobe.com/experience-manager/6-5/release-notes/forms.html)

***✔<sup> + </sup> このバージョンの機能が大幅に強化されました。***

***✔<sup></sup>  SPは、サービスパックまたは機能パックを介して使用可能な機能を示します。***

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
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">アダプティブFormsエディター</a>:</strong>
            デバイスやブラウザーの設定に基づいて、魅力的でレスポンシブなアダプティブフォームを作成します。</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">レコードのドキュメント</a>:</strong>
            データキャプチャエクスペリエンスや印刷準備ができたバージョンを長期にわたって保存するためのドキュメントを作成します。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/forms/using/themes.html" target="_blank">テーマエディター</a>:</strong>
            フォームのコンポーネントやパネルのスタイルを設定するために、再利用可能なテーマを作成します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/forms/using/template-editor.html" target="_blank">テンプレートエディター</a>:</strong>
            アダプティブフォームのベストプラクティスを標準化し、実装します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Adobe Sign統合</a>:</strong>
            Adobe Sign統合フォームベースの署名シナリオのデプロイを許可します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/cm-overview.html" target="_blank">Correspondence Management</a>:</strong>
            AEM Formsでは、パーソナライズされたインタラクティブな顧客通信を作成、管理、配信できます。
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">サードパーティデータの統合</a>:</strong>
            データ統合を使用すると、フォームのユーザー入力に基づいて、異なるデータソースからデータを取得します。この場合、フォームを送信すると、取得したデータが取得元のデータソースに再度書き込まれます。
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">Forms処理用のワークフロー（OSGi上）</a>:</strong>
            フォーム承認プロセスのデプロイメントの簡略化。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/user-guide.html?topic=/experience-manager/6-5/forms/morehelp/integrations.ug.js" target="_blank">Marketing Cloudとの統合</a>:</strong>
            Adobe AnalyticsおよびAdobe Targetとの統合により、顧客体験を強化および測定できます。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank">Form Manager</a>:</strong>
            分析、翻訳、A/Bテスト、レビュー、公開の有効化など、すべてのフォーム/ドキュメント/通信を1か所で管理できます。
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/aem-forms-app.html" target="_blank">AEM Formsアプリ</a>:</strong>
            iOS、AndroidまたはWindowsのアプリ内でオンライン/オフラインのフォーム処理が可能です。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/adaptive-document.html" target="_blank">インタラクティブ通信</a>:</strong>
            グラフなどのインタラクティブ要素（旧称「アダプティブドキュメント」）を含むターゲットステートメントなど、リッチ通信を作成します。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/pdf/aem-forms/6-5/WorkbenchHelp.pdf" target="_blank">Forms処理のワークフロー(J2EE)</a>:</strong>
            直感的なIDEを使用して、複雑なフォームやドキュメント中心のワークフローを構築します。</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank">AEM Forms Document Security</a>:</strong>
            PDFおよびOfficeドキュメントへの安全なアクセスと認証。
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">フレームワークのテスト</a>:</strong>
            アダプティブフォームのサポートとデバッグには、CalvinフレームワークとChromeプラグインを使用します。</td>
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

AEMが提供する主なAEM Communitiesアドオン機能の一覧を以下に示します。 これらの機能の一部は、以前のバージョンで、各リリースで追加された増分機能の強化で導入されました。

+ [AEM Communitiesの新機能の概要](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html#main-pars_text)

***✔<sup> + </sup> このバージョンの機能が大幅に強化されました。***

***✔<sup></sup>  SPは、サービスパックまたは機能パックを介して使用可能な機能を示します。***

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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/forum.html" target="_blank">フォーラム</a>:</strong> (Social Component Framework)新しいトピックを作成するか、既存のトピックを表示、フォロー、検索および移動します。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <p><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-qna.html" target="_blank">Q&amp;A</a>:</strong>
                質問の投稿、表示、回答を行います。</p>
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
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/blog-feature.html" target="_blank">ブログ</a>:</strong>
                投稿側でブログ記事とコメントを作成します。
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
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/ideation-feature.html" target="_blank">アイディエーション</a>:</strong>
                アイデアを作成してコミュニティと共有したり、既存のアイデアを表示、フォローおよびコメントしたりします。
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
                （ソーシャルコンポーネントフレームワーク）コミュニティイベント情報をサイト訪問者に提供します。
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
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/file-library.html" target="_blank">ファイルライブラリ</a>:</strong>
                コミュニティサイト内のファイルをアップロード、管理、ダウンロードします。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/users.html#AboutCommunityGroups" target="_blank">ユーザーグループ</a>:
            </strong>一連のユーザーは、メンバーグループに属し、複数のロールをまとめて割り当てることができます。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong> </strong></td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">割り当て</a>:</strong>
            学習リソースを作成してコミュニティメンバーに割り当てます。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">Enablement</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/catalog.html" target="_blank"></a> カタログとリソ <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">ース管理</a>:</strong>
            カタログからイネーブルメントリソースにアクセスします。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#CreateaLearningPath" target="_blank">学習パスの管理</a>:</strong>
            イネーブルメントリソースのコースまたはグループを管理します。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reports.html#main-pars_text_1739724213" target="_blank">イネーブルメントレポート</a>:</strong>
            イネーブルメントリソースと学習パスに関するレポート。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#main-pars_text_899882038" target="_blank">イネーブルメントのエンゲージメント</a>:</strong>
            イネーブルメントリソースに関するコメントを追加します。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">イネーブルメント分析</a>:</strong>
            ビデオ分析、進捗状況レポート、および割り当てレポート</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="8">コモンズ</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/comments.html" target="_blank"></a> コメントと添付ファイル：</strong>
            （ソーシャルコンポーネントフレームワーク）コミュニティメンバーは、コミュニティサイトのコンテンツに関する意見や知識を共有します。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>コンテンツフラグメントの変換：</strong>
            UGCの投稿をコンテンツフラグメントに変換します。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reviews.html" target="_blank">レビュー</a>:</strong>
                （ソーシャルコンポーネントフレームワーク）コミュニティメンバーは、コメントと評価機能を組み合わせて、コンテンツの一部をレビューします。</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/rating.html" target="_blank">評価</a>:/strong&gt;（ソーシャルコンポーネントフレームワーク）コミュニティメンバーは、コンテンツの一部を評価します。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/voting.html" target="_blank">投票</a>:</strong>
                （ソーシャルコンポーネントフレームワーク）コミュニティメンバーは、コンテンツの一部に対して投票または投票を停止します。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tag-ugc.html" target="_blank">タグ</a>:</strong>
            コンテンツにタグ（キーワードまたはラベル）を付けて、コンテンツをすばやく見つけることができます。</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/translate-ugc.html" target="_blank">翻訳</a>:</strong>
            ユーザー生成コンテンツの機械翻訳。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="10">管理</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/create-site.html" target="_blank">サイト管理</a>:</strong>
            コミュニティ機能を使用したサイトの作成。</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">テンプレート</a>:</strong>
                <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank"></a> 完全に機能する <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tools-groups.html" target="_blank"></a> コミュニティサイトをウィザードベースで作成するためのサイトおよびグループテンプレート。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>編集可能なテンプレート：</strong>
            コミュニティ管理者が、AEM編集可能なテンプレートを使用してリッチなエクスペリエンスを構築できるようにします。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/creating-groups.html" target="_blank">グループまたはサブコミュニティ</a>:</strong>
            コミュニティサイト内でサブコミュニティを動的に作成します。
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
            ユーザー生成コンテンツのモデレート
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
            ユーザー生成コンテンツを一括管理するモデレートコンソール。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderate-ugc.html#CommonModerationConcepts" target="_blank">スパム検出と不敬な言葉遣いのフィルター</a>:</strong>
            自動スパム検出。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/members.html" target="_blank">メンバー管理</a>:</strong>
            メンバー管理領域からユーザープロファイルおよびグループを管理します。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html#main-pars_text_866731966" target="_blank">レスポンシブデザイン</a>:</strong>
            AEM Communitiesサイトはレスポンシブです。
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
            Adobe Analyticsと統合して、Communitiesのサイトの使用に関する主要なインサイトを得ます。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="4">メンバー</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/advanced.html" target="_blank">スコアとバッジ</a>:</strong>
            (Adobe Senseiによる高度なスコア)コミュニティメンバーをエキスパートとして特定し、報酬を与えます。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/activities.html" target="_blank"></a> アクティビティと <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/notifications.html" target="_blank">通知</a>:</strong>
            最近のアクティビティストリームを表示し、関心のあるイベントに関する通知を受け取ります。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/configure-messaging.html" target="_blank">メッセージ</a>:</strong>
            ユーザーおよびグループにダイレクトメッセージを送信します。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/communities/using/social-login.html" target="_blank">ソーシャルログイン</a>:</strong>
            FacebookまたはTwitterアカウントでログインします。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">プラットフォーム</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">MSRP（Mongoストレージ）</a>:</strong>
            ユーザー生成コンテンツ(UGC)は、ローカルのMongoDBインスタンスに直接保持されます</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">DSRP（データベースストレージ）</a>:</strong>
            ユーザー生成コンテンツ(UGC)は、ローカルのMySQLデータベースインスタンスに直接保持されます。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">SRP（クラウドストレージ）</a>:</strong>
                ユーザー生成コンテンツ(UGC)は、Adobeがホストおよび管理するクラウドサービスでリモートに保持されます。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank"><strong>JSRP</a>:</strong>
                コミュニティコンテンツはJCRに格納され、UGCは、その投稿先のオーサー（またはパブリッシュ）インスタンスからアクセスできます。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sync.html" target="_blank">ユーザーとグループの同期</a>:</strong>
            パブリッシュファームトポロジを使用する場合、パブリッシュインスタンス間でユーザーとグループを同期します。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

AEM Communitiesは、次の方法で組織がユーザーを惹きつけ、ユーザーを有効にできるようにする[機能強化](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html)をリリースを追加しました。

+ **ユーザ** ー生成コンテンツでの@mentionsupport。
+ **イネーブルメント**&#x200B;コンポーネントの&#x200B;**キーボードナビゲーション**&#x200B;によるアクセシビリティの改善。
+ **カスタムフィルター**&#x200B;を使用した&#x200B;**一括モデレート**&#x200B;を改善しました。
+ **編集可能なテ** ンプレートを使用すると、コミュニティ管理者がAEMで豊富なコミュニティエクスペリエンスを構築できるようになります。
+ ユーザーは、**ダイレクトメッセージを一括**&#x200B;でグループのすべてのメンバーに送信できるようになりました。
