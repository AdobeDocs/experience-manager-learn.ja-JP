---
title: アップグレードの理由を理解する
description: お客様がAdobe Experience Manager 6 の最新バージョンへのアップグレードを検討する際の主な機能の大まかな分類。
version: 6.5
topic: Upgrade
role: Leader, Architect, Developer, Admin, User
level: Beginner
exl-id: bf4030b0-67c4-4b00-af95-f63e6f79e995
source-git-commit: 34fbb22916cf8a8df0e3240835c71e0979fd11bd
workflow-type: tm+mt
source-wordcount: '3460'
ht-degree: 7%

---

# アップグレードの理由の理解

お客様がAdobe Experience Manager 6 の最新バージョンへのアップグレードを検討する際の主な機能の大まかな分類。

## AEM 6.5 へのアップグレードの主な機能

+ [Adobe Experience Manager 6.5 のリリースノート](https://helpx.adobe.com/jp/experience-manager/6-5/release-notes.html)

### 基盤の強化

Adobe Experience Manager 6.5 は、次の手段を通じて、システムの安定性、パフォーマンス、サポート性を引き続き向上させます。

+ **Java 11** （Java 8 のサポートを維持しながら）をサポートします。

### Web サイトの作成と管理

AEM Sitesでは、Web サイトの作成と構築を高速化するために設計された、次のような様々な機能が導入されています。

+ **SPA Editor** のサポートにより、SPA（シングルページアプリケーション）をAEMで完全にオーサリングし、マーケターに優しい豊富なオーサリングエクスペリエンスをサポートできます。
+_ **JavaScript SDK の**、SPA Project Start Kit およびサポートするビルドツールを使用すると、フロントエンド開発者は、SPAとは別に、AEM Editor と互換性のある単一ページアプリケーションを開発できます。
+ **コアコンポーネント** 多数の新しいコンポーネント、a を追加します。 **コンポーネントライブラリ** と、既存のコアコンポーネントに対する様々な機能強化。
+ 詳細 **翻訳** 機能強化により、AEM Sitesの翻訳が合理化されます。

### 流動的なエクスペリエンス

AEMは、AEM外でのコンテンツの使用を容易にする新しい改善されたツールを使用して、流動的なエクスペリエンスを引き続き採用しています。

+ **コンテンツフラグメント** は、バージョン比較/差分と注釈をサポートしています。
+ **AEM Assets HTTP API** 露出を支持する **コンテンツフラグメント** DAM に **JSON**.
   **エクスペリエンスフラグメント** サポート **フルテキスト検索** および **AEM Dispatcher キャッシュの無効化** 参照用 **ページ**.

### アセット管理

AEM Assetsは、DAM の使用、管理、理解を向上させるために、豊富なアセット管理機能のセットを基に、引き続き構築しています。 AEM 6.5 では、Adobe Creative Cloudとクリエイティブワークフローの統合が引き続き改善されます。

+ **Adobeアセットリンク** は、クリエイティブをAdobe Creative CloudツールからAEM Assetsに直接接続します。
+ **Adobe Stock** 統合により、AEM AssetsエクスペリエンスからAdobe Stock画像に直接アクセスでき、シームレスなコンテンツ検出エクスペリエンスを作成できます。
+ **AEM Desktop App** では、バージョン 2.0 がリリースされ、パフォーマンスと安定性が向上し、バージョン 2.0 自体が再エンビジョンされました。
+ **Connected Assets** は、別のAEM Sitesインスタンスのアセットにシームレスにアクセスし、使用するために、個別のAEM Assetsインスタンスをサポートします。
+ のビデオサポートを更新しました **Dynamic Media**&#x200B;を含む **360 ビデオ** および **カスタムビデオサムネール**.

### コンテンツインテリジェンス

AEMは、機械学習と人工知能を活用して、すべてのエクスペリエンスを向上させ、スマートテクノロジーとの統合を継続的に構築しています。

+ **Adobeアセットリンク** 追加 **視覚的類似性検索**&#x200B;を使用すると、類似の画像を容易に見つけて内で使用することができます。 **Adobe Creative Cloudツール**.

### 統合

AEMは、他のAdobe サービスと統合する機能を拡大します。

+ **エクスペリエンスフラグメント** ～との統合を深める **Adobe Target** 支えて **JSON として書き出し** Adobe Targetと **エクスペリエンスフラグメントベースのオファーを削除** から **Adobe Target**.

### AMS Cloud Manager

[Cloud Manager](https://adobe.ly/2HODmsv)は、Adobe Managed Services(AMS) のお客様専用で、次の機能を提供します。

+ Cloud Manager は、AEM Sitesからに至るまで、AEMデプロイメントサポートを拡張します。 **AEM Assets**&#x200B;を含む **アセット処理の自動パフォーマンステスト**.
+ **自動スケーリング** 事前に定義されたしきい値で AEM パブリッシュ層を使用することで、エンドユーザーのエクスペリエンスを最適化できます。
+ **実稼動以外のパイプライン** 開発チームが Cloud Manager を活用して、コード品質を継続的に確認し、低レベルの環境（開発と QA）にデプロイできるようにします。
+ **CI/CD Pipeline API** を使用すれば、お客様は Cloud Manager をプログラムで操作し、オンプレミス開発インフラストラクチャとの統合の可能性を深めることができます。

## 基盤機能

以下に、AEMが提供する主な基盤機能の一覧を示します。 これらの機能の一部は、以前のバージョンで各リリースに追加された増分機能の強化で導入されました。

+ [AEM Foundation リリースノート](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***✔<sup>+</sup> このバージョンの機能が大幅に強化されました。***

***✔<sup>SP</sup> は、Service Pack または Feature Pack を介して機能が使用可能であることを示します。***

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
                <strong>Java 11 サポート：</strong> AEMは、Java 11（および Java 8）をサポートしています。
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
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Oak コンテンツリポジトリ</a>:</strong> Jackrabbit 2 よりも優れたパフォーマンスと拡張性を提供します。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">oak-run.jar のインデックスサポート</a>:</strong> Oak インデックスの再/インデックス作成、統計の収集、一貫性の確認を改善しました。</td>
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
                サーバーをダウンタイムなしでリポジトリのメンテナンスを実行する。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">TarMK または MongoMK リポジトリストレージ</a>:</strong>
                <br> TarMK（TarPM の次世代バージョン）のシンプルでパフォーマンスの高いファイルベースのストレージを使用するオプション
                <br> MongoMK を使用して、MongoDB ベースのリポジトリで水平方向にスケールすることもできます。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-with-mongodb.html" target="_blank">MongoMK のパフォーマンスと安定性</a>:</strong>
            AEM 6.0 の導入以降、MongoMK に対する継続的な機能強化がおこなわれています。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore">Amazon S3 DataStore</a>:</strong>
            拡張可能なクラウドストレージソリューションを活用して、バイナリアセットを保存します。</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>タッチ操作対応 UI の機能パリティ：</strong>
                クラシック UI と同等の機能と生産性の向上により、オーサリング UI が引き続き強化されました。</td>
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
                AEMをすばやく検索して移動します。</td>
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
 メンテナンスの実行、サーバーの正常性の監視、AEM内からのパフォーマンスの分析をおこないます。</td>
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
            アップグレードの改善により、AEMのインプレースアップグレードを容易かつ迅速に行うことができます。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/htl/using/overview.html" target="_blank">HTL テンプレート言語</a>:</strong>
            プレゼンテーションをロジックから分離する最新のテンプレートエンジン。 コンポーネントの開発時間が大幅に短縮されます。 各リリースで追加された増分機能。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">Sling モデル</a>:</strong>
            JCR リソースをビジネスオブジェクトやロジックにモデリングするための柔軟なフレームワークです。 各リリースで追加された増分機能。
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
                Adobe Managed Services(AMS) のお客様専用の Cloud Manager は、最新の CI/CD パイプラインの状態を通じて、開発とデプロイメントを加速します。</td>
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

以下に、AEMが提供する主なセキュリティ機能の一覧を示します。 これらの機能の一部は、以前のバージョンで各リリースに追加された増分機能の強化で導入されました。

+ [セキュリティリリースノート](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***✔は、このバージョンでの機能の大幅な強化を示しています。***

***✔<sup>+</sup> は、Service Pack または Feature Pack を介して機能が使用可能であることを示します。***

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
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">サービスユーザー</a></strong>
            <br> 権限を区分化して、管理者権限の不要な使用を避けます。</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">キーストア管理</a></strong>
            <br> グローバルトラストストア、証明書、およびキーはすべてリポジトリ内で管理されます。</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong>CSRF</strong> <strong>保護</strong></a>
            <br> クロスサイトリクエストフォージェリ保護をすぐに使用できます。</td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong>CORS</strong> <strong>サポート</strong></a>
            <br> クロスオリジンリソース共有では、アプリケーションの柔軟性を高めることができます。</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://experienceleague.adobe.com/docs/?lang=ja" target="_blank">SAML 認証のサポートの改善</a><br>
 </strong>SAML のリダイレクト、最適化されたグループ情報、および鍵の暗号化の問題が解決されました。
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
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">OSGi 設定としての LDAP</a><br>
 </strong>LDAP 認証の管理と更新を簡素化。</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong>プレーンテキストパスワードの OSGi 暗号化のサポート<br>
 </strong>パスワードやその他の機密値は、暗号化された形式で保存し、自動的に復号化できます。</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">CUG の機能強化</a><br>
 </strong>非公開ユーザーグループの実装が、パフォーマンスと拡張性の問題に対処するために書き直されました。</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔<sup>+</sup></td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">SSL ウィザード</a></strong>
            <br> SSL の設定と管理を簡単にする UI。</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">カプセル化されたトークンのサポート</a></strong>
            <br> 「スティッキー」セッションで、パブリッシュインスタンス間の水平方向の認証をサポートする必要がなくなりました。</td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">Adobe IMS認証のサポート</a><br>
 </strong>Adobe Managed Services(AMS) 専用で、Adobe IMS(Identity Management System) を介して AEM オーサーインスタンスへのアクセスを一元的に管理します。</td>
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

以下に、AEMが提供する主な Sites 機能の一覧を示します。 これらの機能の一部は、以前のバージョンで各リリースに追加された増分機能の強化で導入されました。

+ [AEM Sitesリリースノート](https://helpx.adobe.com/experience-manager/6-5/release-notes/sites.html)

***✔<sup>+</sup> このバージョンの機能が大幅に強化されました。***

***✔<sup>SP</sup> は、Service Pack または Feature Pack を介して機能が使用可能であることを示します。***

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
            編集者がタッチスクリーンを使用してタブレットやコンピューターを活用できるようにします。</td>
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
                レイアウトモードを使用すると、レスポンシブサイトのデバイスの幅に基づいて、編集者がコンポーネントのサイズを変更できます。</td>
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
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/core-components/user-guide.html" target="_blank">コアコンポーネント</a>:</strong>
            サイト開発を加速します。 GitHub では、頻繁なリリーススケジュールと柔軟性を実現できます。</td>
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
            React で構築されたシングルページアプリケーション (SPA) フレームワークを使用して、オーサリング可能で、Web エクスペリエンスを有効化する。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>スタイルシステム：</strong>
            コンテキスト内スタイルシステムを使用してAEMコンポーネントの外観を定義し、再利用を増やします。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">マルチサイトマネージャ (MSM)</a>:</strong>
            共通のコンテンツを共有する複数の Web サイト（複数言語、複数のブランド）を管理します。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">コンテンツの翻訳</a>:</strong>
            プラグアンドプレイフレームワークは、業界をリードするサードパーティの翻訳サービスと統合されています。</td>
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
            コンテンツのパーソナライゼーションのための次世代クライアントコンテキストフレームワーク。</td>
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
            日々のオーサリングを中断することなく、将来のリリース向けにコンテンツを開発できます。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>コンテンツフラグメント：</strong>
            編集用コンテンツを作成し、プレゼンテーションから分離してキュレーションし、再利用しやすくします。</td>
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
            デスクトップ、モバイルおよびソーシャルチャネル用に最適化された、再利用可能なエクスペリエンスとバリエーションを作成します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>コンテンツサービス：</strong>
            デバイスやアプリケーションをまたいで使用するために、AEMからコンテンツを JSON として書き出します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Adobe Analytics統合とコンテンツインサイト：</strong>
                Adobe Analyticsと DTM の統合が容易。 オーサー環境内でパフォーマンス情報を表示します。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/sites/authoring/using/content-targeting-touch.html" target="_blank">Adobe Target統合</a>:</strong>
            ターゲット設定されたエクスペリエンスを作成し、再利用可能なオファーライブラリを作成するためのステップバイステップのウィザード。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank">Adobe Campaign統合</a>:</strong>
            次世代の E メールキャンペーンソリューションとの統合が容易になります。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/using/aem_launch_adobeio_integration.html" target="_blank">Adobe起動の統合</a>:</strong>
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
            <td><strong>スクリーン：</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ecommerce.html" target="_blank">e コマース</a>:</strong>
            Web、モバイル、ソーシャルの各タッチポイントにまたがって、ブランド化されたパーソナライズされたショッピングエクスペリエンスを提供します。
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
            フォーラム、スレッドコメント、イベントカレンダー、その他の多くの機能を使用して、サイト訪問者との深いエンゲージメントを可能にします。</td>
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

## Assets の機能

以下に、AEMが提供する主な Assets 機能の一覧を示します。 これらの機能の一部は、以前のバージョンで各リリースに追加された増分機能の強化で導入されました。

+ [AEM Assetsリリースノート](https://helpx.adobe.com/experience-manager/6-5/release-notes/assets.html)

***✔は、このバージョンでの機能の大幅な強化を示しています。***

***✔<sup>+</sup> は、Service Pack または Feature Pack を介して機能が使用可能であることを示します。***

<table>
    <thead>
        <tr>
            <td>Assets 機能</td>
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
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">タッチ操作向け UI</a>:</strong>
            デスクトップコンピューターまたはタッチ操作対応デバイスでアセットを管理する。</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank">タスク</a> および <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects-with-workflows.html" target="_blank">ワークフロー</a> 管理：</strong>
            AEMプロジェクトを活用したデジタルアセットのレビューと承認のための事前定義済みのワークフローとタスク。</td>
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
            HTTP および JSON を介したプログラムによるアセットの操作。</td>
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
            ログインしなくてもデジタルアセットを簡単にアドホック共有できます。</td>
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
            デジタルアセットのシームレスな共有と配布を実現するクラウドサービス SAAS ソリューション。</td>
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
            Adobe Senseiを利用して、便利なメタデータで画像に自動的にタグ付けします。</td>
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
            AEM Assetsの検索時に検索語句を自動的に翻訳します。</td>
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
            商品カタログを生成します。 パンフレット、チラシ、印刷用広告を、広告テンプレートに基づいてInDesignします。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/desktop-app/aem-desktop-app.html" target="_blank">AEM Desktop App</a>:</strong>
            アセット製品での編集用に、アセットをローカルデスクトップにCreative Suiteします。
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank">Adobe画像ライブラリ</a>:</strong>
                <br> PhotoshopおよびAcrobatPDFライブラリ。高品質のファイル操作に使用されます。</td>
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
            Create Cloud アプリケーションからAEM Assetsに直接Adobeします。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank">Adobe Stock統合</a>:</strong>
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

### AEM Assets Dynamic Media

***✔<sup>+</sup> このバージョンの機能が大幅に強化されました。***

***✔<sup>SP</sup> は、Service Pack または Feature Pack を介して機能が使用可能であることを示します。***


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
            <td></td>
            <td></td>
            <td></td>
            <td></td>
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
            主要なオファーを紹介するクリック可能なコンテンツを含むインタラクティブバナーやビデオを作成します。
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
            <td><strong>セット (<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">画像</a>, <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">スピン</a>, <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank">混在メディア</a>):</strong>
            ユーザーが 360 度の視聴体験をズーム、パン、回転、シミュレーションできるようにします。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/" target="_blank">ビューア</a>:</strong>
            様々な画面やデバイスをサポートする、カスタムブランドのリッチメディアプレーヤーとプリセット。</td>
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
            HTTP/2 プロトコルを使用したDynamic Mediaコンテンツのリンクまたは埋め込みと配信に関する柔軟なオプション。</td>
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
            マスターアセットを移行し、既存の S7 URL を使用し続ける機能。</td>
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

AEMが提供する主なAEM Formsアドオン機能の一覧を以下に示します。 これらの機能の一部は、以前のバージョンで各リリースに追加された増分機能の強化で導入されました。

***✔<sup>+</sup> このバージョンの機能が大幅に強化されました。***

***✔<sup>SP</sup> は、Service Pack または Feature Pack を介して機能が使用可能であることを示します。***

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
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">アダプティブFormsエディタ</a>:</strong>
            デバイスとブラウザーの設定に基づいて、魅力的でレスポンシブなアダプティブフォームを作成します。</td>
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
            ドキュメントを作成して、データキャプチャエクスペリエンスや印刷可能なバージョンの長期保存を確保します。</td>
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
            再利用可能なテーマを作成して、フォームのコンポーネントやパネルのスタイルを設定します。</td>
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
            アダプティブフォームのベストプラクティスを標準化および実装します。</td>
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
            AEM Formsでは、パーソナライズされたインタラクティブな顧客通信を作成、管理および配信できます。
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
            データ統合を使用すると、フォーム内のユーザー入力に基づいて、異なるデータソースからデータを取得します。 この場合、フォームを送信すると、取得したデータが取得元のデータソースに再度書き込まれます。
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">Forms Processing のワークフロー（OSGi 上）</a>:</strong>
            フォーム承認プロセスのデプロイメントを簡略化しました。</td>
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
            分析、翻訳、A/B テスト、レビュー、公開の有効化など、すべてのフォーム/ドキュメント/通信を 1 か所で管理できます。
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/aem-forms-app.html" target="_blank">AEM Forms App</a>:</strong>
            iOS、Android、または Windows のアプリ内でオンライン/オフラインのフォーム処理を実行できるようにします。</td>
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
            グラフ（旧称：アダプティブドキュメント）などのインタラクティブ要素を持つターゲットステートメントなど、リッチなコミュニケーションを作成します。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Forms処理用のワークフロー (J2EE):</strong>
            直感的な IDE を使用して、複雑なフォームやドキュメント中心のワークフローを構築します。</td>
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
            安全にアクセスし、PDFおよび Office ドキュメントを認証します。
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
            アダプティブフォームのサポートとデバッグを行うには、Calvin フレームワークと Chrome プラグインを使用します。</td>
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

AEMが提供する主なAEM Communitiesアドオン機能の一覧を以下に示します。 これらの機能の一部は、以前のバージョンで各リリースに追加された増分機能の強化で導入されました。

***✔<sup>+</sup> このバージョンの機能が大幅に強化されました。***

***✔<sup>SP</sup> は、Service Pack または Feature Pack を介して機能が使用可能であることを示します。***

<table>
    <thead>
        <tr>
            <td> </td>
            <td>コミュニティ機能</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/forum.html" target="_blank">フォーラム</a>:</strong> （Social コンポーネントフレームワーク）新しいトピックを作成するか、既存のトピックを表示、フォロー、検索および移動します。</td>
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
                質問、表示、回答を行います。</p>
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
                公開側でブログ記事とコメントを作成します。
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
                （Social コンポーネントフレームワーク）サイト訪問者にコミュニティイベント情報を提供します。
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
            学習リソースを作成し、コミュニティメンバーに割り当てます。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">Enablement</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/catalog.html" target="_blank">カタログ</a> および <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">リソース管理</a>:</strong>
            カタログからイネーブルメントリソースにアクセスします。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#CreateaLearningPath" target="_blank">学習パス管理</a>:</strong>
            イネーブルメントリソースのコースまたはグループを管理します。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reports.html#main-pars_text_1739724213" target="_blank">使用可能なレポート</a>:</strong>
            イネーブルメントリソースと学習パスに関するレポート。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#main-pars_text_899882038" target="_blank">実施可能に関する取り組み</a>:</strong>
            イネーブルメントリソースにコメントを追加します。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">実施可能な分析</a>:</strong>
            ビデオ分析、進捗レポート、および割り当てレポート</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="8">コモンズ</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/comments.html" target="_blank">コメント</a> および添付ファイル：</strong>
            （ソーシャルコンポーネントのフレームワーク）コミュニティメンバーは、コミュニティサイト上のコンテンツに関する意見や知識を共有します。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>コンテンツフラグメントの変換：</strong>
            UGC の寄稿をコンテンツフラグメントに変換します。</td>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/rating.html" target="_blank">評価</a>:/strong&gt; (Social Component Framework) コミュニティメンバーがコンテンツの一部を評価する場合。</td>
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
            コンテンツをすばやく見つけるには、タグ（キーワードまたはラベル）をコンテンツと共に添付します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/search.html" target="_blank">検索</a>:</strong>
            予測検索と示唆的検索。</td>
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
            コミュニティ機能を使用してサイトを作成する。</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">テンプレート</a>:</strong>
                <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">サイト</a> および <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tools-groups.html" target="_blank">グループ</a> 完全に機能するコミュニティサイトをウィザードベースで作成するためのテンプレート。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>編集可能なテンプレート：</strong>
            コミュニティ管理者が、AEMの編集可能なテンプレートを使用して豊富なエクスペリエンスを構築できるようにします。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/creating-groups.html" target="_blank">グループまたはサブコミュニティ</a>:</strong>
            コミュニティサイト内にサブコミュニティを動的に作成します。
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderate-ugc.html#CommonModerationConcepts" target="_blank">スパム検出と不敬のフィルター</a>:</strong>
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
            メンバー管理領域でユーザープロファイルとグループを管理します。</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html#main-pars_text_866731966" target="_blank">レスポンシブデザイン</a>:</strong>
            AEM Communitiesのサイトはレスポンシブです。
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
            Adobe Analyticsと統合して、Communities のサイトの使用に関する主なインサイトを得ることができます。</td>
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
            (Adobe Senseiを活用した高度なスコアリング ) コミュニティメンバーをエキスパートとして特定し、報奨を与えます。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/activities.html" target="_blank">アクティビティ</a> および <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/notifications.html" target="_blank">通知</a>:</strong>
            最近のアクティビティストリームを表示し、関心のあるイベントについての通知を受け取ります。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/configure-messaging.html" target="_blank">メッセージ</a>:</strong>
            ユーザーおよびグループに対するダイレクトメッセージ。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/communities/using/social-login.html" target="_blank">ソーシャルログイン</a>:</strong>
            facebookまたはTwitterアカウントでログインする。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">プラットフォーム</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">MSRP （Mongo ストレージ）</a>:</strong>
            ユーザー生成コンテンツ (UGC) は、ローカルの MongoDB インスタンスに直接保持されます</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">DSRP （データベースストレージ）</a>:</strong>
            ユーザー生成コンテンツ (UGC) は、ローカルの MySQL データベースインスタンスに直接保持されます。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">SRP （クラウドストレージ）</a>:</strong>
                ユーザー生成コンテンツ (UGC) は、Adobeがホストおよび管理するクラウドサービスにリモートで保持されます。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank"><strong>JSRP</a>:</strong>
                コミュニティコンテンツは JCR に保存され、UGC は、投稿先のオーサー（またはパブリッシュ）インスタンスからアクセスできます。</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sync.html" target="_blank">ユーザーとグループの同期</a>:</strong>
            パブリッシュファームトポロジを使用する場合は、パブリッシュインスタンス間でユーザーとグループを同期します。</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

AEM Communitiesは、組織が以下の方法でユーザーをエンゲージし、ユーザーを有効にするためのリリースを強化しました。

+ **@mention** ユーザー生成コンテンツでのサポート。
+ を通じたアクセシビリティの改善 **キーボード操作** in **有効化** コンポーネント。
+ 改善済み **一括モデレート** using **カスタムフィルター**.
+ **編集可能なテンプレート** コミュニティ管理者がAEMで豊富なコミュニティエクスペリエンスを構築できるようにします。
+ ユーザーが **一括でのダイレクトメッセージ** をグループのすべてのメンバーに追加します。
