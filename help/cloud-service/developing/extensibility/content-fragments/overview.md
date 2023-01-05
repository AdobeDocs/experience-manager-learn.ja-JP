---
title: AEMコンテンツフラグメントコンソールの拡張機能
description: AEMas a Cloud Serviceのコンテンツフラグメントコンソール拡張機能を構築してデプロイする方法について説明します。
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
kt: 11603
thumbnail: KT-11603.png
last-substantial-update: 2022-12-09T00:00:00Z
source-git-commit: fbc8c11841f5b5e04a99ba74fac6f01dc3e3a2da
workflow-type: tm+mt
source-wordcount: '742'
ht-degree: 4%

---


# AEM Content Fragments Console 拡張機能

[AEMコンテンツフラグメントコンソール](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html?lang=ja) 拡張機能は、次の 2 つの拡張ポイントを使用して追加できます。ボタン [コンテンツフラグメントコンソールの](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html?lang=ja) ヘッダーメニューまたはアクションバー。 拡張機能は、App Builder アプリとして実行される JavaScript で記述され、カスタム Web UI とサーバーレスのAdobe I/O Runtimeアクションを実装して、より集中的で長時間実行される作業を実行できます。

![AEM Content Fragments Console 拡張機能](./assets/overview/example.png){align="center"}

| 拡張機能のタイプ | 説明 | パラメーター |
| :--- | :--- | :--- |
| ヘッダーメニュー | 次の場合に表示されるヘッダーにボタンを追加します。 __ゼロ__ コンテンツフラグメントが選択されている。 | なし。 |
| アクションバー | アクションバーに、 __1 つ以上__ コンテンツフラグメントが選択されている。 | 選択したコンテンツフラグメントのパスの配列。 |

1 つのAEM Content Fragments Console 拡張機能に、0 または 1 つのヘッダーメニュー、0 または 1 つのアクションバー拡張タイプを含めることができます。 同じタイプの複数の拡張タイプが必要な場合は、複数のAEMコンテンツフラグメントコンソール拡張を作成する必要があります。

