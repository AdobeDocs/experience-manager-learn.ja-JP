---
title: コンテンツフラグメントとエクスペリエンスフラグメント
description: コンテンツフラグメントとエクスペリエンスフラグメントの類似点と相違点、および各タイプを使用するタイミングと方法について学びます。
sub-product: Experience Manager Assets, Experience Manager Sites
feature: Content Fragments, Experience Fragments
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
role: User
level: Beginner
doc-type: Article
exl-id: ccbc68d1-a83e-4092-9a49-53c56c14483e
duration: 168
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '829'
ht-degree: 100%

---

# コンテンツフラグメントとエクスペリエンスフラグメント

Adobe Experience Manager のコンテンツフラグメントとエクスペリエンスフラグメントは、表面的には似ているように見えるかもしれませんが、それぞれが異なるユース ケースで重要な役割を果たします。コンテンツフラグメントとエクスペリエンスフラグメント類似点と相違点、そしてそれぞれのタイプの使用方法とタイミングについて学びます。

## 比較

<table>
<tbody><tr><td><strong> </strong></td>
<td><strong>コンテンツフラグメント（CF）</strong></td>
<td><strong>エクスペリエンスフラグメント（XF）</strong></td>
</tr><tr><td><strong>定義</strong></td>
<td><ul>
<li>構造化されたデータ要素（テキスト、日付、参照など）で構成される、再利用可能な、プレゼンテーションに依存しない<strong>コンテンツ</strong></li>
</ul>
</td>
<td><ul>
<li><strong>エクスペリエンス</strong> を形成するコンテンツとプレゼンテーションを定義する 1 つ以上の AEM コンポーネントの再利用可能な複合物で、それ自体が理にかなっているもの</li>
</ul>
</td>
</tr><tr><td><strong>主要な考え方</strong></td>
<td><ul>
<li>コンテンツ中心</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/content-fragments/content-fragments-models.html?lang=ja" target="_blank">構造化されたフォームベースのデータ モデル</a>によって定義されます。</li>
<li>デザインとレイアウトに依存しません。</li>
<li>チャネルには、コンテンツフラグメントのコンテンツ（レイアウトとデザイン）のプレゼンテーションがあります</li>
</ul>
</td>
<td><ul>
<li>プレゼンテーション中心</li>
<li>AEM コンポーネントの構造化されていない成分によって定義されています</li>
<li>コンテンツのデザインとレイアウトを定義します</li>
<li>チャネルで「そのまま」使用されます</li>
</ul>
</td>
</tr><tr><td><strong>技術的詳細</strong></td>
<td><ul>
<li><strong>dam:Asset</strong> として実装されます</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/content-fragments/content-fragments-models.html?lang=ja" target="_blank">コンテンツフラグメントモデル</a>によって定義されます</li>
</ul>
</td>
<td><ul>
<li><strong>cq:Page</strong> として実装されます</li>
<li>編集可能なテンプレートによって定義されます</li>
<li>ネイティブ HTML レンディション</li>
</ul>
</td>
</tr><tr><td><strong>バリエーション</strong></td>
<td><ul>
<li>プライマリバリエーションは標準的なバリエーションです</li>
<li>バリエーションはユースケース固有のものであり、チャネルに合わせて調整される場合があります。</li>
</ul>
</td>
<td><ul>
<li>バリエーションは、チャネルやコンテキストに固有です</li>
<li>バリエーションは、AEM ライブコピーを使用して同期が維持されます</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=ja" target="_blank">構築ブロック</a> により、様々なバリエーションでコンテンツを再利用することができます</li>
</ul>
</td>
</tr><tr><td><strong>機能</strong></td>
<td><ul>
<li>バリエーション</li>
<li>バージョン</li>
<li>バリエーション間のコンテンツの<a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=ja#synchronizing-with-master" target="_blank">同期化</a></li>
<li>コンテンツフラグメントバージョンの<a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/content-fragments/content-fragments-managing.html?lang=ja#comparing-fragment-versions" target="_blank">視覚的差分</a></li>
<li>複数行テキスト要素の<a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/content-fragments/content-fragments-variations.html?lang=ja#annotating-a-content-fragment" target="_blank">注釈</a></li>
<li>複数行のテキスト要素のインテリジェントな<a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/content-fragments/content-fragments-variations.html?lang=ja#summarizing-text" target="_blank">要約</a>。</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/content-fragments/creating-translation-projects-for-content-fragments.html?lang=ja" target="_blank">翻訳／ローカライゼーション</a></li>
</ul>
</td>
<td><ul>
<li>バリエーション</li>
<li>ライブコピーとしてのバリエーション</li>
<li>バージョン</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=ja#building-blocks" target="_blank">構築ブロック</a></li>
<li>注釈</li>
<li>レスポンシブレイアウトとプレビュー</li>
<li>翻訳／ローカライゼーション</li>
<li>コンテンツフラグメント参照を介した複雑なデータモデル</li>
<li>アプリ内プレビュー</li>
</ul>
</td>
</tr><tr><td><strong>使用方法</strong></td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/landing/experience-manager/headless/developer.html?lang=ja">AEM ヘッドレス GraphQL API</a> を介した JSON エクスポート</li>
<li>AEM Sites、AEM Screens、またはエクスペリエンスフラグメントで使用できる <a href="https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=ja" target="_blank">AEM コアコンポーネントコンテンツフラグメントコンポーネント。</a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=ja" target="_blank">AEM Content Services</a> を介した JSON の書き出し（サードパーティでの使用）</li>
<li>ターゲットオファー向けに JSON を Adobe Target に書き出す</li>
<li>AEM HTTP Assets API を介したサードパーティによる JSON の利用</li>
</ul>
</td>
<td><ul>
<li>AEM Sites、AEM Screens、またはその他のエクスペリエンスフラグメントで使用する AEM エクスペリエンスフラグメントコンポーネント。</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=ja" target="_blank">プレーンHTML</a> として書き出し（サードパーティ製システムで使用）</li>
<li>ターゲットオファー向けに <a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/integration/experience-fragments-target.html?lang=ja" target="_blank">HTML を Adobe Target に書き出す</a></li>
<li>ターゲットオファー向けに JSON を Adobe Target に書き出す</li>
</ul>
</td>
</tr><tr><td><strong>一般的なユースケース</strong></td>
<td><ul>
<li>GraphQL を使用したヘッドレスユースケースの強化</li>
<li>構造化されたデータ入力またはフォームベースのコンテンツ</li>
<li>ロングフォームの編集コンテンツ（複数行の要素）</li>
<li>コンテンツを配信するチャネルのライフサイクル外で管理されるコンテンツ</li>
</ul>
</td>
<td><ul>
<li>チャネルごとのバリエーションを使用して、マルチチャネルのプロモーション資料を一元管理します。</li>
<li>コンテンツは、web サイトの複数のページで再利用されます。</li>
<li>Web サイトクロム（例： ヘッダーとフッター）</li>
<li>エクスペリエンスを配信するチャネルのライフサイクル外で管理されるエクスペリエンス</li>
</ul>
</td>
</tr><tr><td><strong>ドキュメント化</strong></td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=ja&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">AEM コンテンツフラグメントユーザーガイド</a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=ja" target="_blank">AEM でのコンテンツフラグメントの使用</a></li>
</ul>
</td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=ja" target="_blank">エクスペリエンスフラグメントに関する Adobe ドキュメント</a></li>
</ul>
</td>
</tr></tbody></table>

