---
title: AEM ヘッドレス OpenAPI チュートリアル | コンテンツフラグメント配信
description: AEMで OpenAPI ベースのコンテンツフラグメント配信 API を使用してコンテンツを作成および公開する方法を説明するエンドツーエンドのチュートリアルです。
short-description: OpenAPI API を使用してコンテンツフラグメント配信を使用してAEM コンテンツを構築および公開し、ヘッドレスなCMS シナリオ用に外部アプリで使用する方法を示すチュートリアルです。
doc-type: Tutorial
mini-toc-levels: 1
jira: KT-6678
thumbnail: KT-6678.jpg
feature: Content Fragments
topic: Headless, Content Management
role: Developer
level: Beginner
last-substantial-update: 2022-10-17T00:00:00Z
duration: 54
exl-id: 1bb7c415-58f8-4f6c-a0bc-38bdbdb521cf
source-git-commit: f0b1b906e1ef04b53eca940f191e65d62a2e0bab
workflow-type: tm+mt
source-wordcount: '393'
ht-degree: 20%

---

# OpenAPI を使用したAEM コンテンツフラグメント配信の基本を学ぶ

このチュートリアルでは、OpenAPI を使用してAEMのコンテンツフラグメント配信を使用し、ヘッドレスなAEM シナリオで外部アプリで使用して、CMS コンテンツを構築および公開する方法を説明します。 これは、WKND チームと関連するメンバーの詳細を表示する React アプリの作成について考え、これらの概念を調べます。 チームとメンバーはAEM コンテンツフラグメントモデルを使用してモデル化され、OpenAPI を使用したAEM コンテンツフラグメント配信を使用して React アプリによって使用されます。

![WKND Teams アプリ ](./assets/overview/main.png)

このチュートリアルでは、以下のトピックを扱います。

* プロジェクト設定の作成
* データをモデル化するコンテンツフラグメントモデルの作成
* 以前に作成したモデルに基づいてコンテンツフラグメントを作成します。
* OpenAPI API ドキュメントの「試す」機能を備えたAEM コンテンツフラグメント配信を使用して、AEMのコンテンツフラグメントをクエリする方法を調べます
* サンプル React アプリからの OpenAPI API 呼び出しを使用したAEM コンテンツフラグメント配信を通じたコンテンツフラグメントデータの使用
* React アプリを強化して、ユニバーサルエディターで編集できるようにします

## 前提条件 {#prerequisites}

このチュートリアルに従うには、以下が必要です。

* AEM Sites as a Cloud Service
* HTML と JavaScript の基本的なスキル
* 以下のツールをローカルにインストールする必要があります。
   * [Node.js v22 以降 ](https://nodejs.org/ja/)
   * [Git](https://git-scm.com/)
   * IDE（例：[Microsoft® Visual Studio Code](https://code.visualstudio.com/)）

### AEM as a Cloud Service環境

このチュートリアルを完了するには、AEM as a Cloud Service環境への **AEM管理者** アクセス権を持つことをお勧めします。 **開発** 環境、**迅速な開発環境**、または **サンドボックスプログラム** 内の環境も使用できます。

## それでは、始めましょう。

[コンテンツフラグメントモデルの定義](1-content-fragment-models.md)からチュートリアルを開始します。

## GitHub プロジェクト

ソースコードとコンテンツパッケージは、[AEM ヘッドレスチュートリアル ](https://github.com/adobe/aem-tutorials)GitHub リポジトリに格納されています。

[`main` ブランチには ](https://github.com/adobe/aem-tutorials/tree/main/headless/open-api/basic) このチュートリアルの最終的なソースコードが含まれます。
各手順の最後にあるコードのスナップショットは、Git タグとして利用できます。

* 第 4 章の開始 – React アプリ：[`headless_open-api_basic`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic//headless/open-api/basic)
* 第 4 章の終わり – React アプリ：[`headless_open-api_basic_4-end`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_4-end//headless/open-api/basic)
* 第 5 章の終わり – ユニバーサルエディター：[`headless_open-api_basic_5-end`](https://github.com/adobe/aem-tutorials/tree/headless_open-api_basic_5-end//headless/open-api/basic)

チュートリアルまたはコードに問題が見つかった場合は、[GitHub イシュー](https://github.com/adobe/aem-tutorials/issues)を報告してください。
