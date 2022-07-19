---
title: コンテンツフラグメントとエクスペリエンスフラグメントについて
description: コンテンツフラグメントとエクスペリエンスフラグメントの類似点と相違点、および各タイプの使用方法とタイミングについて説明します。
sub-product: assets, sites, content services
feature: Content Fragments, Experience Fragments
topics: headless
version: 6.3, 6.4, 6.5
doc-type: article
activity: understand
audience: all
topic: Content Management
role: User
level: Beginner
exl-id: ccbc68d1-a83e-4092-9a49-53c56c14483e
source-git-commit: fb4a39a7b057ca39bc4cd4a7bce02216c3eb634c
workflow-type: tm+mt
source-wordcount: '1016'
ht-degree: 3%

---

# コンテンツフラグメントとエクスペリエンスフラグメントについて

Adobe Experience Managerのコンテンツフラグメントとエクスペリエンスフラグメントは、表面的には似ているように見えますが、それぞれが異なる使用例で主な役割を果たします。 コンテンツフラグメントとエクスペリエンスフラグメントが似ている点、異なる点、およびそれぞれの使用方法について説明します。

## コンテンツフラグメントとエクスペリエンスフラグメントの比較

<table>
<tbody><tr><td><strong> </strong></td>
<td><strong>コンテンツフラグメント（CF）</strong></td>
<td><strong>エクスペリエンスフラグメント (XF)</strong></td>
</tr><tr><td><strong>定義</strong></td>
<td><ul>
<li>再利用可能で、プレゼンテーションに依存しない <strong>コンテンツ</strong>( 構造化されたデータ要素（テキスト、日付、参照など）で構成される )</li>
</ul>
</td>
<td><ul>
<li>コンテンツとプレゼンテーションを定義し、次の要素を構成する 1 つ以上のAEMコンポーネントの再利用可能な複合 <strong>エクスペリエンス</strong> それ自体が理にかなっている</li>
</ul>
</td>
</tr><tr><td><strong>コアテナント</strong></td>
<td><ul>
<li>コンテンツ中心の</li>
<li>定義元 <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">構造化されたフォームベースのデータモデルです。</a></li>
<li>デザインとレイアウトに依存しません。</li>
<li>チャネルは、コンテンツフラグメントのコンテンツ（レイアウトとデザイン）のプレゼンテーションを所有しています</li>
</ul>
</td>
<td><ul>
<li>プレゼンテーション中心</li>
<li>AEM Components の非構造化構成によって定義</li>
<li>コンテンツのデザインとレイアウトを定義</li>
<li>チャネルで「現状のまま」使用されます</li>
</ul>
</td>
</tr><tr><td><strong>技術的な詳細</strong></td>
<td><ul>
<li>a として実装 <strong>dam:Asset</strong></li>
<li>定義元 <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">コンテンツフラグメントモデル</a></li>
</ul>
</td>
<td><ul>
<li>a として実装 <strong>cq:Page</strong></li>
<li>編集可能なテンプレートで定義</li>
<li>ネイティブHTMLレンディション</li>
</ul>
</td>
</tr><tr><td><strong>バリエーション</strong></td>
<td><ul>
<li>マスターのバリエーションは標準的なバリエーションです</li>
<li>バリエーションは、チャネルに合わせて調整できる使用例に固有です。</li>
</ul>
</td>
<td><ul>
<li>バリエーションは、チャネルまたはコンテキストに固有です。</li>
<li>バリエーションは、AEM Live Copy を使用して同期を維持します</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html" target="_blank">構築ブロック</a> バリエーション間でコンテンツを再利用できるようにする</li>
</ul>
</td>
</tr><tr><td><strong>機能</strong></td>
<td><ul>
<li>バリエーション</li>
<li>バージョン</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#synchronizing-with-master" target="_blank">同期</a> バリエーションをまたいだコンテンツ</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-managing.html?lang=en#comparing-fragment-versions" target="_blank">視覚的差分</a> /コンテンツフラグメントバージョン</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#annotating-a-content-fragment" target="_blank">注釈</a> 複数行テキスト要素の</li>
<li>インテリジェント <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#summarizing-text" target="_blank">要約</a> 複数行テキスト要素の</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/creating-translation-projects-for-content-fragments.html?lang=en" target="_blank">翻訳/ローカライゼーション</a></li>
</ul>
</td>
<td><ul>
<li>バリエーション</li>
<li>バリエーションをライブコピーとして</li>
<li>バージョン</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en#building-blocks" target="_blank">構築ブロック</a></li>
<li>注釈</li>
<li>レスポンシブレイアウトとプレビュー</li>
<li>翻訳/ローカライゼーション</li>
</ul>
</td>
</tr><tr><td><strong>使用方法</strong></td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html" target="_blank">AEMコアコンポーネントコンテンツフラグメントコンポーネント</a> AEM Sites、AEM Screens、またはエクスペリエンスフラグメントで使用します。</li>
<li>を介した JSON エクスポート <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en" target="_blank">AEM Content Services</a> （サードパーティでの使用）</li>
<li>AEM HTTP Assets API を介したサードパーティでの使用のための JSON。</li>
</ul>
</td>
<td><ul>
<li>AEM Sites、AEM Screens、またはその他のエクスペリエンスフラグメントで使用するAEMエクスペリエンスフラグメントコンポーネント。</li>
<li>書き出し形式 <a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">プレーンHTML</a> （サードパーティ製システムで使用）</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/integration/experience-fragments-target.html?lang=en" target="_blank">Adobe TargetへのHTMLエクスポート</a> ターゲットオファー</li>
<li>ターゲットオファー用のAdobe Targetへの JSON 書き出し</li>
</ul>
</td>
</tr><tr><td><strong>一般的な使用例</strong></td>
<td><ul>
<li>高度に構造化されたデータ入力/フォームベースのコンテンツ</li>
<li>長形式の編集コンテンツ（複数行要素）</li>
<li>コンテンツを配信するチャネルのライフサイクル外で管理されるコンテンツ</li>
</ul>
</td>
<td><ul>
<li>チャネルごとのバリエーションを使用して、マルチチャネルのプロモーション資料を一元管理します。</li>
<li>コンテンツは、Web サイトの複数のページで再利用されます。</li>
<li>Web サイトクロム ( 例： ヘッダーとフッター )</li>
<li>エクスペリエンスを配信するチャネルのライフサイクル外で管理されるエクスペリエンス</li>
</ul>
</td>
</tr><tr><td><strong>ドキュメント化</strong></td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">AEMコンテンツフラグメントユーザーガイド</a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en" target="_blank">AEMでのコンテンツフラグメントの使用</a></li>
</ul>
</td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">エクスペリエンスフラグメントに関するAdobeドキュメント</a></li>
</ul>
</td>
</tr></tbody></table>