## コンテンツフラグメントのアーキテクチャ

次の図は、AEM コンテンツフラグメントの全体的なアーキテクチャを示しています

![コンテンツフラグメントのアーキテクチャ](./assets/content-fragments-architecture.png)

+ **コンテンツフラグメントモデル**&#x200B;は、コンテンツフラグメントが取得して公開するコンテンツを定義する要素（またはフィールド）を定義します。
+ **コンテンツフラグメント** は、論理コンテンツエンティティを表すコンテンツフラグメントモデルのインスタンスです。
+ コンテンツフラグメントの&#x200B;**バリエーション**&#x200B;はコンテンツフラグメントモデルに従いますが、コンテンツにはバリエーションがあります。
+ コンテンツフラグメントは、次の方法で公開/使用できます。
   + AEM WCM コアコンポーネントのコンテンツフラグメントコンポーネントから **AEM Sites**（または AEM Screens）でコンテンツフラグメントを使用する。
   + AEM ヘッドレス GraphQL API を使用して、ヘッドレスアプリから **コンテンツフラグメント** を使用します。
   + 読み取り専用のユースケース向けに、**AEM Content Services** および API ページから JSON 形式でコンテンツフラグメントバリエーションコンテンツを公開する。
   + CRUD のユースケース向けに、**AEM Assets HTTP API** を経由して AEM Assets への直接呼出しから JSON としてコンテンツフラグメントコンテンツ（すべてのバリエーション）を直接公開する。

## エクスペリエンスフラグメントのアーキテクチャ

![エクスペリエンスフラグメントのアーキテクチャ](./assets/experience-fragments-architecture.png)

+ **編集可能なテンプレートタイプ** および **AEM ページコンポーネントの実装**&#x200B;で定義される&#x200B;**編集可能なテンプレート**&#x200B;は、エクスペリエンスフラグメントの作成に使用できる許可された AEM コンポーネントを定義します。
+ **エクスペリエンスフラグメント**&#x200B;は、論理的なエクスペリエンスを表す編集可能なテンプレートのインスタンスです。
+ エクスペリエンスフラグメント&#x200B;**バリエーション**&#x200B;は編集可能テンプレートに準拠するものの、エクスペリエンス（コンテンツとデザイン）にバリエーションがあります。
+ エクスペリエンスフラグメントは、次の方法で公開／使用できます。
   + AEM エクスペリエンスフラグメントコンポーネントを使用して、AEM Sites（または AEM Screens）上でエクスペリエンスフラグメントを使用する。
   + **AEM Content Services** と API ページから、エクスペリエンスフラグメントのバリエーションコンテンツを JSON（埋め込み HTML 付き）として公開する。
   + エクスペリエンスフラグメントのバリエーションを&#x200B;**プレーン HTML** として直接公開する。
   + HTMLまたは JSON オファーのいずれかとして、エクスペリエンスフラグメントをし **Adobe Target** に書き出す。
   + AEM Sites は HTML オファーをネイティブでサポートしていますが、JSON オファーにはカスタム開発が必要です。

## コンテンツフラグメントのサポート資料

+ [コンテンツフラグメントユーザーガイド](https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=ja&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [Adobe Experience Manager as a Headless CMS の概要](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/introduction.html?lang=ja)
+ [AEM でのコンテンツフラグメントの使用](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=ja)
+ [AEM WCM コアコンポーネントのコンテンツフラグメントコンポーネント](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=ja)
+ [コンテンツフラグメントと AEM ヘッドレスの使用](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html?lang=ja)
+ [AEM コンテンツサービスの基本を学ぶ](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=ja)

## エクスペリエンスフラグメントのサポート資料

+ [エクスペリエンスフラグメントに関する Adobe ドキュメント](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=ja)
+ [AEM エクスペリエンスフラグメントについて](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=ja)
+ [AEM エクスペリエンスフラグメントの使用](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=ja)
+ [AEM エクスペリエンスフラグメントを Adobe Target で使用する](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
