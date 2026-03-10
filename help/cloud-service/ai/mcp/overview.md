---
title: AEMの MCP サーバー
description: 好みの AI を利用した IDE またはチャットベースのアプリケーションからAEM Model Context Protocol （MCP）サーバーを使用して、AEM コンテンツの作業を合理化および高速化する方法について説明します。
version: Experience Manager as a Cloud Service
role: Leader, User, Developer
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2026-03-04T00:00:00Z
jira: KT-20473
exl-id: 7f2e4e37-6440-423e-9ba9-9228fe03600b
source-git-commit: ac44a73d2b63dba5292393730c712aec68ddea6c
workflow-type: tm+mt
source-wordcount: '877'
ht-degree: 0%

---

# AEMの MCP サーバー

好みの AI を利用した IDE やチャットベースのアプリケーションからAEM _Model Context Protocol （MCP）サーバー_ を使用して、AEM コンテンツの作業を合理化および高速化する方法を説明します。 低レベルの API コードを記述したり、AEM UI 内を移動したりするのではなく、自然言語で内容を説明します。

## AEM MCP サーバーの一覧

すべてのAEM MCP サーバーは、`https://mcp.adobeaemcloud.com/adobe/mcp/` の下で利用できます。 詳しくは、[AEM as a Cloud Serviceでの MCP の使用 ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/using-mcp-with-aem-as-a-cloud-service) を参照してください。

- **コンテンツ** （`/content`） – ページ、フラグメント、アセットを作成、読み取り、更新、削除するためのフルアクセス権。
- **コンテンツ（読み取り専用）** （`/content-readonly`） – ページ、フラグメント、アセットをリスト表示および取得する読み取り専用（変更なし）。
- **Cloud Manager** （`/cloudmanager`） - Adobe Cloud Managerのプログラム、環境、リポジトリおよびパイプラインを管理します。

>[!TIP]
>
>各サーバーが公開するツールは、時間の経過と共に変化する可能性があります。 利用可能な機能を確認するには、AI に依頼してすべてのAEM MCP ツール（`List all AEM MCP tools available from this server and describe what they do` など）を一覧表示するか、IDE で `tools/list` プロンプトを入力します。

## MCP サーバーの使用パターン

AEM MCP サーバーの使用を開始する前に、MCP サーバーの 2 つの主な使用パターンを理解しておきましょう。

- **人間中心** – あなたは運転席にいます。 ユーザーが尋ねると、AI は IDE でツールを提案または実行します。
- **Agentic** - Agentic アプリケーション（エージェントまたはサブエージェント）は、サーバーを単独で呼び出し、ツールを選択し、人間の入力をほとんど必要としない目標に向かって作業します。

次に、これらの 2 つの使用パターンを比較します。

| 項目 | 人間中心 | 無形成 |
| ------ | ------------- | ------- |
| **誰がアクションを推進** | あなた。 <br> AI は、IDE またはチャットベースのアプリケーションでツールを提案または実行します。 | AI。 <br> 使用するツールを選択し、最小限のガイダンスで継続します。 |
| **決定権** | お前は支配し続ける。 各手順を承認またはトリガーします。 | AI の方が自由度が高い。 影響の大きいアクションには、ガードレールや承認が必要になる場合があります。 |
| **一般的な使用パターン** | **開発者ごとに**、独自の IDE またはチャットベースのアプリケーションから、セッションごとに 1 つの開発者として使用します。これは、毎日の開発作業に適しています。 | 多くのユーザーやエージェントのための共有サービスおよびゲートウェイとして、エージェンティックアプリケーションを介して **共有**。 |
| **最適な対象** | コンテンツのレビュー、ガイド付き更新、探索、ループ内でのタスクの繰り返し。 | 最小限の介入でシステムを実行すべき効率的なワークフロー、バッチジョブ、パイプライン、目標。 |

### エージェントシステムで MCP を使用する場合

MCP サーバーは、インタラクティブな UX と人間による監視を備えた **人間が操作する MCP クライアント** 向けに設計されています。 MCP ツール仕様では、ツールの呼び出しを承認または拒否できる _ループ内の人間_ を推奨しています。

MCP サーバーをエージェンティックまたは自律システムで使用する場合は、それを別の互換性層として扱います。 許可リストに加える **promps** _、__または_ ルーティングロジック _内のツール名はハードコードしないでください_。 MCP では、_ツール名_ はプログラム識別子であり、_説明_ は LLM のモデル向けのヒントです。 機能または説明に基づくプロンプトおよび選択を優先します。

`tools/list` を介してランタイム検出を実装し、ツールリストの変更（`notifications/tools/list_changed`）を処理し、オンボーディングとバージョン管理に関して MCP サーバープロバイダーと連携します（プロトコルベースラインを超える安定性の保証が必要な場合）。

