---
title: AEM コンテンツフラグメント拡張機能
description: AEM as a Cloud Service コンテンツフラグメント拡張機能を作成してデプロイする方法を説明します
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
jira: KT-11603
thumbnail: KT-11603.png
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: 9164423b-a609-4bc5-9777-112d229ae748
duration: 388
source-git-commit: bb902089a709ab00853f4856d9bf42c18a5097e7
workflow-type: ht
source-wordcount: '773'
ht-degree: 100%

---

# AEM コンテンツフラグメントの拡張性

AEM コンテンツフラグメント UI は、コンテンツフラグメントの作成、管理および編集を管理するための強力で拡張可能な UI です。ニーズに合わせて UI をカスタマイズするために使用できる拡張ポイントがいくつかあります。拡張対象の UI によって、使用できる拡張ポイントは異なります。

## コンテンツフラグメントコンソールの拡張ポイント

AEM（Adobe Experience Manager）のコンテンツフラグメントコンソールは、コンテンツフラグメントを一元的に管理および整理するためのユーザーインターフェイスです。コンテンツフラグメントを作成、編集、公開および追跡するための包括的なツールおよび機能セットを提供するので、ユーザーは、様々なチャネルやタッチポイントにわたって構造化コンテンツを効率的に管理できます。

![コンテンツフラグメントコンソール](./assets/overview/cfc.png)

[AEM コンテンツフラグメントコンソール](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html?lang=ja)は、コンテンツフラグメントのリストと管理を行うための拡張可能な UI です。`@adobe/aem-cf-admin-ui-ext-tpl` Application Builder テンプレートを使用して、[AEM コンテンツフラグメントコンソール拡張機能を作成します](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation)。

次のコンテンツフラグメントコンソール拡張ポイントを使用できます。

<div class="columns is-multiline">
      <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Action bar">
        <div class="card" style="height: 100%">
          <div class="card-image">
            <figure class="image is-16by9">
              <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/" title="アクションバー" tabindex="-1" target="_blank" rel="referrer">
                <img class="is-bordered-r-small" src="./assets/overview/cfc-action-bar.png" alt="アクションバー">
              </a>
            </figure>
          </div>
          <div class="card-content is-padded-small">
            <div class="content">
              <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/" title="アクションバー" target="_blank" rel="referrer">アクションバー</a></p>
              <p class="is-size-6">1 つ以上のコンテンツフラグメントが選択されている場合のアクションをカスタマイズします。</p>
              <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/action-bar/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
 <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">ドキュメントを表示</span>
 </a>
            </div>
          </div>
        </div>
      </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Grid columns">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/grid-columns/" title="グリッド列" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfc-grid-columns.png" alt="グリッド列">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/grid-columns/" title="グリッド列" target="_blank" rel="referrer">グリッド列</a></p>
          <p class="is-size-6">コンテンツフラグメントリストに表示されるデータをカスタマイズします。</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/grid-columns/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
 <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">ドキュメントを表示</span>
 </a>
        </div>
      </div>
    </div>
  </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Header menu">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/header-menu/" title="ヘッダーメニュー" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfc-header-menu.png" alt="ヘッダーメニュー">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/header-menu/" title="ヘッダーメニュー" target="_blank" rel="referrer">ヘッダーメニュー</a></p>
          <p class="is-size-6">コンテンツフラグメントが選択されていない場合のアクションをカスタマイズします。</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/api/header-menu/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
 <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">ドキュメントを表示</span>
 </a>
        </div>
      </div>
    </div>
  </div>  
</div>

## コンテンツフラグメントエディターの拡張ポイント

AEM（Adobe Experience Manager）のコンテンツフラグメントエディターは、ユーザーがコンテンツフラグメントを作成、編集および管理できるようにするユーザーインターフェイスコンポーネントです。構造化コンテンツを操作するための視覚的に直感的でわかりやすい環境を提供するので、ユーザーは、コンテンツ要素を定義および整理したり、テンプレートを適用したり、バリエーションを管理したり、様々なチャネルでのコンテンツの表示をプレビューしたりできます。コンテンツフラグメントエディターにより、複数のデジタルエクスペリエンスにわたって簡単に配布および公開できる再利用可能なモジュール型コンテンツを効率的に作成できるようになります。

