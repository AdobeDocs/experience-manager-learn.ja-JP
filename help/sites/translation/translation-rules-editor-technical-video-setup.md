---
title: AEMでの翻訳ルールの設定
description: 翻訳設定UIを使用すると、AEM Sitesでコンテンツを翻訳するルールを管理できます。 このビデオでは、カスタムコンポーネントの新しい翻訳ルールの作成について詳しく説明します。
feature: 言語コピー
topics: localization, content-architecture
audience: developer, administrator
doc-type: technical video
activity: setup
version: 6.4, 6.5
topic: ローカリゼーション
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 7%

---


# 翻訳ルールの設定{#set-up-translation-rules-in-aem}

翻訳設定UIを使用すると、AEM Sitesでコンテンツを翻訳するルールを管理できます。 このビデオでは、カスタムコンポーネントの新しい翻訳ルールの作成について詳しく説明します。

>[!NOTE]
>
> 以下のビデオはAEM 6.3で録画されました。AEM 6.4以降では、翻訳ルールXMLファイルを保存するための新しいリポジトリ構造を導入しました。 AEM 6.4以降で翻訳設定UIを使用する場合、ルールは`/conf/global/settings/translation/rules/translation_rules.xml`に保存されます。 詳しくは、[翻訳するコンテンツの識別](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html)を参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/18135/?quality=9&learn=on)

翻訳ルールは、翻訳用に抽出するAEM内のコンテンツを識別します。 標準の翻訳ルールでは、テキストコンポーネントや画像コンポーネントの代替テキストなど、一般的な使用例を扱っています。 プロジェクトの翻訳要件に応じて、追加のルールが必要な場合があります。 一般的な翻訳ルールでは、ユーザーは次の項目を指定できます。

1. パスやリソースタイプに基づいて翻訳する必要があるプロパティ
2. 翻訳しないプロパティのフィルター
3. 翻訳する必要がある参照コンテンツ（画像またはコンテンツフラグメント）

翻訳XMLファイルを更新する翻訳ルールエディター。 翻訳設定UIを使用すると、XMLを直接編集する際の様々な翻訳ルールの管理や入力ミスに対する対策が容易になります。

翻訳設定UIにアクセスします。

* **[!UICONTROL AEM Start Menu] / [!UICONTROL Tools] / [!UICONTROL General] / [[!UICONTROL Translation Configuration]](http://localhost:4502/libs/cq/translation/translationrules/contexts.html)**

## AEM 6.3より前{#prior-to-aem}

以前のAEMバージョンの翻訳ルールは、翻訳ワークフローの下にあるXMLファイルを編集することで手動で更新されていました。`/etc/workflow/models/translation/translation_rules.xml`.

## その他のリソース {#additional-resources}

* [翻訳するコンテンツの特定](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-rules.html)
* [多言語サイトのコンテンツの翻訳](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html)
* [https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-manage.html)
* [翻訳のベストプラクティス](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/tc-bp.html)