## MCP エンティティとそのマッピング

MCP は、**host**、**client**、**server** の 3 つのエンティティを中心に構築されます。 [MCP 仕様 ](https://modelcontextprotocol.io/docs/getting-started/intro) では、正式に定義されています。 ただし、次の表では、AEM MCP サーバーを使用する際の、各サーバーの概要とそのマッピングについて説明します。

| コンポーネント | 標準定義 | AEM MCP サーバーを使用する場合 |
| --------- | ------------------- | ---------------- |
| **主催者** | すべてを実行し、コンテキストを収集し、AI と対話し、権限を処理し、クライアントを作成するアプリです。 | **IDE** （カーソル）またはチャットベースのアプリケーションがホストです。 MCP クライアントを実行し、セッションで使用できるツールとサーバーを決定します。 |
| **クライアント** | ホストから 1 つのサーバーへの 1 つの接続。 メッセージを送受信し、そのサーバーのアクセスを他のサーバーとは別に保持します。 | **MCP クライアント** は、IDE またはチャットベースのアプリケーションに存在します。 AEM Content MCP Server を settings に追加すると、IDE またはチャットベースのアプリケーションによって、そのサーバーと通信するクライアントが作成されます。 プロンプトとツール呼び出しは、このクライアントを経由します。 |
| **サーバー** | MCP 経由でツール、データおよびプロンプトを公開するサービス。 コンピューター上で実行することも、リモートで実行することもできます。 | Adobeがホストする **AEM MCP サーバー** は、ページ、コンテンツフラグメント、アセットを作成、読み取り、更新、削除するためのツールを提供するので、IDE またはチャットベースのアプリケーションの AI がAEM環境と連携できます。 |

簡単に言えば、**ホスト** は IDE またはチャットベースのアプリケーションです。**クライアント** は、IDE またはチャットベースのアプリケーションからAEMへの接続です。**サーバー** は、AdobeがホストするAEM MCP サーバーで、この作業を行います。

## セットアップ

AEM MCP サーバーは、MCP 互換アプリケーションの定義済みセットで動作するように設計されています。
好みの IDE またはチャットベースのアプリケーションでAEM MCP サーバーを設定するには、詳しくは [ サポートされる MCP アプリケーション ](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/mcp-support/using-mcp-with-aem-as-a-cloud-service#supported-mcp-applications) を参照してください。

## ユースケース

<!-- CARDS
{target = _self}

* ./accelerate-content-operations-with-aem-mcp-server.md    
  {title = Accelerate Content Operations with AEM MCP Server}
  {description = Learn how to use the AEM Content MCP Server from Cursor IDE to streamline and accelerate your AEM content work.}
  {image = ../assets/content-mcp-server/update-adventure-price-prompt-response.png}
  {cta = Learn Content MCP Server}

* ./cloud-manager.md
  {title = Cloud Manager MCP Server}
  {description = Learn how to use the AEM Cloud Manager MCP Server from Cursor IDE to streamline and accelerate your AEM cloud manager work.}
  {image = ../assets/cm-mcp-server/start-pipeline.png}
  {cta = Learn Cloud Manager MCP Server}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Accelerate Content Operations with AEM MCP Server">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./accelerate-content-operations-with-aem-mcp-server.md" title="AEM MCP Server を使用したコンテンツオペレーションの高速化" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/content-mcp-server/update-adventure-price-prompt-response.png" alt="AEM MCP Server を使用したコンテンツオペレーションの高速化"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./accelerate-content-operations-with-aem-mcp-server.md" target="_self" rel="referrer" title="AEM MCP Server を使用したコンテンツオペレーションの高速化">AEM MCP サーバーによるコンテンツ操作の高速化 </a>
                    </p>
                    <p class="is-size-6">Cursor IDE からAEM Content MCP Server を使用して、AEM コンテンツの作業を合理化および高速化する方法を説明します。</p>
                </div>
                <a href="./accelerate-content-operations-with-aem-mcp-server.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Content MCP サーバーについて </span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Cloud Manager MCP Server">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./cloud-manager.md" title="Cloud Manager MCP サーバー" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/cm-mcp-server/start-pipeline.png" alt="Cloud Manager MCP サーバー"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./cloud-manager.md" target="_self" rel="referrer" title="Cloud Manager MCP サーバー">Cloud Manager MCP サーバー </a>
                    </p>
                    <p class="is-size-6">Cursor IDE のAEM Cloud Manager MCP サーバーを使用して、AEM Cloud Manager の作業を合理化および高速化する方法について説明します。</p>
                </div>
                <a href="./cloud-manager.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Cloud Manager MCP サーバーについて </span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