![コンテンツフラグメントエディター](./assets/overview/cfe.png)

AEM コンテンツフラグメントエディターは、コンテンツフラグメントを編集するための拡張可能な UI です。`@adobe/aem-cf-editor-ui-ext-tpl` Application Builder テンプレートを使用して、[AEM コンテンツフラグメントエディター拡張機能を作成します](https://developer.adobe.com/uix/docs/services/aem-cf-editor/code-generation/)。

次のコンテンツフラグメントエディター拡張ポイントを使用できます。

<div class="columns is-multiline">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Header menu">
      <div class="card" style="height: 100%">
        <div class="card-image">
          <figure class="image is-16by9">
            <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/header-menu" title="ヘッダーメニュー" tabindex="-1" target="_blank" rel="referrer">
              <img class="is-bordered-r-small" src="./assets/overview/cfe-header-menu.png" alt="ヘッダーメニュー">
            </a>
          </figure>
        </div>
        <div class="card-content is-padded-small">
          <div class="content">
            <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/header-menu/" title="ヘッダーメニュー" target="_blank" rel="referrer">ヘッダーメニュー</a></p>
            <p class="is-size-6">コンテンツフラグメントエディターのヘッダーメニューのアクションをカスタマイズします。</p>
            <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/header-menu" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
 <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">ドキュメントを表示</span>
 </a>
          </div>
        </div>
      </div>
    </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor toolbar">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/" title="リッチテキストエディターツールバー" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfe-rte-toolbar.png" alt="リッチテキストエディターツールバー">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/" title="リッチテキストエディターツールバー"  target="_blank" rel="referrer">リッチテキストエディターツールバー</a></p>
          <p class="is-size-6">コンテンツフラグメントエディターのリッチテキストエディター（RTE）にカスタムボタンを追加します。</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
 <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">ドキュメントを表示</span>
 </a>
        </div>
      </div>
    </div>
  </div>

<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor widgets">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/" title="リッチテキストエディターウィジェット" tabindex="-1"  target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfe-rte-widgets.png" alt="リッチテキストエディターウィジェット">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/" title="リッチテキストエディターウィジェット" target="_blank" rel="referrer">リッチテキストエディターウィジェット</a></p>
          <p class="is-size-6">キーストロークにバインドされる RTE のアクションをカスタマイズします。</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-widgets/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
 <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">ドキュメントを表示</span>
 </a>
        </div>
      </div>
    </div>
  </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor badges">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/" title="リッチテキストエディターバッジ" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfe-rte-badges.png" alt="リッチテキストエディターバッジ">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/ " title="リッチテキストエディターバッジ" target="_blank" rel="referrer">リッチテキストエディターバッジ</a></p>
          <p class="is-size-6">RTE 内の編集できないスタイル付きブロックをカスタマイズします。</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
 <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">ドキュメントを表示</span>
 </a>
        </div>
      </div>
    </div>
  </div>
</div>

## 拡張例

AEM UI 拡張性コード例のコレクションにようこそ。このリソースは、Adobe Experience Manager（AEM）ユーザーインターフェイスの拡張に関する実践的なデモとインサイトを提供するように設計されています。AEM の機能の強化を検討している開発者であるかどうかにかかわらず、これらのコード例は貴重な参考資料として役に立ちます。

<div class="columns is-multiline">
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Bulk property update">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/console-bulk-property-update.md" title="プロパティの一括更新" tabindex="-1">
            <img class="is-bordered-r-small" src="./assets/../examples/assets/bulk-property-update/card.png" alt="プロパティの一括更新">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/console-bulk-property-update.md" title="プロパティの一括更新">コンテンツフラグメントプロパティの一括更新</a></p>
          <p class="is-size-6">モーダルと Adobe I/O Runtime アクションを備えたコンテンツフラグメントコンソールアクションバー拡張機能です。</p>
          <a href="./examples/console-bulk-property-update.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
 <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">例を表示</span>
 </a>
        </div>
      </div>
    </div>
  </div>
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="OpenAI-based image generation and upload to AEM extension">
        <div class="card" style="height: 100%">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./examples/console-image-generation-and-image-upload.md" title="OpenAI ベースの画像生成と AEM へのアップロードを行う拡張機能" tabindex="-1">
                        <img class="is-bordered-r-small" src="./examples/assets/digital-image-generation/card.png" alt="OpenAI ベースの画像生成と AEM へのアップロードを行う拡張機能">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/console-image-generation-and-image-upload.md" title="OpenAI ベースの画像生成と AEM へのアップロードを行う拡張機能">OpenAPI 画像生成</a></p>
                    <p class="is-size-6">OpenAI を使用して画像を生成し、それを AEM にアップロードして、選択されたコンテンツフラグメントの画像プロパティを更新するアクションバー拡張機能の例について説明します。</p>
                    <a href="./examples/console-image-generation-and-image-upload.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
 <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">例を表示</span>
 </a>
                </div>
            </div>
        </div>
    </div>    
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Custom columns">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/custom-grid-columns.md" title="カスタム列" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/custom-grid-columns/card.png" alt="カスタム列">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/custom-grid-columns.md" title="カスタム列">カスタム列</a></p>
          <p class="is-size-6">コンテンツフラグメントコンソールにカスタム列を追加します。</p>
          <a href="./examples/custom-grid-columns.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
 <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">例を表示</span>
 </a>
        </div>
      </div>
    </div>
  </div>    
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Export to XML">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-export-to-xml.md" title="XML への書き出し" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/export-to-xml/card.png" alt="XML への書き出し">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-export-to-xml.md" title="XML への書き出し">XML への書き出し</a></p>
          <p class="is-size-6">コンテンツフラグメントエディターからコンテンツフラグメントを XML として書き出します。</p>
          <a href="./examples/editor-export-to-xml.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
 <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">例を表示</span>
 </a>
        </div>
      </div>
    </div>
  </div>    
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor toolbar button">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-rte-toolbar.md" title="リッチテキストエディターツールバーボタン" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/rte/rte-toolbar-card.png" alt="リッチテキストエディターツールバーボタン">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-rte-toolbar.md" title="リッチテキストエディターツールバーボタン">リッチテキストエディターツールバーボタン</a></p>
          <p class="is-size-6">コンテンツフラグメントエディターの RTE フィールドにカスタムツールバーボタンを追加します。</p>
          <a href="./examples/editor-rte-toolbar.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
 <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">例を表示</span>
 </a>
        </div>
      </div>
    </div>
  </div>   
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor Widget">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-rte-widget.md" title="リッチテキストエディターウィジェット" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/rte/rte-widget-card.png" alt="リッチテキストエディターウィジェット">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-rte-toolbar.md" title="リッチテキストエディターウィジェット">リッチテキストエディターウィジェット</a></p>
          <p class="is-size-6">コンテンツフラグメントエディターのリッチテキストエディターにウィジェットを追加します。</p>
          <a href="./examples/editor-rte-widget.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
 <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">例を表示</span>
 </a>
        </div>
      </div>
    </div>
  </div>   
  <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Rich Text Editor Badge">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-rte-badges.md" title="リッチテキストエディターバッジ" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/rte/rte-badge-card.png" alt="リッチテキストエディターバッジ">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-rte-badges.md" title="リッチテキストエディターバッジ">リッチテキストエディターバッジ</a></p>
          <p class="is-size-6">コンテンツフラグメントエディターのリッチテキストエディターにバッジを追加します。</p>
          <a href="./examples/editor-rte-badges.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
 <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">例を表示</span>
 </a>
        </div>
      </div>
    </div>
  </div>

<div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Custom fields">
    <div class="card" style="height: 100%">
      <div class="card-image">
        <figure class="image is-16by9">
          <a href="./examples/editor-custom-field.md" title="カスタムフィールド" tabindex="-1">
            <img class="is-bordered-r-small" src="https://video.tv.adobe.com/v/3427585?format=jpeg" alt="カスタムフィールド">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-custom-field.md" title="カスタムフィールド">カスタムフィールド</a></p>
          <p class="is-size-6">カスタムコンテンツフラグメントフィールドを作成します。</p>
          <a href="./examples/editor-custom-field.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">例を表示</span>
          </a>
        </div>
      </div>
    </div>
  </div> 
</div>
