---
title: コンテンツフラグメントとエクスペリエンスフラグメントについて
description: Adobe Experience Managerのコンテンツフラグメントとエクスペリエンスフラグメントは、表面的に似ているように見える場合がありますが、それぞれが異なる用途で重要な役割を果たします。 コンテンツフラグメントとエクスペリエンスフラグメントの類似性、違い、それぞれの用途、使用方法を説明します。
sub-product: アセット、サイト、コンテンツサービス
feature: Content Fragments, Experience Fragments
topics: headless
version: 6.3, 6.4, 6.5
doc-type: article
activity: understand
audience: all
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1004'
ht-degree: 7%

---


# コンテンツフラグメントとエクスペリエンスフラグメントについて

Adobe Experience Managerのコンテンツフラグメントとエクスペリエンスフラグメントは、表面的に似ているように見える場合がありますが、それぞれが異なる用途で重要な役割を果たします。 コンテンツフラグメントとエクスペリエンスフラグメントの類似性、違い、それぞれの用途、使用方法を説明します。

## コンテンツフラグメントとエクスペリエンスフラグメントの比較

<table>
<tbody><tr><td><strong> </strong></td>
<td><strong>コンテンツフラグメント（CF）</strong></td>
<td><strong>エクスペリエンスフラグメント(XF)</strong></td>
</tr><tr><td><strong>定義</strong></td>
<td><ul>
<li>再利用可能で、プレゼンテーションにとらわれない<strong>コンテンツ</strong>。構造化されたデータ要素（テキスト、日付、参照など）で構成されます。</li>
</ul>
</td>
<td><ul>
<li>独自に意味を持つ<strong>エクスペリエンス</strong>を形成するコンテンツと表示を定義する1つ以上のAEMコンポーネントの再利用可能な複合体</li>
</ul>
</td>
</tr><tr><td><strong>コアテナント</strong></td>
<td><ul>
<li>コンテンツ中心</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-models.html" target="_blank">構造化されたフォームベースのデータモデルによって定義されます。</a></li>
<li>デザインとレイアウトにとらわれません。</li>
<li>チャネルは、コンテンツフラグメントのコンテンツ（レイアウトとデザイン）のプレゼンテーションを所有しています</li>
</ul>
</td>
<td><ul>
<li>プレゼンテーション中心</li>
<li>AEMコンポーネントの非構造化構成で定義</li>
<li>コンテンツのデザインとレイアウトを定義します</li>
<li>チャネルで「現状のまま」使用</li>
</ul>
</td>
</tr><tr><td><strong>技術的詳細</strong></td>
<td><ul>
<li><strong>dam:Asset</strong>として実装</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-models.html" target="_blank">コンテンツフラグメントモデル</a>で定義</li>
</ul>
</td>
<td><ul>
<li><strong>cq:Page</strong>として実装</li>
<li>編集可能なテンプレートで定義</li>
<li>ネイティブHTMLレンダリング</li>
</ul>
</td>
</tr><tr><td><strong>バリエーション</strong></td>
<td><ul>
<li>マスターのバリエーションは、標準のバリエーションです</li>
<li>バリエーションは、使用事例によって異なり、チャネルに合わせて調整される場合があります。</li>
</ul>
</td>
<td><ul>
<li>バリエーションは、チャネルまたはコンテキストに固有です。</li>
<li>バリエーションはAEM Live Copyを使用して同期されます。</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#BuildingBlocks" target="_blank">バリエーションを超えて</a> コンテンツを再利用できるように構築</li>
</ul>
</td>
</tr><tr><td><strong>機能</strong></td>
<td><ul>
<li>バリエーション</li>
<li>バージョン</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#SynchronizingwithMaster" target="_blank">バリエーション間のコンテンツの</a> 同期</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-managing.html#ComparingFragmentVersions" target="_blank">Visual </a> Diff Content Fragment versions</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#AnnotatingaContentFragment" target="_blank">複数行のテキスト要素</a> の注釈</li>
<li>複数行のテキスト要素のインテリジェントな<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/content-fragments-variations.html#SummarizingText" target="_blank">要約</a>。</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/creating-translation-projects-for-content-fragments.html" target="_blank">翻訳/ローカライゼーション</a></li>
</ul>
</td>
<td><ul>
<li>バリエーション</li>
<li>ライブコピーとしてのバリエーション</li>
<li>バージョン</li>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#BuildingBlocks" target="_blank">構築ブロック</a></li>
<li>注釈</li>
<li>レスポンシブなレイアウトとプレビュー</li>
<li>翻訳/ローカライゼーション</li>
</ul>
</td>
</tr><tr><td><strong>以下のように</strong></td>
<td><ul>
<li><a href="https://docs.adobe.com/content/help/en/experience-manager-core-components/using/components/content-fragment-component.html" target="_blank">AEM Sites、AEM Screens、またはエクスペリエンスフラグメントで使用するAEMコアコンポーネントコンテンツフラグメント</a> コンポーネント。</li>
<li><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-services-tutorial-use.html" target="_blank">AEM Content Services</a>を介したJSONエクスポート。サードパーティの利用が可能</li>
<li>AEM HTTP Assets APIを介したJSONによるサードパーティの利用。</li>
</ul>
</td>
<td><ul>
<li>AEM Sites、AEM Screensまたはその他のエクスペリエンスフラグメントで使用するAEMエクスペリエンスフラグメントコンポーネント。</li>
<li>サードパーティ製システムで使用する<a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html#ThePlainHTMLRendition" target="_blank">プレーンHTML</a>として書き出し</li>
<li><a href="https://helpx.adobe.com/jp/experience-manager/6-5/sites/administering/using/experience-fragments-target.html" target="_blank">ターゲットオファー用のHTMLのAdobe</a> ターゲットへの書き出し</li>
<li>ターゲットオファー用のJSONエクスポートをAdobe Targetに送信</li>
</ul>
</td>
</tr><tr><td><strong>一般的な使用例</strong></td>
<td><ul>
<li>高度に構造化されたデータ入力/フォームベースのコンテンツ</li>
<li>長形式の編集コンテンツ（複数行要素）</li>
<li>配信するチャネルのライフサイクル外で管理されるコンテンツ</li>
</ul>
</td>
<td><ul>
<li>チャネルごとのバリエーションを使用した、複数チャネルの販促資料の一元管理。</li>
<li>コンテンツがウェブサイトの複数のページで再利用される。</li>
<li>Webサイトクロム( ヘッダーとフッター)</li>
<li>チャネルのライフサイクル外で管理されるエクスペリエンス。</li>
</ul>
</td>
</tr><tr><td><strong>ドキュメント</strong></td>
<td><ul>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/assets/user-guide.html?topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">AEMコンテンツフラグメントユーザーガイド</a></li>
<li><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-use.html" target="_blank">AEMでのコンテンツフラグメントの使用</a></li>
</ul>
</td>
<td><ul>
<li><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html" target="_blank">エクスペリエンスフラグメントに関するAdobeドキュメント</a></li>
</ul>
</td>
</tr></tbody></table>