## コンテンツフラグメントのアーキテクチャ

次の図は、AEMコンテンツフラグメントの全体的なアーキテクチャを示しています

! ![コンテンツフラグメントのアーキテクチャ](./assets/content-fragments-architecture.png)

+ **コンテンツフラグメントモデル** コンテンツフラグメントが取得して公開するコンテンツを定義する要素（またはフィールド）を定義します。
+ この **コンテンツフラグメント** は、論理コンテンツエンティティを表すコンテンツフラグメントモデルのインスタンスです。
+ コンテンツフラグメント **バリエーション** ただし、コンテンツフラグメントモデルにはコンテンツのバリエーションがあります。
+ コンテンツフラグメントは、次の方法で公開/使用できます。
   + でのコンテンツフラグメントの使用 **AEM Sites** ( またはAEM Screens) をAEM WCM コアコンポーネントのコンテンツフラグメントコンポーネントから使用する。
   + コンテンツフラグメントの埋め込み **エクスペリエンスフラグメント** AEM WCM コアコンポーネントのコンテンツフラグメントコンポーネントを使用（エクスペリエンスフラグメントの使用例に使用）。
   + コンテンツフラグメントバリエーションコンテンツを JSON 形式で公開（経由） **AEM Content Services** および API ページ（読み取り専用の使用例）
   + 直接、 **AEM Assets HTTP API** を参照してください。

## エクスペリエンスフラグメントのアーキテクチャ

! ![エクスペリエンスフラグメントのアーキテクチャ](./assets/experience-fragments-architecture.png)

+ **編集可能なテンプレート**&#x200B;を呼び出し、それらを次のように定義します。 **編集可能なテンプレートタイプ** および **AEMページコンポーネントの実装**&#x200B;では、エクスペリエンスフラグメントの作成に使用できる許可されたAEMコンポーネントを定義します。
+ この **エクスペリエンスフラグメント** は、論理的なエクスペリエンスを表す編集可能なテンプレートのインスタンスです。
+ エクスペリエンスフラグメント **バリエーション** ただし、編集可能テンプレートに準拠すると、エクスペリエンス（コンテンツとデザイン）にバリエーションがあります。
+ エクスペリエンスフラグメントは、次の方法で公開/使用できます。
   + AEMエクスペリエンスフラグメントコンポーネントを使用して、AEM Sites( またはAEM Screens) 上でエクスペリエンスフラグメントを使用する。
   + エクスペリエンスフラグメントのバリエーションコンテンツを JSON( 埋め込みHTML付き ) としてで公開する ( **AEM Content Services** と API ページを参照してください。
   + エクスペリエンスフラグメントのバリエーションを次の形式で直接公開 **&quot;プレーンHTML&quot;**.
   + エクスペリエンスフラグメントのへの書き出し **Adobe Target** をHTMLまたは JSON オファーとして使用する。
   + AEM SitesはHTMLオファーをネイティブでサポートしていますが、JSON オファーにはカスタム開発が必要です。

## コンテンツフラグメントのサポート資料

+ [コンテンツフラグメントユーザーガイド](https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [AEMでのコンテンツフラグメントの使用](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en)
+ [AEM WCM コアコンポーネントのコンテンツフラグメントコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=ja)
+ [コンテンツフラグメントとAEMヘッドレスの使用](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html?lang=ja)
+ [AEM Content Services の概要](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en)

## エクスペリエンスフラグメントのサポート資料

+ [エクスペリエンスフラグメントに関するAdobeドキュメント](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en)
+ [AEMエクスペリエンスフラグメントについて](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [AEM Experience Fragments の使用](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [Adobe TargetでのAEM Experience Fragments の使用](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
