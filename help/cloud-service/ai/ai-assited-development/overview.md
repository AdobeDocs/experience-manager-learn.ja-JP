---
title: AIを活用した開発
description: AIを活用したIDEまたはコーディングエージェントと、AGENTS.md、Agent Skills、MCP サーバーを使用して、AEM as a Cloud Service上のプロジェクト用に高品質で本番環境に対応したコードを作成するAI支援の開発について説明します。
version: Experience Manager as a Cloud Service
feature: Developer Tools
role: Developer, Architect
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2026-04-24T00:00:00Z
jira: KT-20899
thumbnail: KT-20899.pngKT-20899
source-git-commit: e3ef450cfe9005ba940ff1897c216681654341b3
workflow-type: tm+mt
source-wordcount: '906'
ht-degree: 0%

---


# AIを活用した開発

AIを活用した開発では、AIを活用したIDEまたはコーディングエージェントと`AGENTS.md`、エージェントスキル、MCP サーバーを使用して、AEM as a Cloud Serviceプロジェクト用の高品質で本番環境での使用に対応したコードを作成します。

[Cursor](https://www.cursor.com/)、[GitHub Copilot in Visual Studio Code](https://code.visualstudio.com/docs/copilot/overview)、[Claude Code](https://code.claude.com/docs/en/overview)などのツール、および類似のAIを活用したIDEやコーディングエージェントは、いくつかの重要な方法で役立ちます。

- **より高速な反復**：目的の機能または変更を説明する自然言語プロンプトからコードを生成またはリファクタリングします。
- **学習ガイド**：使い慣れないコードパス、設定、概念、またはベストプラクティスを説明します。

ただし、これらのメリットは、コーディングエージェント _が利用できる_ コンテキストに大きく依存します。 一般的なトレーニングデータと1つのリポジトリスナップショットは、実稼動対応のAEM コードを確実に生成するには&#x200B;_不十分_&#x200B;であることが多くあります。

## AIだけでは不十分な理由

AI モデルは、適切なコンテキストがなければ（AIを活用したIDEまたはコーディングエージェントを介して）、次のことが可能になります。

- **Hallucinate APIまたはライフサイクル**:AEM as a Cloud Serviceのベストプラクティスまたは最新の機能に合わないコードまたは構成を提案します。
- **手続き手順を見逃す**: コードリポジトリまたはトレーニングデータに表示されない必要な手順を省略します。
- **プロジェクト標準からのドリフト**: コンポーネント、OSGi サービス、ワークフロー、またはDispatcher設定の確立されたパターンを無視します。

このギャップは、_構造化コンテキスト_ （エージェントスキルとAGENTS.md）と&#x200B;_ランタイム可視化_ （MCP サーバー）が、AI支援による開発&#x200B;_生産性_&#x200B;と&#x200B;_信頼性_&#x200B;を実現するために不可欠になる場所です。

## AdobeのAIを活用した制作

AEM as a Cloud Service プロジェクトの場合、Adobeには次の機能があります。

- Agent Skills and AGENTS.md via [AI コーディングエージェントのAdobe Skills](https://github.com/adobe/skills)
- [Software Distribution](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=mcp*&1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&1_group.propertyvalues.operation=equals&1_group.propertyvalues.0_values=software-type%3Atooling&orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&orderby.sort=desc&layout=list&p.offset=0&p.limit=3) ポータル経由のAEM SDKおよびローカル Dispatcher用のローカル MCP サーバー
- IDEまたはチャットアプリケーションのコンテンツおよびCloud Manager ワークフロー用にAdobeでホストされたAEM MCP サーバー – AEMの[MCP サーバー](../mcp/overview.md)を参照

次の節では、各項目の概要を説明します。 このページの最後にある&#x200B;**設定**&#x200B;および&#x200B;**ユースケース**&#x200B;のセクションを使用して、AIを活用した開発のインストールとウォークスルーを行います。

## エージェントスキルとは

エージェントのスキルは&#x200B;_プロシージャの知識または専門知識_&#x200B;であり、コーディング エージェント _が確実に実際の作業を実行するのに役立ちます_。 詳しくは、[ エージェントスキル ](https://agentskills.io)を参照してください。

AEM as a Cloud Service プロジェクトの場合、エージェントスキルは、[AI コーディングエージェントのAdobe スキル ](https://github.com/adobe/skills) リポジトリで利用できます。

## AGENTS.mdとは

AGENTS.mdは、コーディングエージェント _がプロジェクトで作業するのに役立つ_ コンテキストと手順&#x200B;_を提供します_。 詳しくは、[AGENTS.md](https://agents.md/)を参照してください。

AEM as a Cloud Service プロジェクトの場合、`ensure-agents-md` ブートストラップスキルは&#x200B;**AGENTS.md**&#x200B;をリポジトリルート **に作成します。このスキルが見つからない場合は**。 このスキルは、プロジェクト（ルート `pom.xml`やモジュールなど）を検査し、静的なファイルを使用する代わりに、カスタマイズされたガイダンスを生成します。 **AGENTS.md**&#x200B;が既に存在する場合、**not**&#x200B;は上書きされます。

ファイルが存在する場合は、ファイルを編集して、チームまたは組織のベストプラクティスのコンテキストと手順を追加できます。 また、スキルは&#x200B;**AGENTS.md**&#x200B;を参照する&#x200B;**CLAUDE.md**&#x200B;を作成して、Claude ベースのツールが同じガイダンスを受け取るようにすることもできます。

## MCP サーバーとは

MCP サーバーは、デバッグ、検査、実行、変更の検証などのアクションをサポートする[ モデル コンテキスト プロトコル ](https://modelcontextprotocol.io/)を通じて、ツールとデータをコーディング エージェントに公開します。 MCP サーバーは、ワークステーション （**ローカル**）またはホストされたサービス （**リモート**）として実行できます。

AEM SDKおよびDispatcherに対する&#x200B;**ローカル開発**&#x200B;の場合、[Software Distribution](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=mcp*&1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&1_group.propertyvalues.operation=equals&1_group.propertyvalues.0_values=software-type%3Atooling&orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&orderby.sort=desc&layout=list&p.offset=0&p.limit=3) ポータルから&#x200B;**ローカル MCP サーバー**&#x200B;をインストールします。

- **AEM クイックスタート ローカル MCP サーバー**: トラブルシューティングと開発をサポートするために、ローカル AEM SDK インスタンスからライブ ランタイム データを公開します。 詳しくは、[AEM Quickstart MCP Server](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/local-development-with-ai-tools#aem-quickstart-mcp-server)を参照してください。
- **Dispatcher ローカル MCP サーバー**: ローカル Dispatcher インスタンスのランタイム検証とインスペクションを有効にします。 詳しくは、[Dispatcher MCP Server](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/local-development-with-ai-tools#dispatcher-mcp-server)を参照してください。

AdobeでホストされているAEM MCP サーバー（content、read-only content、Cloud Managerなど）については、「[AEMのMCP サーバー](../mcp/overview.md)」を参照してください。

## セットアップ

<!-- 
CARDS
{target = _self}

* ./setup/agent-skills.md
    {title = Set up AEM Agent Skills}
    {description = Learn how to set up AEM Agent Skills for AI-assisted development.}
    {image = ./assets/agent-skills/select-aem-agent-skills-to-install.png}
    {cta = Install AEM Agent Skills}

-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Set up AEM Agent Skills">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./setup/agent-skills.md" title="AEM エージェントスキルの設定" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/agent-skills/select-aem-agent-skills-to-install.png" alt="AEM エージェントスキルの設定"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./setup/agent-skills.md" target="_self" rel="referrer" title="AEM エージェントスキルの設定">AEM Agent Skillsの設定</a>
                    </p>
                    <p class="is-size-6">AIを活用した開発にAEM Agent Skillsを設定する方法を説明します。</p>
                </div>
                <a href="./setup/agent-skills.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">AEM Agent Skillsのインストール </span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## ユースケース

<!-- 
CARDS
{target = _self}

* ./use-cases/component-development.md    
    {title = Create AEM Component with AI-assisted development}
    {description = Learn how to use AI-assisted development to develop AEM components.}
    {image = ./assets/component-development/review-generated-code.png}
    {cta = Create AEM Component}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create AEM Component with AI-assisted development">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./use-cases/component-development.md" title="AIを活用した開発でAEM コンポーネントを作成する" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="./assets/component-development/review-generated-code.png" alt="AIを活用した開発でAEM コンポーネントを作成する"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./use-cases/component-development.md" target="_self" rel="referrer" title="AIを活用した開発でAEM コンポーネントを作成する">AIを活用した開発でAEM コンポーネントを作成</a>
                    </p>
                    <p class="is-size-6">AIを活用した開発を使用して、AEMコンポーネントを開発する方法を説明します。</p>
                </div>
                <a href="./use-cases/component-development.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">AEM コンポーネントの作成</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->

## その他のリソース

- [AI ツールを使用したローカル開発](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/local-development-with-ai-tools)

- [AI コーディングエージェント向けAdobeのスキル](https://github.com/adobe/skills)

- [AGENTS.md](https://agents.md/)

- [エージェントスキル](https://agentskills.io/home)