## コンテンツフラグメントのアーキテクチャ

次の図に、AEMコンテンツフラグメントの全体的なアーキテクチャを示します

!![コンテンツフラグメントのアーキテクチャ](./assets/content-fragments-architecture.png)

+ **コンテンツフラグメントモデル** は、コンテンツフラグメントが取り込んで公開するコンテンツを定義する要素（またはフィールド）を定義します。
+ **コンテンツフラグメント**&#x200B;は、論理コンテンツエンティティを表すコンテンツフラグメントモデルのインスタンスです。
+ ただし、コンテンツフラグメント&#x200B;**バリエーション**&#x200B;は、コンテンツフラグメントモデルに従います。
+ コンテンツフラグメントは次の方法で公開/使用できます。
   + AEM WCMコアコンポーネントのコンテンツフラグメントコンポーネントを介して、**AEM Sites**(またはAEM Screens)上のコンテンツフラグメントを使用する。
   + AEM WCMコアコンポーネントのコンテンツフラグメントコンポーネントを介して、コンテンツフラグメントを&#x200B;**エクスペリエンスフラグメント**&#x200B;に埋め込み、エクスペリエンスフラグメントの使用例に使用します。
   + **AEM Content Services**&#x200B;およびAPIページを介した、コンテンツフラグメントのバリエーションコンテンツのJSONとしての公開（読み取り専用の使用例）。
   + CRUDの使用例に対して、**AEM AssetsHTTP API**&#x200B;経由でAEM Assetsに直接呼び出して、コンテンツフラグメントコンテンツ（すべてのバリエーション）をJSONとして直接公開します。

## エクスペリエンスフラグメントのアーキテクチャ

!![エクスペリエンスフラグメントのアーキテクチャ](./assets/experience-fragments-architecture.png)

+ **編集可能なテンプレート**(これは、 **編集可能なテンプレート** タイプと **AEMページコンポーネントの実装で定義されます**)は、エクスペリエンスフラグメントの作成に使用できる許可されたAEMコンポーネントを定義します。
+ **エクスペリエンスフラグメント**&#x200B;は、論理エクスペリエンスを表す編集可能なテンプレートのインスタンスです。
+ エクスペリエンスフラグメント&#x200B;**バリエーション**&#x200B;は編集可能なテンプレートに従いますが、エクスペリエンス（コンテンツとデザイン）のバリエーションがあります。
+ エクスペリエンスフラグメントは、次の方法で公開/消費できます。
   + AEM Experience Fragmentコンポーネントを使用した、AEM Sites(またはAEM Screens)でのエクスペリエンスフラグメントの使用。
   + エクスペリエンスフラグメントのバリエーションコンテンツをJSON（埋め込みHTMLを含む）として&#x200B;**AEM Content Services**&#x200B;およびAPIページで公開します。
   + エクスペリエンスフラグメントのバリエーションを&#x200B;**&quot;Plain HTML&quot;**&#x200B;として直接公開します。
   + エクスペリエンスフラグメントを&#x200B;**Adobe Target**&#x200B;にHTMLまたはJSONオファーとして書き出します。
   + AEM SitesはHTMLオファーをネイティブでサポートしますが、JSONオファーはカスタム開発が必要です。

## コンテンツフラグメントのサポート資料

+ [コンテンツフラグメントユーザーガイド](https://helpx.adobe.com/experience-manager/6-5/assets/user-guide.html?topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [AEMでのコンテンツフラグメントの使用](https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-use.html)
+ [AEM WCMコアコンポーネントのコンテンツフラグメントコンポーネント](https://docs.adobe.com/content/help/ja-JP/experience-manager-core-components/using/components/content-fragment-component.html)
+ [コンテンツフラグメントとAEM Content Servicesの使用](https://helpx.adobe.com/experience-manager/kt/sites/using/structured-fragments-content-services-feature-video-use.html)
+ [AEM Content Services使用の手引き](https://helpx.adobe.com/jp/experience-manager/kt/sites/using/content-services-tutorial-use.html)

## エクスペリエンスフラグメントのサポート資料

+ [エクスペリエンスフラグメントに関するAdobeドキュメント](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/experience-fragments.html)
+ [AEMエクスペリエンスフラグメントについて](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-understand.html)
+ [AEMエクスペリエンスフラグメントの使用](https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html)
+ [Adobe TargetでのAEMエクスペリエンスフラグメントの使用](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