AEMコンテンツフラグメントコンソール拡張機能には、 [Adobe Developer Console プロジェクト](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#create-a-project-in-adobe-developer-console) および [App Builder アプリ](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation) の使用 `@adobe/aem-cf-admin-ui-ext-tpl` テンプレート。Adobe Developer Console プロジェクトに関連付けられています。

拡張機能の動作に基づいて、App Builder アプリを生成する際に次の機能から選択します。 オプションの組み合わせは、拡張機能で使用できます。

|  | 次にボタンを追加 [ヘッダーメニュー](./header-menu.md) | 次にボタンを追加 [アクションバー](./action-bar.md) | 表示 [モーダル](./modal.md) | 追加 [サーバーサイドハンドラー](./runtime-action.md) |
| ------------------------------------------ | :-----------------------: | :----------------------: | :--------: | :--------------------:  |
| コンテンツフラグメントが選択されていない場合に使用できます | ✔ |  |  |  |
| 1 つ以上のコンテンツフラグメントが選択されている場合に使用できます |  | ✔ |  |  |
| ユーザーからカスタム入力を収集します |  |  | ✔️ |  |
| ユーザーに対してカスタムフィードバックを表示 |  |  | ✔️ |  |
| AEMへの HTTP リクエストを呼び出します |  |  |  | ✔ |
| Adobe/サードパーティのサービスへの HTTP リクエストを呼び出す |  |  |  | ✔ |


## Adobe Developerドキュメント

Adobe Developerには、AEMコンテンツフラグメントコンソールの拡張機能に関する開発者向けの詳細が含まれています。 次を確認してください： [Adobe Developerの詳細な技術的詳細のコンテンツ](https://developer.adobe.com/uix/docs/).

## 拡張機能の開発

AEM as a Cloud Service用のAEMコンテンツフラグメントコンソール拡張機能を生成、開発およびデプロイする方法については、以下の手順に従います。

<div class="columns is-multiline">
    <!-- Create Adobe Developer Project -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create Adobe Developer Project">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./adobe-developer-console-project.md" title="Adobe Developer Project を作成" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/project/card.png" alt="Adobe Developer Project を作成">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">1.プロジェクトを作成する</p>
                    <p class="is-size-6">他のAdobe サービスへのアクセスを定義し、そのデプロイメントを管理するAdobe Developer Console プロジェクトを作成します。</p>
                    <a href="./adobe-developer-console-project.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Adobe Developerプロジェクトの作成</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Generate an Extension app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Generate an Extension app">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./app-initialization.md" title="拡張機能アプリの生成" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/initialize-app/card.png" alt="拡張機能アプリの初期化">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">2.拡張機能アプリを初期化する</p>
                    <p class="is-size-6">拡張機能の表示場所と実行する作業を定義するAEM Content Fragment Console 拡張機能アプリビルダーアプリを初期化します。</p>
                    <a href="./app-initialization.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">拡張機能アプリの初期化</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Extension registration -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Extension registration">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./extension-registration.md" title="拡張機能の登録" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/extension-registration/card.png" alt="拡張機能の登録">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">3.延長登録</p>
                    <p class="is-size-6">拡張機能をヘッダーメニューまたはアクションバー拡張タイプとしてAEMコンテンツフラグメントコンソールに登録します。</p>
                    <a href="./extension-registration.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">拡張機能の登録</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Header Menu -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Header menu">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./header-menu.md" title="ヘッダーメニュー" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/header-menu/card.png" alt="ヘッダーメニュー">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">4a. ヘッダーメニュー</p>
                    <p class="is-size-6">AEMコンテンツフラグメントコンソールのヘッダーメニュー拡張機能を作成する方法について説明します。</p>
                    <a href="./header-menu.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">ヘッダーメニューの拡張</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Action Bar -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Action Bar">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./action-bar.md" title="アクションバー" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/action-bar/card.png" alt="アクションバー">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">4b. アクションバー</p>
                    <p class="is-size-6">AEMコンテンツフラグメントコンソールのアクションバー拡張機能を作成する方法について説明します。</p>
                    <a href="./action-bar.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">アクションバーの拡張</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Modal -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Modal">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./modal.md" title="モーダル" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/modal/card.png" alt="モーダル">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">5.モーダル</p>
                    <p class="is-size-6">ユーザー向けのカスタムエクスペリエンスの作成に使用できる、拡張機能にカスタムモーダルを追加します。 モデルは、多くの場合、ユーザーからの入力を収集し、操作の結果を表示します。</p>
                    <a href="./modal.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">モーダルの追加</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Adobe I/O Runtime action -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adobe I/O Runtime action">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./runtime-action.md" title="Adobe I/O Runtime action" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/runtime-action/card.png" alt="Adobe I/O Runtime action">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">6.Adobe I/O Runtimeの行動</p>
                    <p class="is-size-6">拡張機能が呼び出してコンテンツフラグメントとAEMとやり取りし、カスタムビジネス操作を実行するためのサーバーレスAdobe I/O Runtimeアクションを追加します。</p>
                    <a href="./runtime-action.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Adobe I/O Runtimeアクションの追加</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Test -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Test">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./test.md" title="テスト" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/test/card.png" alt="テスト">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">7.テスト</p>
                    <p class="is-size-6">開発時に拡張機能をテストし、特別な URL を使用して QA または UAT テスト担当者に対して完了した拡張機能を共有します。</p>
                    <a href="./test.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">拡張機能のテスト</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Extension deployment -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Extension deployment">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./deploy.md" title="拡張機能のデプロイメント" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/deploy/card.png" alt="拡張機能のデプロイメント">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">8.実稼動のデプロイメント</p>
                    <p class="is-size-6">拡張機能をAdobe I/Oにデプロイして、AEMユーザーが使用できるようにします。 拡張機能は、更新や削除もできます。</p>
                    <a href="./deploy.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">実稼動環境にデプロイ</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
</div>

## 拡張の例

AEMコンテンツフラグメントコンソール拡張の例。

<div class="columns is-multiline">
    <!-- Bulk property update extension -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Bulk property update extension">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./example-extensions/bulk-property-update.md" title="プロパティ一括更新拡張機能" tabindex="-1">
                        <img class="is-bordered-r-small" src="./example-extensions/assets/bulk-property-update/card.png" alt="プロパティ一括更新拡張機能">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">プロパティ一括更新拡張機能</p>
                    <p class="is-size-6">選択したコンテンツフラグメントのプロパティを一括更新するアクションバー拡張機能の例を参照します。</p>
                    <a href="./example-extensions/bulk-property-update.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">拡張例を見る</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Bulk property update extension -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Image generation and upload to AEM extension">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./example-extensions/image-generation-and-image-upload.md" title="画像の生成とAEM拡張機能へのアップロード" tabindex="-1">
                        <img class="is-bordered-r-small" src="./example-extensions/assets/digital-image-generation/screenshot.png" alt="画像の生成とAEM拡張機能へのアップロード">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">画像の生成とAEM拡張機能へのアップロード</p>
                    <p class="is-size-6">OpenAI を使用して画像を生成し、AEMにアップロードして、選択したコンテンツフラグメントの画像プロパティを更新するアクションバー拡張機能の例を見てみましょう。</p>
                    <a href="./example-extensions/image-generation-and-image-upload.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">拡張例を見る</span>
                    </a>
                </div>
            </div>
        </div>
    </div>



</div>
