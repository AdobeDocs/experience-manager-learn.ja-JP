---
title: AEM Content Fragments 拡張機能
description: AEMas a Cloud Serviceのコンテンツフラグメント拡張機能を構築してデプロイする方法について説明します。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
kt: 11603
thumbnail: KT-11603.png
last-substantial-update: 2023-06-02T00:00:00Z
source-git-commit: 82df468bc9a5f83133adbd7aa7332bb5c21a695c
workflow-type: tm+mt
source-wordcount: '864'
ht-degree: 7%

---

# AEM Content Fragments 拡張機能

AEM Content Fragments UI は、コンテンツフラグメントの作成、管理、編集を管理するための強力な拡張可能な UI です。 ニーズに合わせて UI をカスタマイズするために、いくつかの拡張ポイントを使用できます。 拡張する UI に応じて、様々な拡張ポイントを使用できます。

## コンテンツフラグメントコンソール拡張ポイント

AEM(Adobe Experience Manager) のコンテンツフラグメントコンソールは、コンテンツフラグメントを一元管理し、整理するためのユーザーインターフェイスです。 コンテンツフラグメントの作成、編集、公開、追跡のための包括的なツールと機能のセットを提供し、様々なチャネルやタッチポイントをまたいで構造化コンテンツを効率的に管理できます。

![コンテンツフラグメントコンソール](./assets/overview/cfc.png)

[AEMコンテンツフラグメントコンソール](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html?lang=ja) は、コンテンツフラグメントのリストと管理を行うための拡張可能な UI です。 [AEMコンテンツフラグメントコンソールの拡張機能が作成される](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation) の使用 `@adobe/aem-cf-admin-ui-ext-tpl` App Builder テンプレート。

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

AEM(Adobe Experience Manager) のコンテンツフラグメントエディターは、コンテンツフラグメントの作成、編集、管理をユーザーがおこなうためのユーザーインターフェイスコンポーネントです。 構造化されたコンテンツを操作するための視覚的に直感的で使いやすい環境を提供し、コンテンツ要素の定義と整理、テンプレートの適用、バリエーションの管理、様々なチャネルでのコンテンツの表示方法のプレビューをおこなえます。 コンテンツフラグメントエディターは、複数のデジタルエクスペリエンスに簡単に配布および公開できる、再利用可能でモジュラー型のコンテンツを作成するプロセスを合理化します。

![コンテンツフラグメントエディター](./assets/overview/cfe.png)

AEM Content Fragments Editor は、コンテンツフラグメントを編集するための拡張可能な UI です。 [AEMコンテンツフラグメントエディターの拡張機能が作成される](https://developer.adobe.com/uix/docs/services/aem-cf-editor/code-generation/) の使用 `@adobe/aem-cf-editor-ui-ext-tpl` App Builder テンプレート。

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
            <p class="is-size-6">コンテンツフラグメントエディターのヘッダーメニューでアクションをカスタマイズします。</p>
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
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/" title="リッチテキストエディターのツールバー" tabindex="-1" target="_blank" rel="referrer">
            <img class="is-bordered-r-small" src="./assets/overview/cfe-rte-toolbar.png" alt="リッチテキストエディターのツールバー">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-toolbar/" title="リッチテキストエディターのツールバー"  target="_blank" rel="referrer">リッチテキストエディターのツールバー</a></p>
          <p class="is-size-6">コンテンツフラグメントエディターのリッチテキストエディター (RTE) にカスタムボタンを追加します。</p>
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
          <p class="is-size-6">RTE でキーストロークにバインドされるアクションをカスタマイズします。</p>
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
          <p class="is-size-6">RTE 内の編集不可能なスタイルブロックをカスタマイズします。</p>
          <a href="https://developer.adobe.com/uix/docs/services/aem-cf-editor/api/rte-badges/" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" target="_blank" rel="referrer">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">ドキュメントを表示</span>
          </a>
        </div>
      </div>
    </div>
  </div>
</div>

## 拡張の例

AEM UI 拡張機能コード例のコレクションへようこそ。 このリソースは、Adobe Experience Manager(AEM) ユーザーインターフェイスの拡張に関する実際のデモとインサイトを提供するように設計されています。 AEMの機能を強化したい開発者であれ、これらのコード例は有益な参考になります。

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
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/console-bulk-property-update.md" title="プロパティの一括更新">コンテンツフラグメントの一括更新</a></p>
          <p class="is-size-6">モーダルアクションとAdobe I/O Runtimeアクションを含むコンテンツフラグメントコンソールアクションバー拡張機能。</p>
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
                    <a href="./examples/console-image-generation-and-image-upload.md" title="OpenAI ベースの画像生成とAEM拡張機能へのアップロード" tabindex="-1">
                        <img class="is-bordered-r-small" src="./examples/assets/digital-image-generation/card.png" alt="OpenAI ベースの画像生成とAEM拡張機能へのアップロード">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/console-image-generation-and-image-upload.md" title="OpenAI ベースの画像生成とAEM拡張機能へのアップロード">OpenAPI 画像生成</a></p>
                    <p class="is-size-6">OpenAI を使用して画像を生成し、AEMにアップロードして、選択したコンテンツフラグメントの画像プロパティを更新するアクションバー拡張機能の例を見てみましょう。</p>
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
          <a href="./examples/editor-export-to-xml.md" title="XML に書き出し" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/export-to-xml/card.png" alt="XML に書き出し">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-export-to-xml.md" title="XML に書き出し">XML に書き出し</a></p>
          <p class="is-size-6">コンテンツフラグメントエディターからコンテンツフラグメントを XML 形式で書き出します。</p>
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
          <a href="./examples/editor-rte-toolbar.md" title="リッチテキストエディターのツールバーボタン" tabindex="-1">
            <img class="is-bordered-r-small" src="./examples/assets/rte-toolbar/card.png" alt="リッチテキストエディターのツールバーボタン">
          </a>
        </figure>
      </div>
      <div class="card-content is-padded-small">
        <div class="content">
          <p class="headline is-size-6 has-text-weight-bold"><a href="./examples/editor-rte-toolbar.md" title="リッチテキストエディターのツールバーボタン">リッチテキストエディターのツールバーボタン</a></p>
          <p class="is-size-6">コンテンツフラグメントエディターで、カスタムツールバーボタンを RTE フィールドに追加します。</p>
          <a href="./examples/editor-rte-toolbar.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
            <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">例を表示</span>
          </a>
        </div>
      </div>
    </div>
  </div>   
</div>
