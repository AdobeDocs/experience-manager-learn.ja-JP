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

+ **Java 11のサポート** （Java 8のサポートを維持しながら）。

### Webサイトの作成と管理

AEM Sitesは、ウェブサイトの作成と構築を高速化するために設計された多数の機能を紹介しています。

+ **SPAエディターのサポートにより** 、SPA（シングルページアプリケーション）をAEMで完全に作成し、マーケティング担当者にとって使いやすい豊富なオーサリングエクスペリエンスをサポートします。
+_ **JavaScript SDK**、SPAプロジェクト開始キットとサポートするビルドツールを使用すると、フロントエンド開発者は、AEMとは独立してSPAエディタ互換の単一ページアプリケーションを開発できます。
+ **コアコンポーネント** ：新しいコンポーネント、 **コンポーネントライブラリ** 、および既存のコアコンポーネントに対する様々な拡張を追加します。
+ さらに **翻訳を改良し** 、AEM Sitesの翻訳を合理化。

### 流体エクスペリエンス

AEMは、AEM以外でのコンテンツの使用を容易にする新しい改善されたツールを使用して、流体エクスペリエンスを引き続き受け入れます。

+ **コンテンツフラグメントは** 、バージョンの比較/相違と注釈をサポートしています。
+ **AEM Assets HTTP API** :DAM内での **コンテンツフラグメントの** JSONとしての直接公開をサポートします ****。
   **エクスペリエンスフラグメントは** 、 **ページ参照のための** 全文検索 **とAEMディスパッチャーキャッシュの無効化をサポートしてい** ます ****。

### アセット管理

AEM Assetsは、DAMの使用、管理、理解を向上させるため、豊富な資産管理機能を基盤として構築を続けています。 AEM 6.5では、Adobe Creative Cloudとクリエイティブワークフローの統合が引き続き改善されます。

+ **「Adobeアセットリンク** 」は、クリエイティブをAdobe Creative CloudツールからAEM Assetsに直接接続します。
+ **Adobe Stock** 統合により、AEM Assetsの経験から直接Adobe Stock画像にアクセスでき、シームレスなコンテンツ発見体験を実現します。
+ **AEM Desktop App** Version 2.0がリリースされ、パフォーマンスと安定性が向上しながら、自らを再エンビジョンします。
+ **接続されたアセット** ：独立したAEM Sitesインスタンスをサポートし、異なるAEM Assetsインスタンスのアセットにシームレスにアクセスし、使用します。
+ 360ビデオ **と**&#x200B;カスタムビデオサムネールなど、 **ダイナミックメディアでのビデオのサポートが更新されました******。

### コンテンツ情報

AEMは、機械学習と人工知能を活用して、スマートテクノロジーとの統合を継続的に構築し、すべてのエクスペリエンスを向上させます。

+ **Adobeアセットリンク** : **視覚的類似性検索を追加して**、 **** Adobe Creative Cloudツール内で類似の画像を簡単に見つけ、使用できるようにします。

### 統合

AEMは、他のAdobeサービスとの統合機能を拡張します。

+ **Experience Fragmentsは、「JSON** ベースのオファーとしてエクスポート **」をAdobe Targetにサポートし、「エクスペリエンスフラグメントベースの** をJSONAdobe Targetから **削除する」をサポートすることで、** Adobe Targetとの統合を ********&#x200B;深めます。

### AMS Cloud Manager

