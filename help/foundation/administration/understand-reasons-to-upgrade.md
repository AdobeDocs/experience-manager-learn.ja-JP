---
title: アップグレードする理由について
description: Adobe Experience Manager 6 の最新バージョンへのアップグレードを検討しているお客様向けの主な機能の概要。
version: Experience Manager 6.5
topic: Upgrade
feature: Release Information
role: Leader, Developer, Admin, User
level: Beginner
doc-type: Article
exl-id: bf4030b0-67c4-4b00-af95-f63e6f79e995
duration: 538
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '2576'
ht-degree: 94%

---

# アップグレードする理由について

Adobe Experience Manager 6 の最新バージョンへのアップグレードを検討しているお客様向けの主な機能の概要。

## AEM 6.5 へのアップグレードの主な機能

+ [Adobe Experience Manager 6.5 リリースノート](https://helpx.adobe.com/jp/experience-manager/6-5/release-notes.html)

### 基盤の強化

Adobe Experience Manager 6.5 は、次によって、システムの安定性、パフォーマンス、サポート性を引き続き向上させています。

+ **Java 11**（Java 8 のサポートを維持しながら）をサポートします。

### Web サイトの作成と管理

AEM Sites では、web サイトの作成と構築を高速化するために設計された、次のような様々な機能が導入されています。

+ **SPA Editor** のサポートにより、SPA（単一ページアプリケーション）を AEM で完全にオーサリングし、マーケターに優しい豊富なオーサリングエクスペリエンスをサポートできます。
+_ **JavaScript SDK**、SPA Project Start Kit およびサポートするビルドツールを使用すると、フロントエンドデベロッパーは、AEM とは別に、SPA Editor と互換性のある単一ページアプリケーションを開発できます。
+ **コアコンポーネント**&#x200B;は既存のコアコンポーネントに対し、多数の新しいコンポーネント、**コンポーネントライブラリ**、様々な機能強化を追加します。
+ さらなる&#x200B;**翻訳**&#x200B;機能強化により、AEM Sites の翻訳が合理化されます。

### 流動的なエクスペリエンス

AEM は、AEM 外でのコンテンツの使用を容易にする新しい改善されたツールを使用して、流動的なエクスペリエンスを引き続き採用しています。

+ **コンテンツフラグメント**&#x200B;は、バージョン比較／差分と注釈をサポートしています。
+ **AEM の Assets HTTP API** は、**コンテンツフラグメント**&#x200B;を **JSON** として直接 DAM に公開することをサポートしています。
  **エクスペリエンスフラグメント**&#x200B;は、**ページ**&#x200B;参照のために&#x200B;**全文検索**&#x200B;および **AEM Dispatcher Cache Invalidation** をサポートします。

### アセット管理

AEM Assets は、DAM の使用、管理、理解を向上させるために、豊豊富なアセット管理機能のセットを引き続き構築します。 AEM 6.5 では、Adobe Creative Cloud とクリエイティブワークフローの統合が引き続き改善されます。

+ **Adobe アセットリンク**&#x200B;は、クリエイティブを Adobe Creative Cloud ツールから AEM Assets に直接接続します。
+ **Adobe Stock** 統合により、AEM Assets エクスペリエンスから Adobe Stock 画像に直接アクセスでき、シームレスなコンテンツ検出エクスペリエンスを作成できます。
+ **AEM デスクトップアプリ**&#x200B;ではバージョン 2.0 がリリースされ、パフォーマンスと安定性を向上しつつ、アプリ自体を刷新しました。
+ **Connected Assets** は、別の AEM Sites インスタンスのアセットにシームレスにアクセスし、使用するために、個別の AEM Assets インスタンスをサポートします。
+ **360 ビデオ**&#x200B;および&#x200B;**カスタムビデオサムネイル**&#x200B;を含む、**Dynamic Media** のビデオサポートを更新しました。

### コンテンツインテリジェンス

AEM は、あらゆるエクスペリエンスを向上させるため、機械学習と人工知能を活用して、スマートテクノロジーとの統合を継続的に構築しています。

+ **Adobe アセットリンク**&#x200B;には&#x200B;**ビジュアル検索または類似性検索**&#x200B;が追加され、同様の画像を簡単に見つけて **Adobe Creative Cloud ツール**&#x200B;内で使用できるようになります。

### 統合

AEM は、他の Adobe サービスと統合する機能を拡大します。

+ **エクスペリエンスフラグメント**&#x200B;は **Adobe Target** との統合を深め、Adobe Target への&#x200B;**JSON としての書き出し**&#x200B;および **Adobe Target**&#x200B;から&#x200B;**エクスペリエンスフラグメントベースのオファーを削除**&#x200B;する機能をサポートします。

### AMS Cloud Manager

Adobe Managed Services（AMS）ユーザー専用の [Cloud Manager](https://adobe.ly/2HODmsv) は、次の機能を提供します。

+ Cloud Manager のサポートは、 AEM デプロイメントを AEM Sites から **AEM Assets**（**アセット処理の自動パフォーマンステスト**&#x200B;を含む）へと拡張します。
+ 事前定義されたしきい値での AEM パブリッシュ層の&#x200B;**自動スケーリング**&#x200B;によって、最適なエンドユーザーエクスペリエンスを保証します。
+ **実稼動以外のパイプライン**&#x200B;を使用することで、開発チームは Cloud Manager を活用してコード品質を継続的に確認し、下位環境（開発および QA）にデプロイできます。
+ **CI/CD Pipeline API** を使用すれば、お客様は Cloud Manager をプログラムで操作し、オンプレミス開発インフラストラクチャとの統合の可能性を深めることができます。

## 基盤機能

以下に、AEM が提供する主な基盤機能のマトリックスを示します。 これらの機能の一部は以前のバージョンで導入されたもので、リリースごとに段階的に機能強化が追加されています。

+ [AEM Foundation リリースノート](https://helpx.adobe.com/jp/experience-manager/6-5/release-notes/wcm-platform.html)

***✔<sup>+</sup> このバージョンの機能が大幅に強化されました。***

***✔<sup>SP</sup> は、サービスパックまたは機能パックを介して機能が使用可能であることを示します。***

<table>
    <thead>
        <tr>
            <td>基盤機能</td>
            <td>5.6 x</td>
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
                <strong>Java 11 サポート：</strong>AEMは、Java 11（および Java 8）をサポートしています。
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
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Oak コンテンツリポジトリ</a>：</strong>前世代の Jackrabbit 2 よりも優れたパフォーマンスと拡張性を提供します。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/indexing-via-the-oak-run-jar.html?lang=ja"> oak-run.jar のインデックスサポート</a>：</strong>Oak インデックスの再／インデックス作成、統計の収集、一貫性の確認を改善しました。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html?lang=jp" target="_blank">カスタム検索インデックス</a>：</strong>
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
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">オンラインでのリビジョンクリーンアップ</a>：</strong>
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
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/deploying/introduction/storage-elements-in-aem-6.html?lang=ja" target="_blank">TarMK または MongoMK リポジトリストレージ</a>：</strong>
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
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/deploying/introduction/aem-with-mongodb.html?&lang=jp" target="_blank">MongoMK のパフォーマンスと安定性</a>：</strong>
AEM 6.0 の導入以降、MongoMK に対する継続的な機能強化が行われています。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore">Amazon S3 DataStore</a>：</strong>
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
AEM をすばやく検索して移動します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">操作ダッシュボード</a>：</strong>
 メンテナンスの実行、サーバーの正常性の監視、AEM 内からのパフォーマンスの分析を行います。</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">アップグレードの改善</a>：</strong>
アップグレードの改善により、AEM のインプレースアップグレードを容易かつ迅速に行うことができます。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/htl/using/overview.html" target="_blank">HTL テンプレート言語</a>：</strong>
 プレゼンテーションをロジックから分離する最新のテンプレートエンジン。 コンポーネントの開発時間が大幅に短縮されます。 各リリースで追加された機能。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">Sling モデル</a>：</strong>
JCR リソースをビジネスオブジェクトやロジックにモデリングするための柔軟なフレームワーク。 各リリースで追加された機能。
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
            <td><strong><a href="https://adobe.ly/2HODmsv" target="_blank">Cloud Manager</a>：</strong>
 Adobe Managed Services（AMS）のお客様専用の Cloud Manager は、最先端の CI／CD パイプラインを通じて開発とデプロイメントを促進します。</td>
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

以下に、AEM から提供される主なセキュリティ機能の一覧を示します。 これらの機能の一部は以前のバージョンで導入されたもので、リリースごとに段階的に機能強化が追加されています。

+ [セキュリティリリースノート](https://helpx.adobe.com/jp/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***✔は、このバージョンでの機能の大幅な強化を示しています。***

***✔<sup>+</sup> は、サービスパックまたは機能パックを介して機能が使用可能であることを示しています。***

<table>
    <thead>
        <tr>
            <td>セキュリティ機能</td>
            <td>5.6 x</td>
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
        <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html?lang=jp" target="_blank">サービスユーザー</a></strong>
 <br> 権限を区分化して、管理者権限の不要な使用を回避します。</td>
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
 <br> グローバルトラストストア、証明書およびキーがすべてリポジトリ内で管理されます。</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/csrf-protection.html?lang=jp" target="_blank"><strong>CSRF</strong> <strong>対策</strong></a>
 <br> クロスサイトリクエストフォージェリーに対する防御が標準で搭載されています。</td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/jp/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong>CORS</strong> <strong>サポート</strong></a>
 <br> クロスオリジンリソース共有のサポートで、アプリケーションの柔軟性を高めることができます。</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://experienceleague.adobe.com/docs/?lang=ja" target="_blank">SAML 認証サポートの改善</a><br>
 </strong>SAML リダイレクトの改善、グループ情報の最適化および鍵暗号化の問題の解決が行われました。
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
        <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/ldap-config.html?lang=ja" target="_blank">OSGi 設定としての LDAP</a><br>
 </strong>LDAP 認証の管理と更新を簡素化します。</td>
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
        <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/user-group-ac-admin.html?lang=ja" target="_blank">CUG の機能強化</a><br>
 </strong>非公開ユーザーグループの実装を書き直し、パフォーマンスと拡張性の問題を解決しました。</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔<sup>+</sup></td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/use-the-ssl-wizard.html?lang=ja" target="_blank">SSL ウィザード</a></strong>
 <br> SSL の設定と管理を簡素化する UI です。</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/encapsulated-token.html?lang=ja" target="_blank">カプセル化されたトークンのサポート</a></strong>
 <br> 「スティッキー」セッションで、パブリッシュインスタンス間の水平認証をサポートする必要がなくなりました。</td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">Adobe IMS 認証のサポート</a><br>
 </strong>Adobe Managed Services（AMS）専用で、Adobe IMS（Identity Management System）を介して AEM オーサーインスタンスへのアクセスを一元的に管理します。</td>
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

以下に、AEM が提供する主な Sites 機能の一覧を示します。 これらの機能の一部は以前のバージョンで導入されたもので、リリースごとに段階的に機能強化が追加されています。

+ [AEM Sites リリースノート](https://helpx.adobe.com/jp/experience-manager/6-5/release-notes/sites.html?lang=ja)

***✔<sup>+</sup> このバージョンの機能が大幅に強化されました。***

***✔<sup>SP</sup> は、サービスパックまたは機能パックを介して機能が使用可能であることを示します。***

<table>
    <thead>
        <tr>
            <td><strong>Sites 機能</strong></td>
            <td>5.6 x</td>
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
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/page-editor-feature-video-use.html?lang=ja" target="_blank">タッチ操作に最適化したページオーサリング</a>：</strong>
 編集者がタッチスクリーン搭載のタブレットやコンピューターを利用できるようになります。</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">レスポンシブサイトオーサリング</a>：</strong>
 レイアウトモードを使用すると、レスポンシブサイトのデバイス幅に基づいて編集者がコンポーネントのサイズを変更できます。</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html?lang=ja" target="_blank">編集可能なテンプレート</a>：</strong>
 専門の作成者がページテンプレートを作成および編集できるようになります。</td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/core-components/user-guide.html" target="_blank">コアコンポーネント</a>：</strong>
 サイト開発を促進します。 GitHub で入手でき、頻繁なリリーススケジュールと柔軟性の確保に対応できます。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">SPA エディター</a>：</strong>
 React に基づいて作成された単一ページアプリケーション（SPA）フレームワークを使用して、オーサリング可能で魅力的な web エクスペリエンスを作成します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>スタイルシステム：</strong>
 コンテキスト内スタイルシステムを使用して AEM コンポーネントの外観を定義することで、コンポーネントの再利用を促進します</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">マルチサイトマネージャ（MSM）</a>：</strong>
 共通のコンテンツを共有する複数の web サイト（複数言語、複数のブランドなど）を管理します。</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">コンテンツ翻訳</a>：</strong>
 プラグアンドプレイフレームワークは、業界をリードするサードパーティ翻訳サービスと統合されています。</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/developing/personlization/contexthub.html?lang=ja" target="_blank">ContextHub</a>：</strong>
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
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/launches/launches.html?lang=ja" target="_blank">ローンチ</a>：</strong>
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
再使用しやすくするため、プレゼンテーションから切り離した編集用コンテンツを作成および調整します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=ja" target="_blank">エクスペリエンスフラグメント</a>：</strong>
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
デバイスやアプリケーションをまたいで使用するために、AEM からコンテンツを JSON として書き出します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Adobe Analytics 統合とコンテンツインサイト：</strong>
Adobe Analytics と DTM の統合が容易になります。 オーサー環境内でパフォーマンス情報を表示します。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/personalization/content-targeting-touch.html?lang=jp" target="_blank">Adobe Target 統合</a>：</strong>
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
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/integration/campaign.html?lang=ja" target="_blank">Adobe Campaign 統合</a>：</strong>
次世代のメールキャンペーンソリューションとの統合が容易になります。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/using/aem_launch_adobeio_integration.html" target="_blank">Adobe Experience Platform のタグ統合</a>：</strong>
            アドビの次世代タグ管理クラウドサービスと統合します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Screens：</strong>
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
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/commerce/classic/administering/ecommerce.html?lang-jp" target="_blank">e コマース</a>：</strong>
Web、モバイル、ソーシャルという各種タッチポイントにまたがってブランド化を行い、パーソナライズされたショッピングエクスペリエンスを提供します。
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
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/communities/introduction/overview.html?lang=jp" target="_blank">コミュニティ</a>：</strong>
フォーラム、スレッドコメント、イベントカレンダーなどの多くの機能を使用して、サイト訪問者との深いエンゲージメントを可能にします。</td>
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

以下に、AEM が提供する主な Assets 機能の一覧を示します。 これらの機能の一部は以前のバージョンで導入されたもので、リリースごとに段階的に機能強化が追加されています。

+ [AEM Assets リリースノート](https://helpx.adobe.com/experience-manager/6-5/release-notes/assets.html?lang=ja)

***✔は、このバージョンでの機能の大幅な強化を示しています。***

***✔<sup>+</sup> は、サービスパックまたは機能パックを介して機能が使用可能であることを示しています。***

<table>
    <thead>
        <tr>
            <td>Assets 機能</td>
            <td>5.6 x</td>
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
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">タッチ操作向け UI</a>：</strong>
 デスクトップコンピューターまたはタッチ操作対応デバイスでのアセットを管理します。</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/using/metadata.html?lang=ja" target="_blank">高度なメタデータ管理</a>：</strong>
 メタデータテンプレート、メタデータスキーマエディターおよび一括メタデータ編集</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/projects/task-content.html?lang=ja" target="_blank">タスク</a>とワークフロー管理：</strong>
            AEM プロジェクトを活用したデジタルアセットのレビューと承認のための事前定義済みのワークフローとタスク。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>スケーラビリティとパフォーマンス：</strong>
 大規模な取り込み、アップロード、保存を対象とする強化されたサポート</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/extending/mac-api-assets.html?lang=ja" target="_blank">Assets HTTP API</a>：</strong>
HTTP および JSON を介したプログラムによるアセットの操作</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/administer/link-sharing.html?lang=ja" target="_blank">リンク共有</a>：</strong>
ログインの必要なしにデジタルアセットを簡単にアドホック共有</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/brand-portal/using/brand-portal.html" target="_blank">ブランドポータルl</a>：</strong>
デジタルアセットのシームレスな共有および配布のための Cloud Service SAAS ソリューション</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/using/use-assets-across-connected-assets-instances.html?lang=ja" target="_blank">Connected Assets</a>：</strong>
AEM Sites インスタンスは、別の AEM Assets インスタンスのアセットにシームレスにアクセスして使用可能</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/asset-insights.html?lang=jp" target="_blank">アセットインサイト</a>：</strong>
Adobe Analytics を活用して、デジタルアセットの顧客インタラクションをキャプチャし、AEM で表示します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/multilingual-assets.html?lang=jp" target="_blank">多言語アセット</a>：</strong>
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
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/assets/using/enhanced-smart-tags.html" target="_blank"> スマートタグとモデレート </a>:</strong>
            Adobe AI を活用して、便利なメタデータで画像を自動的にタグ付けします。</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html?lang=jp" target="_blank">スマート翻訳検索</a>：</strong>
AEM Assets の検索時に検索語句を自動的に翻訳します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/indesign.html?lang=jp" target="_blank">Adobe InDesign Server 統合</a>：</strong>
商品カタログを生成します。 InDesign テンプレートに基づいて、パンフレット、チラシ、印刷広告を作成します。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/desktop-app/aem-desktop-app.html" target="_blank">AEM デスクトップアプリ</a>：</strong>
Creative Suite 製品で編集するために、アセットをローカルデスクトップに同期します。
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
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/administer/imaging-transcoding-library.html?lang=jp" target="_blank">Adobe 画像ライブラリ</a>：</strong>
 <br>高品質のファイル操作に使用される Photoshop および Acrobat PDF ライブラリ。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">Adobe アセットリンク</a>：</strong>
Adobe Create Cloud アプリケーションから AEM Assets に直接アクセスします。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/using/aem-assets-adobe-stock.html?lang=jp" target="_blank">Adobe Stock統合</a>：</strong>
 AEM から直接 Adobe Stock 画像にシームレスにアクセスして使用します。</td>
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

***✔<sup>SP</sup> は、サービスパックまたは機能パックを介して機能が使用可能であることを示します。***


<table>
    <thead>
        <tr>
            <td>Dynamic Media 機能</td>
            <td>5.6 x</td>
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
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/managing-assets.html?lang=jp" target="_blank">イメージング</a>：</strong>
 スマート切り抜きを含め、様々なサイズおよび形式で画像を動的に配信します。</td>
            <td> </td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/assets/using/video-profiles.html" target="_blank">ビデオ</a>：</strong>
 高度なビデオエンコーディングとアダプティブビデオストリーミング</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/interactive-images.html?lang=jp" target="_blank">インタラクティブメディア</a>：</strong>
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
            <td><strong>セット（<a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/image-sets.html?lang=jp" target="_blank">画像</a>、<a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/spin-sets.html?lang=jp" target="_blank">スピン</a>、<a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/mixed-media-sets.html?lang=jp" target="_blank">混在メディア</a>）：</strong>
 ユーザーが 360 度の視聴エクスペリエンスをズーム、パン、回転、シミュレーションできるようにします。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/?lang=ja" target="_blank">ビューア</a>：</strong>
 様々な画面やデバイスをサポートする、カスタムブランドのリッチメディアプレーヤーとプリセット。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/delivering-dynamic-media-assets.html?lang=jp" target="_blank">配信</a>：</strong>
 HTTP/2 プロトコルを使用した Dynamic Media コンテンツのリンクまたは埋め込みと配信に関する柔軟なオプション。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Scene7 から Dynamic Media へのアップグレード：</strong>
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

## Forms 機能

AEM が提供する主な AEM Forms アドオン機能のマトリックスを以下に示します。 これらの機能の一部は以前のバージョンで導入されたもので、リリースごとに段階的に機能強化が追加されています。

***✔<sup>+</sup> このバージョンの機能が大幅に強化されました。***

***✔<sup>SP</sup> は、サービスパックまたは機能パックを介して機能が使用可能であることを示します。***

<table>
    <thead>
        <tr>
            <td>Forms 機能</td>
            <td>5.6 x</td>
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
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">アダプティブフォームエディター</a>：</strong>
 デバイスとブラウザーの設定に基づいて、魅力的でレスポンシブなアダプティブフォームを作成します。</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">レコードのドキュメント</a>：</strong>
 ドキュメントを作成して、データキャプチャエクスペリエンスや印刷可能なバージョンの長期保存を確保します。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/forms/using/themes.html" target="_blank">テーマエディター</a>：</strong>
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
            <td><strong><a href="https://helpx.adobe.com/jp/experience-manager/6-5/forms/using/template-editor.html" target="_blank">テンプレートエディター</a>：</strong>
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
            <td><strong><a href="https://helpx.adobe.com/jp/aem-forms/6-5/whats-new.html?lang=ja" target="_blank">Acrobat Sign 統合</a>：</strong>
 Acrobat Sign 統合フォームベースの署名シナリオのデプロイを許可します。</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/forms/letters-correspondences/cm-overview.html?lang=jp" target="_blank">通信監理</a>：</strong>
AEM Forms では、お客様とのパーソナル化されたインタラクティブな通信を作成、管理、提供できます。
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">サードパーティデータの統合</a>：</strong>
 データ統合を使用すると、フォーム内のユーザー入力に基づいて、異なるデータソースからデータが取得されます。 この場合、フォームを送信すると、取得したデータが取得元のデータソースに再度書き込まれます。
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
            <td><strong><a href="https://helpx.adobe.com/jp/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">Forms Processing のワークフロー（OSGi 上）</a>：</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/user-guide.html?topic=/experience-manager/6-5/forms/morehelp/integrations.ug.js" target="_blank">Marketing Cloud との統合</a>：</strong>
            Adobe Analytics および Adobe Target との統合により、顧客体験を強化および測定できます。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/forms/getting-started/introduction-managing-forms.html?lang=jp" target="_blank">フォームマネージャー</a>：</strong>
 分析、翻訳、A/B テスト、レビュー、公開の有効化など、すべてのフォーム／ドキュメント／通信を 1 か所で管理できます。
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/forms/aem-forms-app/aem-forms-app.html?lang=jp" target="_blank">AEM Forms アプリ</a>：</strong>
 iOS、Android、Windows のアプリ内でオンライン／オフラインのフォーム処理を実行できるようにします。</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/jp/aem-forms/6-5/introduction-forms-authoring.html" target="_blank">インタラクティブなコミュニケーション</a>：</strong>
グラフ（旧称：アダプティブドキュメント）などのインタラクティブ要素を持つターゲットステートメントなど、豊かなコミュニケーションを作成します。</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>フォーム処理用のワークフロー（J2EE）：</strong>
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
            <td><strong><a href="https://helpx.adobe.com/jp/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank">AEM Forms Document Security</a>：</strong>
 安全にアクセスし、PDF および Office ドキュメントを認証します。
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
            <td><strong><a href="https://helpx.adobe.com/jp/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">フレームワークのテスト</a>：</strong>
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