[Adobe Managed Services](https://adobe.ly/2HODmsv)(AMS)のお客様専用のCloud Managerでは、次の機能がオファーに提供されます。

+ Cloud Managerは、アセット処理の **自動パフォーマンステストを含む、AEM Sitesから** AEM Assets **まで、AEMの展開サポートを拡張します**。
+ **事前に定義されたしきい値でのAEM Publish層の自動拡大縮小** 。エンドユーザーの操作性を最適化します。
+ **非実稼動パイプラインを使用すると** 、開発チームはCloud Managerを利用してコードの品質を継続的に確認し、環境を下げる（開発とQA）ためのデプロイを行うことができます。
+ **CI/CDパイプラインAPIを使用すると** 、お客様はプログラムを使用してCloud Managerと連携でき、オンプレミスの開発インフラストラクチャとの統合の可能性を深めることができます。

## 基礎機能

以下に、AEMが提供する主な基盤機能の一覧を示します。 これらの機能の一部は、以前のバージョンで導入され、各リリースで追加された増分機能です。

+ [AEM Foundationリリースノート](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***このバージョンの✔機能に対する大幅な強化点です。<sup></sup>***

***✔<sup>SP</sup>:Service PackまたはFeature Packを介して使用可能な機能であることを示します。***

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
                <strong>Java 11のサポート：</strong> AEMは、Java 11（およびJava 8）をサポートしています。
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
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Oak Content Repository</a>:</strong> 前任のJackrabbit 2よりもはるかに高いパフォーマンスと拡張性を提供します。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">oak-run.jarインデックスのサポート</a>:</strong> Oakインデックスの再インデックス作成、統計情報収集、整合性チェックを改善。</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">オンラインリビジョンのクリーンアップ</a>:</strong>               サーバのダウンタイムを伴わずにリポジトリの保守を実行</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">TarMKまたはMongoMKリポジトリストレージ</a>:</strong>               <br> シンプルでパフォーマンスの高いTarMK（TarPMの次世代版）のファイルベースストレージを使用するか、MongoDB対応リポジトリをMongoMKで使用して水平方向に拡張するかを選択で <br> きます。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-with-mongodb.html" target="_blank">MongoMKのパフォーマンスと安定性</a>:</strong>           MongoMKはAEM 6.0での導入以降、引き続き機能強化が行われています。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore">AmazonS3 DataStore</a>:</strong>           拡張可能なクラウドストレージソリューションを活用して、バイナリアセットを保存できます。</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>タッチ操作対応UI機能のパリティ：</strong>               従来のUIと同等の生産性と機能を実現し、オーサリングUIの機能強化を継続。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Omnisearch:</strong>               AEMの検索とナビゲートをすばやく行います。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">操作ダッシュボード</a>:</strong>AEM内でメンテナンスの実行、サーバの正常性の監視、パフォーマンスの分析を行います。</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">アップグレードの改善</a>:</strong>           アップグレードの機能強化により、AEMのインプレースアップグレードを簡単かつ迅速に行うことができます。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/htl/using/overview.html" target="_blank">HTL Template Language</a>:</strong>           プレゼンテーションと論理を分離する最新のテンプレート化エンジン。 コンポーネントの開発時間を大幅に短縮できます。 各リリースで追加される増分機能</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">Slingモデル</a>:</strong>           JCRリソースをビジネスオブジェクトやロジックにモデリングするための柔軟なフレームワーク。 各リリースで追加される増分機能
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

***✔<sup>+</sup>は、Service PackまたはFeature Packを使用して機能を利用できることを示します。***

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
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">サービスユーザー</a></strong><br> ：権限を区分化し、管理者権限を不要に使用しないようにします。</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">キーストアの管理</a></strong><br> グローバルTrust Store、証明書、およびキーはすべてリポジトリ内で管理されます。</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong>CSRF</strong><strong>保護</strong></a><br> — クロスサイト要求偽造保護をすぐに使用できます。</td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong>CORS</strong><strong>サポート</strong></a><br> (接触チャネル間のリソース共有)は、アプリケーションの柔軟性を高めるためのサポートです。</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://docs.adobe.com/docs/en/aem/6-5/administer/security/saml-2-0-authenticationhandler.html" target="_blank">SAML認証のサポートの強化</a><br>SAMLリダイレクトの </strong>改善、最適化されたグループ情報、およびキー暗号化の問題が解決されました。
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
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">OSGi設定としてのLDAP</a><br>:LDAP認証の管理と更新を </strong>簡素化します。</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong>プレーンテキストパスワードのOSGi暗号化のサポート<br></strong>パスワードやその他の機密値は、暗号化された形式で保存し、自動的に復号化できます。</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">CUGの強化</a><br></strong>クローズドユーザーグループの実装は、パフォーマンスと拡張性の問題に対処するために書き直されました。</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔<sup>+</sup></td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">SSL Wizard</a></strong>UI(SSL Wizard <br> UI):SSLのセットアップと管理を簡単にします。</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">カプセル化トークンのサポート</a></strong><br> 「スティッキー」セッションで、パブリッシュインスタンス間の水平認証をサポートする必要がなくなりました。</td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">Adobe Managed Services(AMS)専用のAdobeIMS認証サポート</a><br></strong>(AMS)を使用すると、AdobeIMS(Identity Managementシステム)を介したAEM Authorインスタンスへのアクセスを一元的に管理できます。</td>
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

***このバージョンの✔機能に対する大幅な強化点です。<sup></sup>***

***✔<sup>SP</sup>:Service PackまたはFeature Packを介して使用可能な機能であることを示します。***

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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank">タッチ操作向けに最適化されたページオーサリング</a>:</strong>           タッチスクリーンを備えたタブレットやコンピューターを利用できるようにします。</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">レスポンシブサイトオーサリング</a>:</strong>               レイアウトモードを使用すると、レスポンシブサイトのデバイスの幅に基づいて、コンポーネントのサイズを変更できます。</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/template-editor-feature-video-use.html" target="_blank">編集可能なテンプレート</a>:</strong>           専門的な作成者がページテンプレートを作成および編集できるようにします。</td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/core-components/user-guide.html" target="_blank">コアコンポーネント</a>:</strong>           サイト開発を迅速化 GitHubで、頻繁なリリーススケジュールと柔軟性を提供。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">SPAエディタ</a>:</strong>           ReactまたはAngularに基づいて構築されたシングルページアプリケーション(SPA)フレームワークを使用して、承認可能でエネージング可能なWebエクスペリエンスを作成します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/release-notes/style-system-fp.html" target="_blank">Style System</a>:</strong>           コンテキスト内スタイルシステムを使用して外観を定義することで、AEMコンポーネントの再利用率を高めます。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">マルチサイトマネージャ(MSM)</a>:</strong>           共通のコンテンツを共有する複数のWebサイト（多言語、複数のブランドなど）を管理します。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">コンテンツの翻訳</a>:</strong>           プラグアンドプレイフレームワークは、業界をリードするサードパーティの翻訳サービスと統合されています。</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html" target="_blank">ContextHub</a>:</strong>           コンテンツのパーソナライゼーションのための次世代クライアントコンテキストフレームワーク。</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/launches.html" target="_blank">起動回数</a>:</strong>           日々のオーサリングを妨げることなく、将来のリリース向けにコンテンツを開発できます。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-understand.html" target="_blank">コンテンツフラグメント</a>:</strong>           再利用しやすいように、プレゼンテーションから結合を解除した編集コンテンツを作成およびキュレーションします。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html" target="_blank">エクスペリエンスフラグメント</a>:</strong>           デスクトップ、モバイルおよびソーシャルチャネルに最適化された、再利用可能なエクスペリエンスやバリエーションを作成します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/release-notes/content-services-fragments-featurepack.html" target="_blank">Content Services</a>:</strong>           AEMからJSONとしてコンテンツを書き出し、デバイスやアプリケーション全体で使用できます。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Adobe Analytics統合とコンテンツのインサイト：</strong>               Adobe AnalyticsとDTMの統合が容易 作成者環境内にパフォーマンス情報を表示します。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/sites/authoring/using/content-targeting-touch.html" target="_blank">Adobe Target統合</a>:</strong>           ターゲット設定されたエクスペリエンスを作成し、再利用可能なオファーライブラリを作成するためのステップバイステップ方式のウィザード。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank">Adobe Campaign統合</a>:</strong>           次世代の電子メールキャンペーンソリューションとの容易な統合。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/using/aem_launch_adobeio_integration.html" target="_blank">Adobe起動の統合</a>:</strong>           Adobeの次世代tag managementクラウドサービスと統合できます。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-screens-introduction.html" target="_blank">画面</a>:</strong>           デジタル署名とキオスクのエクスペリエンスを管理します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ecommerce.html" target="_blank">eコマース</a>:</strong>           Web、モバイルおよびソーシャルのタッチポイントにわたって、ブランドのパーソナライズされた買い物体験を提供します。
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html" target="_blank">コミュニティ</a>:</strong>           フォーラム、スレッドコメント、イベントカレンダー、その他多くの機能を使用して、サイト訪問者に深い関与を与えることができます。</td>
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

***✔<sup>+</sup>は、Service PackまたはFeature Packを使用して機能を利用できることを示します。***

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
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">タッチ操作向けUI</a>:</strong>           デスクトップコンピューターまたはタッチ対応デバイスでアセットを管理します。</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/metadata.html" target="_blank">高度なメタデータ管理</a>:</strong>           メタデータテンプレート、メタデータスキーマエディタ、およびバルクメタデータの編集を参照してください。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank">タスク</a> と <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects-with-workflows.html" target="_blank">ワークフローの管理</a> :</strong>           AEMプロジェクトを利用したデジタルアセットのレビューと承認を行うための、事前にビルドされたワークフローとタスク。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>拡張性とパフォーマンス：</strong>           取り込み、アップロード、ストレージを拡張できるようになりました。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">アセットHTTP API</a>:</strong>           プログラムは、HTTPとJSONを使用してアセットとやり取りします。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/link-sharing.html" target="_blank">リンク共有</a>:</strong>           ログインしなくてもデジタルアセットを簡単にアドホック共有できます。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/brand-portal/using/brand-portal.html" target="_blank">ブランドポータル</a>:</strong>           クラウドサービスSAASソリューションにより、デジタルアセットのシームレスな共有と配信を実現。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/use-assets-across-connected-assets-instances.html" target="_blank">接続されているアセット</a>:</strong>           AEM Sitesインスタンスは、別のAEM Assetsインスタンスのアセットにシームレスにアクセスし、使用できます。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">アセットのインサイト</a>:</strong>           Adobe Analyticsを活用して、AEMのデジタル資産や表示の顧客とのやり取りを捕捉します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/multilingual-assets.html" target="_blank">多言語アセット</a>:</strong>           言語ルートを使用してアセットメタデータを自動的に変換できます。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/enhanced-smart-tags.html" target="_blank">スマートタグとモデレート</a>:</strong>           Adobe Senseiを活用して、有用なメタデータを持つ画像に自動的にタグ付けします。</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">スマート翻訳検索</a>:</strong>           AEM Assets検索時に検索用語を自動的に翻訳します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/indesign.html" target="_blank">Adobe InDesign Server統合</a>:</strong>           製品カタログを生成します。 InDesignテンプレートに基づいて、パンフレット、チラシ、印刷用広告を作成します。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/desktop-app/aem-desktop-app.html" target="_blank">AEM Desktop App</a>:</strong>           Creative Suite製品の編集用に、アセットをローカルデスクトップに同期します。
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank">Adobeイメージングライブラリ</a>:</strong>               <br> 高品質のファイル操作に使用されるPhotoshopおよびAcrobatのPDFライブラリ。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/jp/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">Adobeアセットリンク</a>:</strong>           AdobeのCreate CloudアプリケーションからAEM Assetsに直接アクセスできます。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank">Adobe Stock統合</a>:</strong>           Adobe Stockの画像に直接シームレスにアクセスし、使用できます。</td>
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

### AEM Assetsダイナミックメディア

***このバージョンの✔機能に対する大幅な強化点です。<sup></sup>***

***✔<sup>SP</sup>:Service PackまたはFeature Packを介して使用可能な機能であることを示します。***


<table>
    <thead>
        <tr>
            <td>ダイナミックメディア機能</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets.html" target="_blank">画像処理</a>:</strong>           スマート切り抜きなど、様々なサイズと形式で画像を動的に配信します。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/assets/using/video-profiles.html" target="_blank">ビデオ</a>:</strong>           高度なビデオエンコーディングとアダプティブビデオストリーミング</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/interactive-images.html" target="_blank">Interactive Media</a>:</strong>           クリック可能なコンテンツを含むインタラクティブバナーやビデオを作成して主要オファーを紹介します。
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
            <td><strong>セット(<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">画像</a>、 <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">スピン</a>、 <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank">混在メディア</a>):</strong>           360°の画像表示を、ズーム、パン、回転、シミュレーションできます。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://docs.adobe.com/docs/en/aem/6-5/administer/content/dynamic-media/viewer-presets.html" target="_blank">ビューア</a>:</strong>           様々な画面/デバイスをサポートするカスタムブランドのリッチメディアプレイヤーおよびプリセット。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/delivering-dynamic-media-assets.html" target="_blank">配信</a>:</strong>           ダイナミックメディアコンテンツのリンクまたは埋め込み、およびHTTP/2プロトコル経由の配信に関する柔軟なオプション。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Scene7からダイナミックメディアへのアップグレード：</strong>           マスターアセットを移行し、既存のS7 URLを引き続き使用する機能。</td>
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

***このバージョンの✔機能に対する大幅な強化点です。<sup></sup>***

***✔<sup>SP</sup>:Service PackまたはFeature Packを介して使用可能な機能であることを示します。***

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
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">アダプティブFormsエディタ</a>:</strong>           デバイスとブラウザーの設定に基づいて、魅力的でレスポンシブなアダプティブフォームを作成します。</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">レコードのドキュメント</a>:</strong>           データ取得の体験や印刷用のストレージを長期にわたって確実に行うドキュメントを作成します。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/forms/using/themes.html" target="_blank">テーマエディタ</a>:</strong>           再利用可能なテーマを作成して、フォームのコンポーネントやパネルのスタイルを設定します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/forms/using/template-editor.html" target="_blank">テンプレートエディタ</a>:</strong>           アダプティブフォームのベストプラクティスを標準化し、実装する。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Adobe Sign統合</a>:</strong>           Adobe Sign統合フォームベースの署名シナリオの展開を許可します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/cm-overview.html" target="_blank">Correspondence Management</a>:</strong>           AEM Formsでは、パーソナライズされたインタラクティブな顧客対応を作成、管理および配信できます。
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">サードパーティデータ統合</a>:</strong>           データ統合を使用すると、フォームのユーザー入力に基づいて、異なるデータソースからデータを取得できます。 この場合、フォームを送信すると、取得したデータが取得元のデータソースに再度書き込まれます。
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">Forms処理のワークフロー(OSGi)</a>:</strong>           フォーム承認プロセスのデプロイメントの簡素化。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/user-guide.html?topic=/experience-manager/6-5/forms/morehelp/integrations.ug.js" target="_blank">Marketing Cloudとの統合</a>:</strong>           Adobe AnalyticsおよびAdobe Targetとの統合により、顧客体験を強化し、測定できます。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank">Form Manager</a>:</strong>           分析、翻訳、A/Bテスト、レビュー、発行の有効化など、フォーム/ドキュメント/通信をすべて1か所で管理できます。
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/aem-forms-app.html" target="_blank">AEM Formsアプリ</a>:</strong>           iOS、Android、またはWindows上のアプリケーション内でオンライン/オフラインのフォーム処理を許可する</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/adaptive-document.html" target="_blank">Interactive Communications</a>:</strong>           グラフ(以前のアダプティブドキュメント)などのインタラクティブな要素を使用して、ターゲット設定された文など、豊富な通信を作成できます。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/pdf/aem-forms/6-5/WorkbenchHelp.pdf" target="_blank">Forms処理のワークフロー(J2EE)</a>:</strong>           直感的なIDEを使用して、複雑なフォームやドキュメント中心のワークフローを構築します。</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank">AEM Formsドキュメントセキュリティ</a>:</strong>           PDFおよびOfficeドキュメントへの安全なアクセスと認証。
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">テストフレームワーク</a>:</strong>           アダプティブフォームのサポートとデバッグを行うには、CalvinフレームワークとChromeプラグインを使用します。</td>
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

***このバージョンの✔機能に対する大幅な強化点です。<sup></sup>***

***✔<sup>SP</sup>:Service PackまたはFeature Packを介して使用可能な機能であることを示します。***

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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/forum.html" target="_blank">フォーラム</a>:</strong> (Social Component Framework)新しいトピック、または表示、追跡、検索、移動の既存のトピックを作成します。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <p><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-qna.html" target="_blank">QnA</a>:</strong>               質問、表示、質問への回答を行います。</p>
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
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/blog-feature.html" target="_blank">ブログ</a>:</strong>               投稿側でブログ記事とコメントを作成します。
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
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/ideation-feature.html" target="_blank">Ideation</a>:</strong>               コミュニティや表示とアイデアを作成し共有し、既存のアイデアに従ってコメントします。
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
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/calendar.html" target="_blank">カレンダー</a>:</strong>               (Social Component Framework)コミュニティイベント情報をサイト訪問者に提供します。
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
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/file-library.html" target="_blank">ファイルライブラリ</a>:</strong>               コミュニティサイト内のファイルをアップロード、管理およびダウンロードします。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/users.html#AboutCommunityGroups" target="_blank">User Groups</a>:            </strong>ユーザーのセットは、メンバーグループに属することができ、まとめてロールを割り当てることができます。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong> </strong></td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">割り当て</a>:</strong>           学習リソースを作成し、コミュニティメンバーに割り当てます。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">Enablement</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/catalog.html" target="_blank">カタログ</a> / <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">リソース管理</a>:</strong>           カタログから有効化リソースにアクセスします。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#CreateaLearningPath" target="_blank">学習パス管理</a>:</strong>           有効化リソースのコースまたはグループを管理します。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reports.html#main-pars_text_1739724213" target="_blank">有効化レポート</a>:</strong>           有効化リソースと学習パスに関するレポート。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#main-pars_text_899882038" target="_blank">有効化の取り組み</a>:</strong>           有効化追加リソースに関するコメント。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">有効化分析</a>:</strong>           ビデオ分析、進行状況レポートおよび割り当てレポート</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="8">コモンズ</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/comments.html" target="_blank">コメント</a> と添付ファイル：</strong>           (Social Component Framework)コミュニティのメンバーは、コミュニティサイトでコンテンツに関する意見や知識を共有します。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>コンテンツフラグメント変換：</strong>           コンテンツフラグメントに対するUGCの貢献度を変換します。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reviews.html" target="_blank">レビュー</a>:</strong>               (Social Component Framework)コミュニティのメンバーは、コメントと評価機能を組み合わせて、コンテンツの一部をレビューします。</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/voting.html" target="_blank">投票数</a>:</strong>               (Social Component Framework)コミュニティのメンバーは、コンテンツの一部に対して賛成または反対の投票を行います。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tag-ugc.html" target="_blank">タグ</a>:</strong>           コンテンツをすばやく見つけるために、タグ（キーワードまたはラベル）にコンテンツを付加します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/search.html" target="_blank">検索</a>:</strong>           予測検索と示唆的検索。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/translate-ugc.html" target="_blank">翻訳</a>:</strong>           ユーザー生成コンテンツの機械翻訳。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="10">Administration</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/create-site.html" target="_blank">サイト管理</a>:</strong>           コミュニティ機能を持つサイトの作成</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">テンプレート</a>:</strong>               <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">完全に機能するコミュニティサイトをウィザードベースで作成するためのサイト</a> および <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tools-groups.html" target="_blank"></a> グループテンプレート。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>編集可能なテンプレート：</strong>           コミュニティ管理者は、AEMの編集可能なテンプレートを使用して、豊富なエクスペリエンスを構築できます。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/creating-groups.html" target="_blank">グループまたはサブコミュニティ</a>:</strong>           コミュニティサイト内にサブコミュニティを動的に作成します。
            </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/in-context.html" target="_blank">モデレート</a>:</strong>           ユーザー生成コンテンツのモデレート。
            </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderation.html" target="_blank">一括モデレート</a>:</strong>           モデレートコンソールを使用して、ユーザー生成コンテンツを一括管理します。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderate-ugc.html#CommonModerationConcepts" target="_blank">スパム検出と不敬のフィルター</a>:</strong>           自動スパム検出。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/members.html" target="_blank">会員管理</a>:</strong>           メンバー管理領域からユーザープロファイルおよびグループを管理します。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html#main-pars_text_866731966" target="_blank">レスポンシブデザイン</a>:</strong>           AEM Communitiesのサイトはレスポンシブです。
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">解析</a>:</strong>           Adobe Analyticsと統合して、コミュニティのサイトの使用に関する主要なインサイトを得る。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="4">メンバー</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/advanced.html" target="_blank">スコアリングとバッジ</a>:</strong>           (Adobe Senseiによる高度スコアリング)コミュニティのメンバーを専門家として特定し、報酬を与えます。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/activities.html" target="_blank">アクティビティ</a> と <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/notifications.html" target="_blank">通知</a>:</strong>           表示の最近のアクティビティがストリーミングし、関心のあるイベントに関する通知を受け取ります。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/configure-messaging.html" target="_blank">メッセージ</a>:</strong>           ユーザーおよびグループへのダイレクトメッセージング。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/communities/using/social-login.html" target="_blank">ソーシャルログイン</a>:</strong>           FacebookまたはTwitterアカウントでサインインします。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">プラットフォーム</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">MSRP (Mongoストレージ)</a>:</strong>           ユーザー生成コンテンツ(UGC)は、ローカルのMongoDBインスタンスに直接保持されます</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">DSRP (データベースストレージ)</a>:</strong>           ユーザー生成コンテンツ(UGC)は、ローカルのMySQLデータベースインスタンスに直接保持されます。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">SRP(クラウドストレージ)</a>:</strong>               ユーザー生成コンテンツ(UGC)は、Adobeがホストし管理するクラウドサービスでリモートで保持されます。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank"><strong>JSRP</a>:</strong>               コミュニティコンテンツはJCRに保存され、UGCには、そのコンテンツが投稿された作成者（または発行）インスタンスからアクセスできます。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sync.html" target="_blank">ユーザーとグループの同期</a>:</strong>           発行ファームトポロジを使用する場合、発行インスタンス間でユーザーとグループを同期します。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

AEM Communitiesは [](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html) 、次のように組織がユーザーの関与とユーザーの有効化を可能にする機能強化をリリースしました。

+ **ユーザー生成コンテンツでの@mentionのサポート** 。
+ 有効化コンポーネントの **キーボードナビゲーション** ( **Keyboard Navigation** )を使用してアクセシビリティを改善。
+ カス **タムフィルターを使用した** 一括モデレート **を改善**。
+ **編集可能なテンプレート** ：コミュニティ管理者がAEMで豊富なコミュニティエクスペリエンスを構築できるようにします。
+ ユーザーは、 **ダイレクトメッセージを一括してグループのすべてのメンバーに送信できるようになりました** 。
